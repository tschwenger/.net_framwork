# Setting this shit up

setting this shit up:
.Net Framework 4.5 (or higher)
http://www.microsoft.com/en-us/download/details.aspx?id=30653


Install NUnit 2.6.4 framework.  Download NUnit 2.6.4
https://github.com/nunit/nunitv2/releases/tag/2.6.4



Folder Framework and Schema from NBi are required. The version 1.17.1
http://www.nbi.io/release/


Each test suite need:

"DLL.config" file: indicates which test suite .nbits file to run.  
".Nunit" file: contains dll.config files, NBi framework, project configuration (Debug, release), base working directory that contain .nbits and .dll files
".nbits" test suite file: contains granular level of each test cases.  Connection to source and target for both "system-under-test" and "assert" are located in this file.  
"Framework" directory, DDL.config, .Nunit, .nbits files should be located on the same folder.
Optional "VisualState.xml" file: Another project file that allow which categories of test to be excluded in during test run.  If  <ExcludeCategories> = true and <SelectedCategories> has test cases, the test cases in <SelectedCategories> tag are ignored by test run.


NUnit also provides UI tool to run test cases.  It is part of the installation of NUnit 2.6.4.

For Visual Studio NBi extension 

https://www.nuget.org/packages/NBi.VisualStudio/
