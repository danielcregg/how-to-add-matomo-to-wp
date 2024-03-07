# How To Install Matomo Analytics Server

**Introduction:**

Matomo (formerly Piwik) is an open-source web analytics application that rivals Google Analytics. It offers even more features, including the ability to send custom daily, weekly, and monthly reports to your clients. Here's a video introduction to get you started: **Your introduction to Matomo Analytics!**

[![Image Alt Text](https://img.youtube.com/vi/Qc2kooLNDiU/maxresdefault.jpg)](https://www.youtube.com/watch?v=Qc2kooLNDiU>)

**Downloading and Unzipping Matomo:**

These commands will download and extract the latest version of Matomo:

~~~bash
sudo wget https://builds.matomo.org/matomo.zip -P ~
echo "unzip unzip/replace boolean true" | sudo debconf-set-selections
sudo apt -qy install unzip
sudo unzip -o ~/matomo.zip -d /var/www/html
~~~

**Explanation:**

* `sudo wget https://builds.matomo.org/matomo.zip -P ~`: This command downloads the latest version of the Matomo zip file using `wget` and stores it in your home directory (`~`).
* `echo "unzip unzip/replace boolean true" | sudo debconf-set-selections`: This line sets a configuration option (`debconf-set-selections`) to automatically accept any prompts during the unzip installation (`unzip`).
* `sudo apt -qy install unzip`: This installs the `unzip` package using `apt` with quiet mode (`-q`) to suppress output.
* `sudo unzip -o ~/matomo.zip -d /var/www/html`: This extracts the downloaded `matomo.zip` file (`-o` to overwrite existing files) into the `/var/www/html` directory, which is the common default location for web server documents.

**Assigning Permissions:**

The following commands configure permissions for the Matomo directory:

~~~bash
sudo chown -R www-data:www-data /var/www/html/matomo
sudo chmod -R 0755 /var/www/html/matomo/tmp
sudo service apache2 restart
~~~

**Explanation:**

* `sudo chown -R www-data:www-data /var/www/html/matomo`: This command changes ownership (`chown`) of the `/var/www/html/matomo` directory and its contents (`-R` for recursive) to the user and group `www-data`. This user typically runs the web server process and needs ownership for proper access.
* `sudo chmod -R 0755 /var/www/html/matomo/tmp`: This sets permissions (`chmod`) on the `/var/www/html/matomo/tmp` directory (where temporary files might be stored) recursively (`-R`) to allow read, write, and execute access for the owner (user and group `www-data`) and only read and execute access for others (`0755`). 
* `sudo service apache2 restart`: This restarts the Apache web server service (`apache2`) to ensure the permission changes take effect.

**Creating the Database**

Now we'll create a database for Matomo using MySQL commands:

~~~bash
sudo mysql
CREATE DATABASE matomodb;
CREATE USER matomoadmin@localhost IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON matomodb.* TO matomoadmin@localhost;
FLUSH PRIVILEGES;
exit;
~~~

**Explanation:**

1. `sudo mysql`: This launches the MySQL command-line interface with superuser privileges (`sudo`).
2. `CREATE DATABASE matomodb;`: This creates a new database named `matomodb` for Matomo to store its data.
3. `CREATE USER matomoadmin@localhost IDENTIFIED BY 'password';`: This creates a new MySQL user named `matomoadmin` who can access the database from the local machine (`localhost`) and sets the password to `password` (replace with a strong password).
4. `GRANT ALL PRIVILEGES ON matomodb.* TO matomoadmin@localhost;`: This grants the `matomoadmin` user all privileges on the `matomodb` database, allowing Matomo to interact with the data.
5. `FLUSH PRIVILEGES;`: This refreshes the MySQL privilege tables to reflect the changes made.
6. `exit;`: This exits the MySQL command-line interface.

**Configuration via GUI**

1. Browse to your server by going to http://yourserverIP/matomo in a web browser.
2. Click "Next" and proceed through the system check. Everything should pass if LAMP is properly installed and configured (LAMP stands for Linux, Apache, MySQL, and PHP, a common web server stack).
![image](https://github.com/danielcregg/how-to-add-matomo-to-wp/assets/22198586/1925df2e-b19e-48c5-800c-c578240a646e)

