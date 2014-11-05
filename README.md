Multi-WP-Script
===============

The point of this script is to easily create multiple WordPress sites for multiple users on one server. There is a user list which the script checks. New users are set up automatically and anything no longer on the list is removed. The script also creates backups and sends an email to the admin to notify whether or not DNS entries need to be updated.

I have this script running every night at 2AM by adding the following line to /etc/crontab:

0  2  *  *  * root       ~/setup-accounts


Important things to note:
=========================
- This script has only been tested on Centos 7
- Make sure you have your LAMP stack setup
- You will need to create the directory "/backups" if you plan on using that feature (Or you can modify the code to change the location completely)
- The scripts need to be in the same location
- If you copy or download the scripts you will need to make them executable (chmod +x script_name) except for "users", that is just a text file
- MAKE SURE YOU DON'T HAVE ANY OTHER FOLDERS IN /var/www/html/
  - This script will delete any folders in that directory that don't match the user list
  - Files are fine to have there
  - If you need to have other folders in apache's root directory, you will need to add additional code in the script for error checking
    - To get around this you need to add another nested if/else on line 88. This would basically be: if folder name matches (folder_to_keep or another_folder_to_keep) then do nothing. Else, continue with commands to remove everything.
    - Also, if you have additional folders in the Apache root directory that you want to keep, you will need to change the  variable "html". For example, say you have 2 other folders in /var/www/html/ that you don't want to be deleted. After the value of "html" is assigned on line 74, you will need to subtract 2 from that variable so it has only the number of folders that are/were users' folders.


What these scripts do in detail and how to use them:
================================
- Put whatever accounts you want to have setup on the server in the file "users"
  - There are examples in that file already. Make sure to remove them when you add ones you want
- Each account in "users" is checked one by one to see if it already exists
  - If it exists the program says the user already exists
  - If nothing is set up for that user then the script does the following in order:
    - Downloads wordpress
    - Creates apache directory
    - Copies wordpress to apache directory
    - Creates backup folder
    - Generates random 16 digit password for database account
    - Creates database and account for wordpress to use (both are the name of the user)
    - Sets password for account to random password and gives database account full rights to database
    - Inputs the database name, account name, and account password into the wordpress config file
    - Creates virtual host file
    - Adds user to the array "add", which is used to send an email to notify admin to update DNS entries
- The script then checks to see if there are more folders in the apache directory than there are users
  - This would happen when a user is removed from the user list
  - If there are more folders in the apache directory than there are users then the script does the following:
    - Checks each folder in the apache directory one by one to see if there is a matching name in "users"
      - If there isn't a matching name in "users" then the script does the following
        - Tells you which user needs to be removed
        - Deletes the apache directory
        - Deletes the database and database account
        - Deletes the virtual host file
        - Adds user to the array "remove", which is used to send an email to notify admin to update DNS entries
  - If there aren't more folderes in the apache directory than there are users then the script says everything is OK
- If at least one user was added then the script does the following:
  - Deletes the downloaded WordPress files
  - Gives apache ownership of everything in the apache directory and the virtual host files
- If at least one user was added or removed then the script does the following:
  - Refreshes the mysql privileges so everything takes effect immediately
  - Restarts apache so the virtual host files are loaded
- Runs the backup script (Comment that line out if you don't want that script to be run)
  - The backup script creates a tar archive for the user's database and apache directory
  - If there are files in the user's backup directory that are older than 3 weeks they will be deleted.
- Sends email to admin reporting users that need to be added or removed in DNS
  - If the arrays "add" and "remove" are both empty then it does nothing
  - If one of the arrays is empty it will set the empty one to "none" and then send an email
- That's it, have fun

















