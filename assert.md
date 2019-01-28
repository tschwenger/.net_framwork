2. Assert
Skip to end of metadata

<assert> tag is where power of NBi test framework, there are documentations on NBi page (http://www.nbi.io/docs/home/) provides many use cases.  Following are sample code to validate result set from <system-under-test> as input.  It is where expecting conditions to be verify of an out put from <system-under-test>.  Each test case should have only a single <assert> tab.  Following are tag insider of <assert>:

Validate a result set:
          <assert>
           <equalTo>
            <resultSet>
             <row>
              <cell>CY 2006</cell> ← first column 
              <cell>1015</cell>  ← second column
             </row>
            </resultSet>
           </equalTo>
          </assert>



Validate a result set contain specific value:
        <assert>
          <contain caption="Ontario"/>
        </assert>



Validate result set with row count:
        <assert>
         <count more-than="5"/>
        </assert>



Validate a result set with <equivalentTo> value:
         <equivalentTo>
          <range-integer start="1" end="8"/>
        </equivalentTo>



        <equivalentTo>
         <predefined type="months-of-year" language="en"/>
        </equivalentTo>



        <equivalentTo>
        <range-date start="2005-01-01" end="2010-12-31" culture="en" format="MMMM d, yyyy"/>
        </equivalentTo>



        <equivalentTo>
        <range-integer-pattern start="2005" end="2010" pattern="CY " position="prefix"/>
        </equivalentTo>



      <equivalentTo>
       <members children-of="Corporate" >



      <equivalentTo>
      <item>Base Rate</item>



Validate a result set with successful status: 
        <assert>
        <successful/>
        </assert>



Faster:
          <assert>
          <fasterThan max-time-milliSeconds="10000"/>
          </assert>



Sucessful
          <successful/>



Validate multiple rows:
        <assert>
         <evaluate-rows>
          <variable column-index="2">OrderQuantity</variable>
          <variable column-index="3">UnitPrice</variable>
          <variable column-index="4">UnitDiscount</variable>
          <expression column-index="5" type="numeric" tolerance="0.01">= OrderQuantity*(UnitPrice-(UnitPrice*UnitDiscount))</expression>
         </evaluate-rows>
        </assert>

Validate with "is"

        <assert>
         <is>decimal(8, 2)</is>
        </assert>



Linked to:
         <linkedTo>
         <measure-group caption="Internet Sales"/>
        </linkedTo>



Match pattern:
        <assert>

         <matchPattern>

          <regex>^\s*[a-zA-Z,\s]+\s*$</regex>

         </matchPattern>

        </assert>



Order:
          <assert>
           <ordered rule="chronological"/>
          </assert>



Predicate:
          <assert>
          <all-rows>
          <variable column-index="1">TotalDue</variable>
          <predicate name="TotalDue">
            <less-than>10000000</less-than>
          </predicate>
          </all-rows>
          </assert>



        <predicate name="TotalDue">
         <within-range>(+)</within-range>
        </predicate>



        <predicate name="Name" type="text">
        <more-than>Afg</more-than>
        </predicate>



        <predicate name="Name" type="text">
         <starts-with>E</starts-with>
        </predicate>



        <predicate name="Name" type="text">
        <contains ignore-case="true">en</contains>
        </predicate>



        <predicate name="Name" type="text">
        <matches-regex>^[A-Z][A-z\s]+$</matches-regex>
        </predicate>



        <predicate name="Name" type="text">
        <lower-case/>
        </predicate>



Validate against with data in csv file:
          <equalTo>
          <column index="0" role="key" type="text"/>
          <column index="1" role="value" type="numeric"/>
          <resultSet file="..\Csv\ResellerOrderCountByYearBefore2006WithProfile.csv"/>
          </equalTo>



Validate result set is a query:
          <assert>
           <equalTo>
            <query connectionString="Driver={ODBC Driver 13 for SQL Server};Server=...,1433;Database=...;Uid=...;Pwd=123;Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;">
             select 'OK';
            </query>
           </equalTo>
          </assert>



Convert query output to match with <system-under-test>
          <test name="CSharp string to string and decimal to decimal (with Tolerance)">
           <system-under-test>
            <execution>
             <query>
              select '10.2016', 121 union all select '11.2016',242
             </query>
            </execution>
          </system-under-test>
          <assert>
           <equalTo>
            <column index="0" role="key" type="text">
             <transformation original-type="text">value.Substring(5,2) + "." + value.Substring(0,4)</transformation>
            </column>
            <column index="1" role="value" type="numeric" tolerance="10%">
             <transformation original-type="numeric">value * Convert.ToDecimal(1.21)</transformation>
            </column>
            <query>
              select '2016.10', 100 union all select '2016.11', 210
            </query>
           </equalTo>
           </assert>
          </test>



Validate resultset:
            <equalTo>
            <column index="1" role="value" type="numeric" tolerance="5%"/>
            <resultSet>
            <row>
            <cell>CY 2005</cell>
            <cell>350</cell>
            </row>
            <row>
            <cell>CY 2006</cell>
            <cell>1000</cell>
            </row>

...

          </resultSet>

          </equalTo>



Validate syntax:
          <syntacticallyCorrect/>


In case of result set return less than expected, use subset:
          <subsetOf>
          <item>Account</item>
          <item>Date</item>
          <item>Department</item>
          <item>Destination Currency</item>
          <item>Organization</item>
          <item>Scenario</item>
          </subsetOf>



In case of result set return more than expected, use supperset:
            <assert>
            <superset-of>
            <column index="0" role="key" type="text"/>
            <column index="1" role="value" type="numeric"/>
            <query>
            SELECT 'CY 2006', 1015.0 

            UNION ALL

            SELECT 'CY 2007', 1521.0
            </query>
            </superset-of>
            </assert>



Row count with filter:
            <row-count>
            <filter>
            <variable column-index="2">Region</variable>
            <predicate name="Region" type="text">
            <equal>Europe</equal>
            </predicate>
            </filter>
            <more-than>25%</more-than>
            </row-count>



Runtime parametter value:
            <row-count>
            <equal>@ThreeQuery</equal>
            </row-count>



Validate with exists:
        <assert>
        <exists/>
        </assert>



Not exist:
          <assert>
          <exists not="true"/>
          </assert>



Validate contained-in:
            <contained-in>

            <item>Organizations</item>
            </contained-in>



Range of data:
          <row>
          <cell>CY 2006</cell>
          <cell>]100;500]</cell>
          </row>



          <cell>(>500)</cell>
