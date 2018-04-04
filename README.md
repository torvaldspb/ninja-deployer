# ninja-deployer
Simple php tool to safely deploy php projects, with git, mysql and laravel support

The script requires git shell installed on your system, git and php commands should be available from the shell.
Run php somposer install before using the script (despite it is used only for classes autoloading).

Command line usage: deployer.php InstanceName [refresh|clean]
Difference between refresh and clean i sthe DB behavior.
refresh - runs only new migrations or sql script for no-laravel project
clean - performs a complete DB reset (php artisan migrate:refresh --seed or one by one $db_sql_clean_file and $db_sql_update_file sql scripts) 

You should provide a instances\InstanceName\InstanceName.php file for every InstanceName, it should extend the parent Deployer\DeployerInstance class. All instance paramaters are available from there. There is a possibility to set additional actions with the DB for both Laravel and non-Laravel projects. There is a file to be used as a template here - instances\TemplateProject\TemplateProject.php. Also there is an example instance file here - instances\Project1\Project1.php

The idea is to make this project as flexible as possible. The goal was reached by using doDeployAdditional and doCleanupAdditional, where you can put your own code as you like. It will be executed before replacing old files and after, respectively.
Main logic is discribed in deployer.php file.

To safely handle errors in the deployment process, it is being done in the new, temporary folder, and old files will be replaced only after the new system was tested by using curl.
Deployer\Deployer class can also be used for batch deployment of multiple instances, without interrupting the process in case of one instance failure.
There are 2 logfile types , logs\Deployer.log is a general one, and logs\InstanceName.log is used by every InstanceName.

System checks the OS type while executing deleteFolder() - it is needed to successfully delete folders like .git under Windows.
