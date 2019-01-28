

## script to generate tests for the nbi .net framework for testing

          --generate test cases for measure generation

          WITH CTE_TestCase AS (
              SELECT [test/@name] = 'test name'
                  ,[test/system-under-test/query] = ''
                  ,[test/assert/equalTo/column/@connectionString] = '@Assert-SSAS'
                  ,[test/assert/equalTo/query] = ''
          ), CTE_Column ([test/assert/equalTo/column/@index], [test/assert/equalTo/column/@role], [test/assert/equalTo/column/@type], [test/assert/equalTo/column/@tolerance]) AS (
              SELECT 0, 'value', 'numeric', 0001
          )

          SELECT [test/@name]
              , [test/@uid] = 0
              , [test/system-under-test/query]
              , [test/assert/equalTo/column/@index]
              , [test/assert/equalTo/column/@role]
              , [test/assert/equalTo/column/@type]
              , [test/assert/equalTo/column/@tolerance]
              , [test/assert/equalTo/column/@connectionString]
              , [test/assert/equalTo/query]
          FROM CTE_TestCase AS t CROSS JOIN CTE_Column AS c FOR XML PATH('')


          --this is to generate a test case number for something like a measures validation
          USE QA_DBO --this is whatever the qa table is
          SELECT NEXT VALUE FOR QA_DBO.dbo.Test_case_uid AS TestId



          -- ETL Mapping dcoument is persisted in your server name
          USE QA_dbo --point to your qa dob 
          GO

          DECLARE @vMaxUpdated DATETIME	
            ,@TableName VARCHAR(250) = 'Program'
            ,@TestCaseNumber INT = 1405
              ,@TestID INT
          ----------- no need to update code below -----------------

          IF Object_ID('tempdb..#TableSSAS_DIM') IS NOT NULL
            DROP TABLE #TableETLMappingCMS 

          SELECT @vMaxUpdated = MAX(DocumentVersionDate) FROM QA_Validations.dbo.SSAS_dim;
          SELECT @TestID = NEXT VALUE FOR QA_Validations.dbo.Test_case_uid;

          SELECT DISTINCT SSASTableName		
          INTO #TableSSAS_DIM
          FROM QA_Validations.dbo.SSAS_dim 
          WHERE ColumnsRequired_in_AS IS NOT NULL
            AND DocumentVersionDate = @vMaxUpdated
            AND SSASTableName = @TableName AND HierarchyRequired IS NULL
          (
          SELECT REPLACE(REPLACE(k, '[[', '<item>'), ']]', '</item>')
             FROM (	
          SELECT DISTINCT CONCAT('<test name="In dimension ''', SSASTableName, ''', we find at least this list of members. BNG-', @TestCaseNumber,'" uid="',@TestID ,'">    
              <system-under-test>
                <structure>
                  <hierarchies dimension="Date" perspective="Model" connectionString="@SSAS-system-under-test"/>
                </structure>
              </system-under-test>
              <assert>      
               <equivalentTo>	  ',

              ( SELECT CONCAT('[[', FriendlyName, ']]') FriendlyName
                     FROM QA_Validations.dbo.SSAS_dim sub
                    WHERE m.SSASTableName = sub.SSASTableName          
                AND sub.ColumnsRequired_in_AS IS NOT NULL
                AND sub.HierarchyRequired IS NULL
                AND sub.DocumentVersionDate = @vMaxUpdated
              ORDER BY FriendlyName
                      FOR XML PATH('') 
                 ),
            '</equivalentTo>
             </assert>
             </test>  ')k
          FROM #TableSSAS_DIM m)x )
