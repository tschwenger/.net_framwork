
example for performance testing nbit

            <?xml version="1.0" encoding="utf-8"?>
          <testSuite name="suite name" xmlns="http://NBi/TestSuite">

            <!-- Test cases-->
          <group name = "GL Wage Rate">
            <test name="TestId:2. Validate performance test name." uid="2">
              <system-under-test>
                <execution>
                  <query connectionString="@SSAS-system-under-test"> <![CDATA[  dax query]]> </query>
                </execution>
              </system-under-test>
              <assert>
                <fasterThan max-time-milliSeconds="2000"/>
              </assert>
            </test>
            <test name="TestId:3. Validate performance'GL Wage Rate' YTD v LY % for GL Balance Actual." uid="3">
              <system-under-test>
                <execution>
                  <query connectionString="@SSAS-system-under-test"> <![CDATA[   DAX query    ]]>          </query>
                </execution>
              </system-under-test>
              <assert>
                <fasterThan max-time-milliSeconds="2000"/>
              </assert>
            </test>
            <test name="TestId:4. Validate performance test name. " uid="4">
              <system-under-test>
                <execution>
                  <query connectionString="@SSAS-system-under-test"> <![CDATA[  dax query  ]]>  </query>
                </execution>
              </system-under-test>
              <assert>
                <fasterThan max-time-milliSeconds="2000"/>
              </assert>
            </test>
            <test name="TestId:5. name of test." uid="5">
              <system-under-test>
                <execution>
                  <query connectionString="@SSAS-system-under-test"> <![CDATA[     
          DAX query
                )
              ]]></query>
                </execution>
              </system-under-test>
              <assert>
                <fasterThan max-time-milliSeconds="2000"/>
              </assert>
            </test>
          </group>

          </testSuite>
