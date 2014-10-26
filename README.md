Multi-WP-Script
===============

The point of this script is to easily create multiple WordPress sites for multiple users on one box. There is a user list which the script checks. New users are added and anything not on the list is removed. Script also creates backups.


Important things to note:
- This script has only been tested on Centos 7
- You will need to create the directory "/backups" if you plan on using that feature (Or you can modify the code to change the location completely)
- The scripts need to be in the same location
- If you copy or download the scripts you will need to make them executable (chmod +x script_name) except for "students", that is just a text file

What these scripts do in detail:
- Put whatever accounts you want to have setup on the box in the file "students"
- Each account in "students" is checked one by one to see if it already exists
- If nothing is set up for that person then the script downloads wordpress, creates an apache directory, creates a backup folder, creates a wordpress database and account with a random password, and a virtual host file.
- The script then checks if 
