Make sure you have the necessary downloads per confluence. Dll, .net framework etc

Make sure the framework is dropped into the folder you are testing from on your local machine

Make sure the connections document in your test folder points to the proper servers connections (I dropped a copy of the proper connections just in case they get overwritten(dev environment))

sync and drop the updated files from the source repo into your test folder


from the power shell ise run the following file using the run selection selection:

"powershell promt for nunit test case"


from cmd shell

changes directory the the following:

c:

navigate down to the directory if needed

cd users\username\\documents\my tests


just run the following after 2018-03-23_test1 "name of dimensino"



note if you are running test cases from the dataprocessvalidation folder follow the same steps above but point to one level lower on your file directory

example

cd users\username\documents\my tests\DataProcessValidation

You will need to update the command prompt for the power shell script as well to point to the proper directory. Then you should be fine. 

