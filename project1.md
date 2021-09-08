# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
A technology stack is a set of frameworks and tools used to develop a software product. This set of frameworks and tools are very specifically chosen to work together in creating a well-functioning software. They are acronymns for individual technologies used together for a specific technology product. some examples are…

LAMP (Linux, Apache, MySQL, PHP or Python, or Perl)
LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl)
MERN (MongoDB, ExpressJS, ReactJS, NodeJS)
MEAN (MongoDB, ExpressJS, AngularJS, NodeJS

To get started with this project you will need an AWS account if you dont have one then kindly sign up for a free tier account.
sign into your aws account and select the EC2 service
select the desired region example us-east-1
launch an ec2 instance of t2.micro family with Ubuntu Server 20.04 LTS (HVM)
configure desired storage.
select an existing security group with ssh port 22and 80 open if not create a security group and open up port 22 and 80
create and download a keypair if you dont have 1 already.
review and lunch the instance
After instance check is completed connect to the instance via{ ec2 instance connect or by ssh}
## STEP 1 — INSTALLING APACHE AND UPDATING THE FIREWALL
### Install Apache using Ubuntu’s package manager ‘apt’:
-sudo apt update
-sudo apt install apache2
-sudo systemctl status apache2
-curl http://127.0.0.1:80
![Screenshot 2021-09-08 084921](https://user-images.githubusercontent.com/76639282/132583813-cc13e308-0b04-4870-baa7-a178b28ff27f.png)
## STEP 2 — INSTALLING MYSQL
Now that you have a web server up and running, you need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database. MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.
-sudo apt install mysql-server
-sudo mysql_secure_installation
If you enabled password validation, you’ll be shown the password strength for the root password you just entered and your server will ask if you want to continue with that password. If you are happy with your current password, enter Y for “yes” at the prompt:

Estimated strength of the password: 100 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
For the rest of the questions, press Y and hit the ENTER key at each prompt. This will remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately respects the changes you have made.
-sudo mysql
![Screenshot 2021-09-08 090708](https://user-images.githubusercontent.com/76639282/132586884-9a3b8982-ac30-49c8-8966-5701b6e5cc12.png)
## STEP 3 — INSTALLING PHP
To install these 3 packages at once, run:
sudo apt install php libapache2-mod-php php-mysql
Once the installation is finished, you can run the following command to confirm your PHP version 
By typing the code in the bracket (php -v) the below 
PHP 7.4.3 (cli) (built: Oct  6 2020 15:47:56) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
At this point, your LAMP stack is completely installed and fully operational.
## STEP 4 — CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE
In this project, you will set up a domain called projectlamp, but you can replace this with any domain of your choice
Create the directory for projectlamp using ‘mkdir’ command as follows:
-sudo mkdir /var/www/projectlamp
-sudo chown -R $USER:$USER /var/www/projectlamp
-sudo vi /etc/apache2/sites-available/projectlamp.conf
-This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:

<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
you can save the vi by typing this esc,:wq then enter key
-sudo ls /etc/apache2/sites-available
-000-default.conf  default-ssl.conf  projectlamp.conf
-sudo a2ensite projectlamp
-sudo a2dissite 000-default
-sudo systemctl reload apache2
Your new website is now active, but the web root /var/www/projectlamp is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected:
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
Now go to your browser and try to open your website URL using IP address:
-http://<Public-IP-Address>:80
 ![Screenshot 2021-09-08 092541](https://user-images.githubusercontent.com/76639282/132589045-f8bef962-11a0-45cf-a8dc-0b74b3a3ca91.png)
  ## STEP 5 — ENABLE PHP ON THE WEBSITE
  In case you want to change this behavior, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive:

sudo vim /etc/apache2/mods-enabled/dir.conf
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
After saving and closing the file, you will need to reload Apache so the changes take effect:

sudo systemctl reload apache2
Finally, we will create a PHP script to test that PHP is correctly installed and configured on your server.

Now that you have a custom location to host your website’s files and folders, we’ll create a PHP test script to confirm that Apache is able to handle and process requests for PHP files.

Create a new file named index.php inside your custom web root folder:

vim /var/www/projectlamp/index.php
This will open a blank file. Add the following text, which is valid PHP code, inside the file:

<?php
phpinfo();
When you are finished, save and close the file, refresh the page and you will see a page similar to this:
![Screenshot 2021-09-08 093428](https://user-images.githubusercontent.com/76639282/132589373-09898f3f-0521-4adf-b991-9fc334013390.png)
This page provides information about your server from the perspective of PHP. It is useful for debugging and to ensure that your settings are being applied correctly.

If you can see this page in your browser, then your PHP installation is working as expected.

After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment -and your Ubuntu server. You can use rm to do so:

sudo rm /var/www/projectlamp/index.php
You can always recreate this page if you need to access the information again later.





