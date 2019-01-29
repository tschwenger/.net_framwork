# generate a test script for a model in the dimension


        -- ETL Mapping dcoument is persisted in PDX1BIIS1D
        USE QA_DBO  
        GO

        DECLARE @vMaxUpdated DATETIME	
          ,@TableName VARCHAR(250) = 'Company' --use the name of the ssas table to test
          ,@TestCaseNumber INT = 1249  --this is the bng test issue for ssas validation
            ,@TestID INT
        ----------- no need to update code below -----------------

        IF Object_ID('tempdb..#TableSSAS_DIM') IS NOT NULL
          DROP TABLE #TableETLMappingCMS 

        SELECT @vMaxUpdated = MAX(DocumentVersionDate) FROM QA_Validations.dbo.SSAS_dim;
        SELECT @TestID = NEXT VALUE FOR QA_Validations.dbo.Test_case_uid;

        SELECT DISTINCT SSASTableName		
        INTO #TableSSAS_DIM
        FROM QA_DBO.dbo.SSAS_dim 
        WHERE ColumnsRequired_in_AS IS NOT NULL
          AND DocumentVersionDate = @vMaxUpdated
          AND SSASTableName = @TableName--'Student' 
          AND HierarchyRequired IS NULL

        SELECT DISTINCT CONCAT('<test name="In dimension ''', SSASTableName, ''', we find at least this list of members. BNG-', @TestCaseNumber,'" uid="',@TestID ,'">    
            <system-under-test>
              <structure>
                <hierarchies dimension="' ,SSASTableName, '" perspective="Model"/>
              </structure>
            </system-under-test>
            <assert>      
             <equivalentTo>	  ',
            ( SELECT CONCAT('<item>', FriendlyName, '</item>') 
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
           </test>  ')
        FROM #TableSSAS_DIM m
