
# Create your own folder for testing

Ideally, test suites will be set up and all you need to do is copy and paste out the test suite from the dev environment that's updated and paste that somewhere locally that you aren't pushing to the source control environment. 

This is a script to run each time you create a new branch and need to test

            # **** Replaced variables below ****
            $FolderType =  ''
                          # '\Functional'
            $PresentWorkingPath = 'C:\Users\working file path\Documents\My Tests' + $FolderType
            $NewLocalFolderPath = 'C:\Users\working file path\Documents\My Tests' + $FolderType  #Could be same as $PresentWorkingPath

            $NetworkFolderPath_tobe_replaced = 'D:\RMWrkDir\TestSuites' # path on Octopus


            ##--------- No need to update code below ----------

            ############ Loop through each directory and replace Octopus path /w your local path
            Get-ChildItem -Path $PresentWorkingPath *.nunit -Recurse -Exclude Framework | ForEach-Object {
                $filename = Get-ChildItem  $_.fullname;
                (Get-Content $filename).replace($NetworkFolderPath_tobe_replaced, $NewLocalFolderPath) | Set-Content $filename
                }


2. Set up connections file in your local test suite to connect to your servers so that you can conduct testing. 

### This file be the NBISET file

here is a sample code to set up your local connections

                                    <?xml version="1.0" encoding="utf-8"?>
                                     <settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://NBi/TestSuite">
                                       <default apply-to="system-under-test">
                                           <connectionString>Data Source=locahost;Initial Catalog=database;Integrated Security=SSPI;</connectionString>
                                       </default>

                                      <reference name="Assert-database">	
                                       <connectionString>Data Source=locahost;Initial Catalog=database;Integrated Security=SSPI;</connectionString>	  
                                      </reference>     

                                     <reference name="Assert-database">	
                                       <connectionString>Data Source=locahost;Initial Catalog=database;Integrated Security=SSPI;</connectionString>	  
                                      </reference>   

                                      <reference name="Assert-database">
                                       <connectionString>Data Source=locahost;Initial Catalog=database;Integrated Security=SSPI;</connectionString>	  
                                      </reference>   

                                       <reference name="Assert-MISC_Staging">
                                           <connectionString>Data Source=locahost;Initial Catalog=MISC_Staging;Integrated Security=SSPI;</connectionString>
                                       </reference>

                                     <reference name="Assert-Salesforce_Staging">
                                       <connectionString>Data Source=locahost;Initial Catalog=Salesforce_Staging;Integrated Security=SSPI;</connectionString>
                                      </reference>



                                       <reference name="SSAS-system-under-test">
                                                <connectionString>Provider=MSOLAP.4;Data Source="SERVER Name";Initial Catalog='"Name of your SSAS datasource"';Integrated Security=SSPI;localeidentifier=1033</connectionString>
                                       </reference>

                                                <reference name="Assert-SSAS">
                                                  <connectionString>Provider=MSOLAP.4;Data Source="SERVER Name";Initial Catalog='_SSAS';Integrated Security=SSPI;localeidentifier=1033</connectionString>	  
                                         </reference>

                                                <reference name="Assert-"Name of a database">	
                                                  <connectionString>Data Source=locahost;Initial Catalog=_Staging;Integrated Security=SSPI;</connectionString>	  
                                         </reference>

                                                <reference name="Assert-"Name of a database"">
                                                  <connectionString>Data Source=locahost;Initial Catalog=_Staging;Integrated Security=SSPI;</connectionString>	  
                                         </reference>   

                                                <reference name="Assert-"Name of a database"">
                                                  <connectionString>Data Source="conncetion string";Initial Catalog=_Staging;Integrated Security=SSPI;</connectionString>	  
                                         </reference>   

                                                <reference name="Assert-"Name of a database"">
                                                  <connectionString>Data Source="conncetion string";Integrated Security=SSPI;</connectionString>	  
                                         </reference>
                                     </settings>

                                      <reference name="Currency1">
                                                <currency-format
                                                            currency-pattern="$n"
                                                            currency-symbol="$"
                                                            decimal-digits="2"
                                                            decimal-separator="."
                                                            group-separator=","/>
                                      </reference>

                                      <reference name="Currency2">
                                                <currency-format
                                                            currency-pattern="$n"
                                                            currency-symbol="$"
                                                            decimal-digits="0"
                                                            decimal-separator="."
                                                            group-separator=","/>
                                      </reference>

                                      <reference name="Number1">
                                                <numeric-format						
                                                            decimal-digits="1"
                                                            decimal-separator="."
                                                            group-separator=","/>
                                      </reference>
                                      <reference name="Number2">
                                                <numeric-format						
                                                            decimal-digits="2"
                                                            decimal-separator="."
                                                            group-separator=","/>
                                      </reference>
                                      <reference name="Number3">
                                                <numeric-format								
                                                            group-separator=","/>
                                      </reference>
                                      <reference name="Number4">
                                                <numeric-format							
                                                            group-separator=","/>
                                      </reference>

                                    </settings>
