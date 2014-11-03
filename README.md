Multi-WP-Script
===============

The point of this script is to easily create multiple WordPress sites for multiple users on one box. There is a user list which the script checks. New users are added and anything not on the list is removed. Script also creates backups.


Important things to note:
- This script has only been tested on Centos 7
- You will need to create the directory "/backups" if you plan on using that feature (Or you can modify the code to change the location completely)
- The scripts need to be in the same location
- If you copy or download the scripts you will need to make them executable (chmod +x script_name) except for "students", that is just a text file
- MAKE SURE YOU DON'T HAVE ANY OTHER FOLDERS IN /var/www/html/
  - This script will delete any folders in that directory that don't match the user list
  - Files are fine to have there
  - If you need to have other folders in apache's root directory, you will need to add additional code in the script for error checking
    - To get around this you need to add another nested if/else on line 88. This would basically be: if folder name matches (folder_to_keep or another_folder_to_keep) then do nothing. Otherwise, continue with commands to remove everything.


What these scripts do in detail:
- Put whatever accounts you want to have setup on the box in the file "students"
- Each account in "students" is checked one by one to see if it already exists
- If nothing is set up for that person then the script downloads wordpress, creates an apache directory, creates a backup folder, creates a wordpress database and account with a random password, and a virtual host file.
- The script then checks if 
