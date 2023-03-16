# Project-2 
WEB STACK IMPLEMENTATION (LEMP STACK)
In this project you will implement a similar stack as in project-one, but with an alternative Web Server – NGINX, which is also very popular and widely used by many websites in the Internet

Step 1 – Installing the Nginx Web Server
In project-one we used Apache as our webserver. An alternative is Nginx. Nginx is known for its ability to handle multiple open connections effectively.

To Update your Package Repository run the command below:

sudo apt update

To install Nginx run the command below:

sudo apt install nginx

To verify that nginx was successfully installed and is running as a service in Ubuntu, run:

sudo systemctl status nginx

You should the something like the screenshot below:

![new project 2(1)](https://user-images.githubusercontent.com/112444993/225490628-033a0c83-ca0d-4329-ae1a-296c544bf2c4.jpg)


Confirm that Nginx can handle request from the internet by lunching it on the browser using the URL below:

http://<Public-IP-Address>:80

You should find something like the screenshot below if nginx is running:

![new project 2(2)](https://user-images.githubusercontent.com/112444993/225490376-45597f9c-6788-4a34-b0ca-c66b34fc46b9.png)

nginx

Step 2 — Installing MySQL
Run the command below to install mysql:

sudo apt install mysql-server

When prompted, confirm installation by typing Y, and then ENTER.

To secure your database run the preinstalled script below:

sudo mysql_secure_installation

This will ask if you want to configure the VALIDATE PASSWORD PLUGIN. Answer Y for yes or any character for No. Answer Y subsequently when prompted to complete the process.

Run the command below to confirm mysql is running:

sudo systemctl status mysql and Confirm that you have access to the database by running the command below:
"sudo mysql", it will bring this output.

![new pro 2(5)](https://user-images.githubusercontent.com/112444993/225491898-95dd35d5-d3da-4e3c-a4a9-742f75ce9efb.jpg)
Exit the database with the command below:
exit.

Step 3 – Installing PHP
While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. You’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.

To install these 2 packages at once, run:

sudo apt install "php-fpm php-mysql" and it will bring the output below:

![new pro 2(5)](https://user-images.githubusercontent.com/112444993/225492477-8ac77aae-b784-4ae1-86d4-3923eb774992.jpg)
When prompted, type Y and press ENTER to confirm installation.

Step 4 — Configuring Nginx to Use PHP Processor
When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. In this guide, we will use projectLEMP as an example domain name.

On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instead of modifying /var/www/html, we’ll create a directory structure within /var/www for the your_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites.

Create the root web directory for your_domain as follows:

sudo mkdir /var/www/projectLEMP

Set logged-in user as the owner of the directory /var/www/projectLEMP/

sudo chown -R $USER:$USER /var/www/projectLEMP

open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor.

sudo nano /etc/nginx/sites-available/projectLEMP
This will create a new blank file. Paste in the following bare-bones configuration: and it will look like these

![new pro (6)](https://user-images.githubusercontent.com/112444993/225493693-e6f84d00-8e36-4b2f-a5e8-8644c0a7a7e6.jpg)




Here’s what each of these directives and location blocks do:

listen — Defines what port Nginx will listen on. In this case, it will listen on port 80, the default port for HTTP. root — Defines the document root where the files served by this website are stored. index — Defines in which order Nginx will prioritize index files for this website. It is a common practice to list index.html files with a higher precedence than index.php files to allow for quickly setting up a maintenance landing page in PHP applications. You can adjust these settings to better suit your application needs. server_name — Defines which domain names and/or IP addresses this server block should respond for. Point this directive to your server’s domain name or public IP address.

location / — The first location block includes a try_files directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate resource, it will return a 404 error.

location ~ .php$ — This location block handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the php7.4-fpm.sock file, which declares what socket is associated with php-fpm.

location ~ /.ht — The last location block deals with .htaccess files, which Nginx does not process. By adding the deny all directive, if any .htaccess files happen to find their way into the document root ,they will not be served to visitors.

When you’re done editing, save and close the file. If you’re using nano, you can do so by typing CTRL+X and then y and ENTER to confirm.

Activate your configuration by linking to the config file from Nginx’s sites-enabled directory:

sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/

You can test your configuration for syntax errors by typing:

sudo nginx -t

You shall see following message:

nginx: the configuration file /etc/nginx/nginx.conf syntax is ok

nginx: configuration file /etc/nginx/nginx.conf test is successful

Disable default Nginx host that is currently configured to listen on port 80, for this run:

sudo unlink /etc/nginx/sites-enabled/default

Reload Nginx to apply the changes:

sudo systemctl reload nginx

Your new website is now active, but the web root /var/www/projectLEMP is still empty. Create an index.html file in that location so that we can test that your new server block works as expected:

sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html

Now go to your browser and try to open your website URL using IP address:

http://<Public-IP-Address>:80 and you will see this below:

![Screenshot 2023-02-13 19143611](https://user-images.githubusercontent.com/112444993/225495933-0e610adf-5435-4916-884c-02c51c00776c.png)

Step 5 – Testing PHP with Nginx
Your LEMP stack should now be completely set up.

At this point, your LAMP stack is completely installed and fully operational.

You can test it to validate that Nginx can correctly hand .php files off to your PHP processor.

You can do this by creating a test PHP file in your document root.

Open a new file called info.php within your document root in your text editor:

sudo vi /var/www/projectLEMP/info.php

paste the following lines below into the new file.

You can access your website with the Url below:

http://server_domain_or_IP/info.php

You should see something like the screenshot below:

![new pro 9](https://user-images.githubusercontent.com/112444993/225496231-04029c8f-955d-4a19-b712-a1956b0c164a.png)
NB:The screenshot above contains sensitive information of your environment. So you should remove it by Running this command :sudo rm /var/www/your_domain/info.php

Step 6 — Retrieving data from MySQL database with PHP
In this step you will create a test database (DB) with simple "To do list" and configure access to it, so the Nginx website would be able to query data from the DB and display it.

First, connect to the MySQL console using the root account:

sudo mysql

To create a new database, run the following command from your MySQL console:

mysql> CREATE DATABASE 'example_database';

To Create a user and password run the command below:

mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';

Now we need to give this user permission over the example_database database:

mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';

To exit mysql as root user, run the command below:

mysql> exit

To test our database, log in as user: example_user using the command below:

mysql -u example_user -p

change into `example_database using the command below:


mysql> SHOW DATABASES; it will bring the output below:

![new pro 2(12)](https://user-images.githubusercontent.com/112444993/225498425-9cb6f24e-3591-47e9-9808-f55514a5d048.jpg)



Next, we’ll create a test table named todo_list. From the MySQL console, run the following statement:

CREATE TABLE example_database.todo_list ( mysql> item_id INT AUTO_INCREMENT, mysql> content VARCHAR(255), mysql> PRIMARY KEY(item_id) mysql> );

Insert a few rows of content in the test table. You might want to repeat the next command a few times, using different VALUES:

mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");

To confirm that the data was successfully saved to your table, run:

mysql>  SELECT * FROM example_database.todo_list; it will bring the following output:

![new pro 2(130](https://user-images.githubusercontent.com/112444993/225498987-3fdbb081-8037-4c8f-9e9a-bbc4b8a709aa.jpg)

After confirming that you have valid data in your test table, you can exit theMySQL console:
mysql> exit

Now you can create a PHP script that will connect to MySQL and query for your content. Create a new PHP file in your custom web root directory using your preferred editor. We’ll use vi for that:

vi /var/www/projectLEMP/todo_list.php

Paste the text below into the blank file

TODO
"; foreach($db->query("SELECT content FROM $table") as $row) { echo "
" . $row['content'] . "
"; } echo "
"; } catch (PDOException $e) { print "Error!: " . $e->getMessage() . "
"; die(); }
Save and close the file when you are done editing.

Access the browser through the URL below:

http://<Public_domain_or_IP>/todo_list.php and you will see something like these

![my new pro(14)](https://user-images.githubusercontent.com/112444993/225509434-fb34af95-25a0-4e33-9838-e8390e4b0520.jpg)





