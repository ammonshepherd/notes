---
layout: page
title:  "Updating Omeka and Neatline"
categories: how-to
---

The following steps assume that you are upgrading Omeka and Neatline on the
same server where the production Omeka/Neatline site is located. A better option
would be to copy the files and database to a development environment, perform
the upgrade there, and then replace the live files and database with the
upgraded version. In any case, please, please, please, make a backup copy of
your database and files before upgrading. This process will leave the site
unavailable for a time during the upgrade process. Also, these steps are
general and given "as is." They are a basic guide for help in upgrading your
Neatline plugin.

# Things to Note

- In upgrading, you'll be upgrading two things, the Omeka core files and the Neatline plugin.
- You will need to progressively advance through stable builds.
    - with Omeka, you move from 1.5 to 2.0 to 2.1 to 2.2 
    - with Neatline, depending on where you start, you might move from 1.1.x to
    2.0 to 2.0.2 to 2.2 to 2.3. These are builds with migrations (mostly) and
    gets you to the most recent build.
- These steps will create a new directory and new database for the upgraded version.


# Steps:

## On the server (command line)

- Enter the parent directory where the Omeka installation is located.
    - For example, if the Omeka installation is at /var/www/htdocs/myproject/,
    then you will do the following commands at /var/www/htdocs/.
- Get a copy of the next highest Omeka release from the version you are
  upgrading from. (If your current version is 1.5, then you'll need version
  2.0.) 

    ```
    git clone https://github.com/omeka/Omeka.git
	```

    - switch to stable branch corresponding to that of the Omeka instance of the
      Neatline exhibit with which you are working 

        ```
        cd Omeka
        ```

        ```
        git checkout stable-2.0
        ```

        - To get the plugins and themes

        ```
        git submodule init
        ```

        ```
        git submodule update
        ```
        
- OR, grab a copy of the zip file from Omeka.org

    ```
    wget http://omeka.org/files/omeka-2.0.4.zip
    ```

    ```
    unzip omeka-2.0.4.zip
    ```

    ```
    mv omeka-2.0.4/ Omeka/
    ```

- Set up ```db.ini``` (rename ```db.ini.changeme``` to ```db.ini```, or copy previous db.ini to
  new instance).
    - Change host, username, password, dbname, port, and prefix parameters as needed.
- Rename ```htaccess.changeme``` to ```.htaccess```
- Rename ```application/config/config.ini.changeme``` to ```application/config/config.ini```
- Copy over files from the previous Omeka site (including ```archive/``` (or ```files/```),
  ```plugins/```, ```themes/``` etc...) to the new Omeka instance
    - To get the live archive/files (for Omeka versions 1.5 and below)

        ```
        cp -r myproject/archive/ Omeka/files/
        ```

        - You will also need to rename the Omeka/files/files/ directory to Omeka/files/original/

            ```
            mv Omeka/files/files/ Omeka/files/original/
            ```

    - OR for Omeka versions 2.0 and above

        ```
        cp -r myproject/files/ Omeka/files/
        ```

    - AND copy over any plugins and themes. Exclude the Neatline plugin. That will be covered in the next step.

        ```
        cp -r myproject/plugins/ Omeka/plugins/
        ```

        ```
        cp -r myproject/themes/ Omeka/themes/
        ```

- Install the newer version of the Neatline plugin. (When moving from 1.x.x to
  2.x.x you do not need the NeatlineMaps plugin. This functionality is included
  in the Neatline plugin by default after 2.0.)
    - Using git. Move into the Omeka/plugins/ directory.

        ```
        git submodule add -f https://github.com/scholarslab/Neatline.git Neatline
		```

		```
        cd Neatline
		```

		```
        git checkout 2.x.x
        ```

    - Using the .zip file from Omeka.org. Copy the zip file to the plugins
    directory, then change to the plugins directory and unzip the file. It will
    prompt you to overwrite existing files. Do this for all files.

        unzip Neatline-2.0.4.zip
        
- Deactivate plugins on live installation. Either through the admin site, or
  via phpMyAdmin, you can enter the following SQL:

    ```
    UPDATE `PREFIX_plugins` SET `active`=0 WHERE 1
    ```

    - When upgrading from Neatline versions 1.x to 2.x, deactivate and uninstall the NeatlineMaps plugin.

        - from command line

            ```
            mysql -uUSER -p --execute="DELETE FROM PREFIX_plugins WHERE name="NeatlineMaps"; DROP TABLE IF EXISTS `PREFIX_neatline_maps_servers`; DROP TABLE IF EXISTS `PREFIX_neatline_maps_services`"
            ```

        - from the Omeka admin plugin page, deactivate NeatlineMaps plugin and then uninstall.
- Export the live, current database to an sql file
    - from command line

        ```
        mysqldump -uUSER -p ORIGINAL_DATABASE_NAME > ORIGINAL_DATABASE_NAME.sql
        ```
    - from phpMyAdmin, use the 'Export' feature to save output to an sql file.
- Create a new database.
    - from command line

        ```
        mysqladmin -uUSER -p create NEW_DATABASE_NAME
        ```

        - OR

        ```
        mysql -uUSER -p --execute="CREATE DATABASE NEW_DATABASE_NAME;"
        ```

    - from phpMyAdmin, use the Database tab from the home page.
    - Remember to give the database a user with permissions to access the
    database. This needs to be the same as the database name, user name and
    password as used in the db.ini file.
- Import the database from previous installation into the new database
    - from command line

        ```
        mysql -uUSER -p NEW_DATABASE_NAME < ORIGINAL_DATABASE_NAME.sql
        ```
    - from phpMyAdmin, with the new database selected, use the 'Import' feature to import the sql file you just created.
- Rename the current installation directory to make way for the new.

    ```
    mv myproject old-myproject
    ```

	```
    mv Omeka myproject
    ```


## In the Omeka Admin (web browser)

- Visit the admin site to upgrade the database.
    - http://myproject.com/admin
- Switch the theme to "Thanks Roy" or another theme. The Neatline and
  NeatlineThin themes have not yet been updated for use with Omeka 2.x.
    - You can uninstall and delete Neatline and neatlinethin themes
- In the admin plugin page, upgrade the Neatline plugin by clicking the "Upgrade" button.
- Check to see if the site loads and if the Neatline exhibits are there by clicking on the "Neatline" tab in the left menu box.



## Fix Missing Neatline Exhibits

- When upgrading from Neatline 1.x to 2.0, the upgrade script does not
  completely upgrade the Neatline plugin. You will need to manually start the
  background process to get the Neatline database tables to update correctly.

### On the server (command line)

- In the Neatline plugin directory, find Neatline_Migration_200.php
  (Neatline/migrations/2.0.0/), and insert the following code at the top of the
  migrateData() function:

        $fc = Zend_Registry::get('bootstrap')->getPluginResource('FrontController')->getFrontController();
        $fc->getRouter()->addDefaultRoutes();


    - So this:

        <pre>
        public function migrateData()
        {
            $this->db->beginTransaction();
            try {
        </pre>

    - Becomes this:

        <pre>
        public function migrateData()
        {
            $fc = Zend_Registry::get('bootstrap')->getPluginResource('FrontController')->getFrontController();
            $fc->getRouter()->addDefaultRoutes();  
            $this->db->beginTransaction();
            try {
        </pre>

- Create a Bash script (```script.sh```) file with the following: (You'll need to
  change /PATH/TO/, USER, PASSWORD, and NEW_DATABASE_NAME to the appropriate values.)

        #!/bin/sh

        /PATH/TO/bin/mysql -uUSER -pPASSWORD NEW_DATABASE_NAME --execute="UPDATE PREFIX_processes SET status='starting' WHERE id=1;"

        /PATH/TO/bin/php /PATH/TO/myproject/application/scripts/background.php -p 1

- To run the script you'll need to change the permissions:

    ```
    chmod u+x script.sh
    ```

- To run the script, type:

    ```
    ./script.sh
    ```

# Upgrade to the next version

- Switch to the next stable build for Omeka (e.g. stable-2.1 or stable-2.2)
    - In the install directory

    ```
    git checkout stable-2.1
    ```

- Go to Omeka admin web pageand update the database.
- Update the Omeka plugins and themes

    ```
    git submodule update
    ```

    - This installs the latest version of Neatline. To make the incremental
    upgrade, go to the Neatline plugin directory and checkout the next
    incremental upgrade.

        git checkout 2.1.1

- Check to see if exhibits in Neatline or working properly.
- Repeat the steps above, increasing the version numbers until you have the latest.
