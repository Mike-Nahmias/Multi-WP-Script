Multi-WP-Script
===============

The point of this script is to easily create multiple WordPress sites for multiple users on one box. There is a user list which the script checks. New users are added and anything not on the list is removed. Script also creates nightly backups.


Important things to note:
- This script has only been tested on Centos 7
- You will need to create the directory "/backups" if you plan on using that feature (Or you can modify the code to change the location completely)
- The scripts need to be in the same location
- If you copy or download the scripts you will need to make them executable (chmod +x script_name) except for "students", that is just a text file
