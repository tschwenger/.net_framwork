
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
