

# Batch file to run tests

step 1. This file needs to be in your test suite


Here is the script for the file:


echo off

      SET FileName=%1
      "c:\Program Files (x86)\NUnit 2.6.4\bin\"nunit-console "C:\Users\folderpath\Documents\My Tests"\%FileName%\%FileName%.nunit
