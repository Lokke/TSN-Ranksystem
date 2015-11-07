# TSN-Ranksystem
The Ranksystem is a simple tool to automatically grant ranks (servergroups) to user on a TeamSpeaks3 Server for online activity. You can create your own servergroups, with permissions, icons etc. of your choice, and define these for the ranksystem.

Features:

    scan online activity of users
    automatically add an user to a servergroup, if reached a predefined online time
    separate list for future rankups
    inform the user at rankup (as TeamSpeak textmessage)
    language-support [at moment english (en), german (de) and russian (ru)]
    simple installation with installation guide
    webinterface to configurate the ranksystem
    and much more...

 

Configs:

    customizable online time -> servergroup (needed time to rankup in group..)
    option to substrate idle time from online time for rankup (use active time for rankup)
    option to inform user per TeamSpeak textmessage at rankup
    except servergroups
    except unique ids of clients
    and much more...

 

Requirements:

    Webspace (webserver)
    PHP 5.2.1 or later with PDO support
    Database; List of potential (tested with MySQL on MariaDB)
    Database user privileges:
        create databases and tables (for installation)
        select, update, insert and delete entries; should be normal ;-)
        alter tables (for update ranksystem)
    TeamSpeak 3 Server Query Account with Permissions:
        b_virtualserver_client_list
        i_client_private_textmessage_power
        i_group_member_add_power
        i_group_member_remove_power
        b_virtualserver_servergroup_list
    Connection to ts-n.net on TCP Port 80 from your webserver (for Update check)
    Job Sheduler; e.g. a cronjob (unix) or task (windows)


Installation Linux:

1. Unzip the downloaded file "ranksystem x.xx-beta.zip".

2. Upload the whole unzipped folder to a directory on your webspace and grant permissions to the webuser (e.g. chown -R www-data /var/www/ranksystem).

3. Open the "install.php" on your Webbrowser and follow the instructions. After installation you have to configurate according to your pleasure. A documentation you find for each parameter as mouse-hover inside the webinterface.

4. Create a cronjob (here described for debian/ubuntu) for the worker to observer the activity of the clients (and more stuff):

Login in to your server via ssh.

Enter the command "crontab -e" (In vim you have to press "i" to be able to edit the text)

Paste the following lines in a new line (you have to edit the path):

*/1 * * * * php /path/to/your/websever/path/to/ranksystem/worker.php >/dev/null 2>&1

If you want, you can edit the connection time.. Here it is every minute. (the lower the time, the more accurate the script works)

Save and quit the crontable.

The Script is now ready to work!!! Open the "list_rankup.php" to see, what happens next.

 

 

Installation Windows:

1. Unzip the downloaded file "ranksystem x.xx-beta.zip".

2. Upload the whole unzipped folder to a directory on your webspace and grant permissions to the webuser.

3. Open the "install.php" on your Webbrowser and follow the instructions. After installation you have to configurate according to your pleasure. A documentation you find for each parameter as mouse-hover inside the webinterface.

4. Download the worker.vbs, edit the weburl to your worker.php and then upload it to your windows server on a place of your wish.

5. Open the task sheduler and create a new task (windows help) with following option:

C:/Windows/System32/cscript.exe C:/path/to/your/folder/worker.vbs

The Script is now ready to work!!! Open the "list_rankup.php" to see, what happens next.

 

 

How to use:

worker.php

This file scans the activity of the user and counts the online time. On a predefined online time of an user it adds the servergroup.

list_rankup.php

Shows the next rankups.

webinterface.php

Here you can config the main core of the ranksystem and also the style of the list_rankup.php

 

 

How to update:

This HowTo is only for the last version. If you skipped a version, you have to do the described steps for each update between your and the actual version.

1. Unzip the downloaded file "ranksystem 0.13-beta.zip". You needn't to update the version 0.12-beta separably. The version 0.12-beta is included in 0.13-beta.

2. Save the "other/dbconfig.php" on your websever. We recommend to backup the whole webspace and also the database.

3. Stop the sheduler (cron/task) for the worker.php.

4. Upload the whole unzipped folder to a directory on your webspace and grant permissions to the webuser (i.E. chown -R www-data /var/www/ranksystem).

5. Restore the saved "other/dbconfig.php"

6. Give the folder "icons" execute permissions with a chmod 0777.

7. Open the "update_0-13.php" on your Webbrowser and follow the instructions.

8. Reactivate the sheduler (cron/task) for the worker.php.

 

 

Changelog:

1.00 (XXXX-XX-XX) - not yet released!

! rework style on list_rankup.php; within this the style configuration in the webinterface.php does not matter any more for list_rankup.php
+ add search (filter) function on list_rankup.php
+ add option on list_rankup.php to limit the entries
+ add column actual servergroup in list_rankup.php as option
+ add function "boost"; You can define a servergroup for the ranksystem as boost group. There is also to define how much the boost should count (factor) and how long it should be grant (time). User in the specified servergroup get rated their online time by the factor. The higher the factor, the faster an user reaches the next higher rank. Is the time expired, the boost servergroup get automatically removed from the concerned user.
- correct error by hiding column next servergroup on list_rankup.php

0.13-beta (2015-09-04)

* fullfill russian translation; many thanks to sergey
- correct problem by saving icons (PHP Warning:  file_put_contents[..])
- correct wrong array shift by clean ranksystem database (get error like: parse/syntax error) 
- complement some missed database options with update 0.12-beta

0.12-beta (2015-08-30)

+ add option to show clients in highest group in list_rankup.php (not even in adminlist)
+ add function to clean Ranksystem database; this deletes clients, which doesn't exist any more in the TeamSpeak database. You can also use the perfectly with the ClienCleaner together (http://ts-n.net/clientcleaner.php)
+ add servergroupicon to the column next servergroup in list_rankup.php; servergroup icons get downloaded with worker.php to the subfolder "icons"
- correct count on feeter in list_rankup.php (sitegen needs to be enabled)

0.11-beta (2015-03-27)

! change database connection from MySQLi to PDO; Within this also other databases (e.g. MS SQL, Oracle ..) are possible
+ add slowmode to prevent against bans in case of flood
+ add function for renew the servergroups; can be helpful, when user are not in the servergroup, they should be at the defined online time
+ add link inside the webinterface with an admin overview for the list_rankup.php with all fields/columns
+ add variable language support for list_rankup.php -> list_rankup.php?lang=en
+ add column rank number to list_rankup.php as option
+ add column last seen to list_rankup.php as option
+ add option to simulate the deletion of old clients
+ add new menu to the webinterface to edit the database configuration
* smaller other improvements
- fixed bug, that the SQL statements failed with an error, when a client is using a backslash in its name or a servergroup contains a special sign
- fixed bug; clients with | character in his username couldn't be use for the "selective clients functions", cause the uuid couldn't be dissolving
- fixed bug; fixed wrong update information, when update server is not reachable
- fixed bug; year 2038 problem (max. integer 2147483647) solved, 64-bit system required; for old entries could stand a wrong ip address "127.255.255.255" in database

0.10-beta (2015-02-01)

+ add russian translation; many thanks to sergey
+ add option to remove client out of the servergroup (which was given by ranksystem), by deleting clients out of the Ranksystem database
+ add function to delete one or multiple selected clients out the Ranksystem database
+ add function to set manual a summary online time for one or multiple selected clients
+ add the most important colors to the webinterface; to define it there
+ add column next servergroup in list_rankup.php as option; new TeamSpeak permission required (b_virtualserver_servergroup_list)
+ add option to hide the site generation at the end of the site
+ add client count to the site generation
* relaunch the webinterface; separate the different settings and functions in own sectors, which are foldable
- fixed bug; add a check, when a servergroup was manually given to a client, which is relevant for the ranksystem. This clients will not until add to a servergroup, they reached the time for the next higher rank.
- fixed bug; sorting on list_rankup.php for the column 'next rank up' is now correct

0.02-alpha (2014-12-20):

+ add time format as value
+ add column with active time to list_rankup.php
+ hide excepted clients or rather groups in list_rankup.php as option
+ hide each column in list_rankup.php as separate option
* check version after predefined time, not with every run the worker.php, to save traffic
- fixed bug of wrong sorting for next rankup; the time for next rankup of offline users will now calculate new with every run the worker.php
- fixed bug of countdown the next rankup time in list_rankup.php of offline users
- fixed bug with header problems by saving options in the webinterface.php
- fixed bug that a "reseted" user do not gets an update for his new cldbid

0.01-alpha (2014-10-05)

 

 

ToDo:

    add clientinfo at hover the client in list_rankup.php; clientinfo about country, lastseen, connectionscont,           clientversion, servergroups, clienticon, traffic
    ignore idle time of a client, if under predefine time (on each scan)
    different inform messages about rankup (ts3 private textmessage) for different groups
    option for an additional serverchat message about rankup
    option to set a limit as showing entries for the list_rankup.php, other on next pages
    second kind of rank up, which contains servergroups, that doesn't get removed on next rank up; so they'll stay        forever
    you have more ideas? leave us a comment and perhaps we will implent it with the next release..
