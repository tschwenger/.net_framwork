## Generate testing for formatting



        --generates a test for formatting with the measures of a database
        -- ETL Mapping dcoument is persisted in 
        USE QA_dbo --qa database
        GO

        DECLARE @vMaxUpdated DATETIME	
          ,@MeasureName VARCHAR(250) = 'Withdrawal Count'
          ,@formatName VARCHAR(20) = 'Number4' -- Number1, Number2, Number3, Number4, Currency1, Currency2, Percentage1, Percentage2
          ,@TestCaseNumber INT = 1566

        ----------- no need to update code below -----------------
          ,@TestID INT
          --generates a new test id
        SELECT @TestID = NEXT VALUE FOR qadbo.dbo.Test_case_uid 

        SELECT CONCAT('<test name="TestId: ',@TestID, '. Validate ''', @MeasureName, ''', format as ', @formatName, '. BNG-', @TestCaseNumber,'" uid="',@TestID ,'">    
            <system-under-test>
             <execution>
              <query connectionString="@Assert-SSAS"> <![CDATA[
              SELECT
                [Measures].[', @MeasureName, '] ON 0,
                NON EMPTY([Date].[Fiscal Period Number]) ON 1
              FROM [Model]
              ]]>
              </query>
            </execution>
            </system-under-test>
            <assert>
            <matchPattern> ',CHAR(13),
              CASE 
                WHEN @formatName LIKE 'Number%' THEN  CONCAT('<numeric-format ref="', @formatName, '"/> ')
                WHEN @formatName LIKE 'Currency%' THEN  CONCAT('<currency-format ref="', @formatName, '"/> ')
                WHEN @formatName = 'Percentage1' THEN '<regex>^\d[0-9]{1,3}(?:,?[0-9]{3})*\.[0-9]{1}[%]</regex>'
                WHEN @formatName = 'Percentage2' THEN '<regex>^\d[0-9]{1,3}(?:,?[0-9]{3})*[%]</regex>'
              END ,
            '</matchPattern>		
            </assert>
          </test>')
