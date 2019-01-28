

Example code for test suite:


          <?xml version="1.0" encoding="utf-8"?>
          <testSuite name=" data validation test suite" xmlns="http://NBi/TestSuite">

            <!-- Test cases-->
          <group name = "Name of the testing group">
            <test name="TestId:11101. Validate '"Name of the measure"" uid="11101">
              <system-under-test>
                <execution>
                  <query> <![CDATA[     SQL code   ]]> </query>
                </execution>
              </system-under-test>
              <assert>
                <equalTo>
                  <column index="0" role="value" type="numeric" tolerance="0.000001"/>
                  <query connectionString="@Assert-SSAS">      <![CDATA[     " DAX or mdx query make sure it returns a scalar value "  ]]> </query>
                </equalTo>
              </assert>
            </test>

        <!-- Test multiple results-->
          <test name="Ttest name" uid="">
            <system-under-test>
            <execution>
              <query> <![CDATA[     
        DECLARE @T DECIMAL(19, 4)
          ,@L DECIMAL(19, 4)
          ,@FiscalPeriodNumber INT = 201808
          ,@FiscalPeriodNumberL INT
          ,@TuitionType VARCHAR(250) = 'Subsidy'

        SET @FiscalPeriodNumberL = @FiscalPeriodNumber - POWER(10, LEN(@FiscalPeriodNumber) - 4)
        SET @T = (
            SELECT SUM(ISNULL(f.GLBalanceAmount, 0))
            FROM BING_EDW.dbo.DimDate AS d
            INNER JOIN balance AS f ON f.DateKey = d.DateKey
            INNER JOIN asa a ON f.AccountSubaccountKey = a.AccountSubaccountKey
            INNER JOIN DimOrganization o ON f.OrgKey = o.OrgKey
              AND o.Date IS NULL
            WHERE d.FiscalPeriodNumber = @FiscalPeriodNumber
              AND o.OrgDistrictName = 'R03 D07'
              AND a.[Field Path] LIKE '%9200.000100%'
              AND (
                a.[Tuition Type] = @TuitionType
                OR @TuitionType IS NULL
                )
              AND f.GLMetricTypeKey = 1
              AND f.DataScenarioKey = 1
            )
        SET @L = (
            SELECT SUM(ISNULL(f.GLBalanceAmount, 0))
            FROM DimDate AS d
            INNER JOIN balance AS f ON f.DateKey = d.DateKey
            INNER JOIN asa a ON f.AccountSubaccountKey = a.AccountSubaccountKey
            INNER JOIN DimOrganization o ON f.OrgKey = o.OrgKey
              AND o.EDWEndDate IS NULL
            WHERE d.FiscalPeriodNumber = @FiscalPeriodNumberL
              AND o.OrgDistrictName = 'R03 D07'
              AND a.[Field Path] LIKE '%9200.000100%'
              AND (
                a.[Tuition Type] = @TuitionType
                OR @TuitionType IS NULL
                )
              AND f.GLMetricTypeKey = 1
              AND f.DataScenarioKey = 1
            )

        SELECT [TvL%] = ROUND(((@T - @L) / @L), 6)
        WHERE @T != 0
          OR @L != 0
              ]]>            </query>
            </execution>
            </system-under-test>
            <assert>
            <equalTo>
              <column index="0" role="value" type="numeric" tolerance="0.000099"/>
              <query connectionString="@Assert-SSAS">     <![CDATA[      
        DAX direct query
              ]]>         </query>
            </equalTo>
            </assert>
          </test>


             </group>

          </testSuite>
