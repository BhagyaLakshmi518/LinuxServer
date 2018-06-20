# LinuxServer
About Server
## This is the sixth project of Udacity Full Stack Nanodegree course.
### This project is about Configuring the linux servers.
### Details About The Server:
Server IP address: 
##### UrlSite : http://13.232.90.166.xip.io
##### open awazon lightsail.com and create an instance that we want for project and static ip address for it.
### To connect to grader:
#### Download the file that is having our private key when we created our instance in the lightsail amazon.com
#### Save the private key provided in your machine and run the command as follows:
      ssh -i path/to/privatekey -p 2200 grader@ipaddress
#### To get Update all files:
      sudo apt-get update
      sudo apt-get upgrade
#### To create grader user:
      sudo adduser grader
#### To get another new user:
      sudo nano /etc/sudoers
#### To grant permissions:
      grader  ALL=(ALL:ALL) ALL
#### To create ssh keypair grader:
      ssh-keygen
#### This generate the public key and private key to the .ssh folder
#### To get change to the new user:
     su - grader
#### create a new folder .ssh and new authorized_file:
      mkdir .ssh
#### copy the public keys with .pub extension and save the authorized file
      sudo nano .ssh/authorized_keys
#### To get permissions for to user:
      chmod 700 .ssh
#### To prevent other user to write on file:
      chmod 644 .ssh/authorized_keys
#### Then we can restart our server by using coomand:
      service ssh restart
#### Now we get log in to the grader with our private key generated:
      ssh -i .ssh/id_rsa grader@ipaddress 
#### Now changing the port number of ssh:
      sudo nano /etc/ssh/sshd_config
      change the port number from 22 to 2200
#### Now get login by using this command:
      ssh -i .ssh/id_rsa -p 2200 grader@ipaddress
#### To make changes for Root:
      sudo nano /etc/ssh/sshd_config
#### To get security for firewall:
      sudo ufw allow 2200/tcp
      sudo ufw allow 80/tcp
      sudo ufw allow 123/udp
      sudo ufw enable
#### To check status:
      sudo ufw status
#### To get time and day of our configuration:
      sudo dpkg-reconfigure tzdata
#### To get install for apache2:
      sudo apt-get install apache2
#### To get mod_wsgi
      sudo apt-get install python-setuptools libapache2-mod-wsgi
#### To get enable:
      sudo a2enmod wsgi
#### Create a new directory named FlaskAPP and go into that directory:
      sudo mkdir FlaskApp
      cd FlaskApp
#### To get git clone we need to install git:
      sudo apt-get install git
#### Now get address of your itemcatalog project from github:
      sudo git clone https://github.com/name/item_catalog.git
#### To change the file name of our main project file:
      sudo mv project.py __init__.py
#### Now open that file and add json file:
      sudo nano __init__.py
      PROJECT_ROOT = os.path.realpath(os.path.dirname(__file__))
      json_url = os.path.join(PROJECT_ROOT, 'client_secrets.json')
#### Then after save and exit:
      ctrl+o
      ctrl+x
#### create wsgi file:
      sudo nano mv project.py FlaskApp.wsgi
      #!/usr/bin/python
      import sys
      import logging
      logging.basicConfig(stream=sys.stderr)
      sys.path.insert(0,"/var/www/FlaskApp/")
      from FlaskApp import app as application
      application.secret_key = 'Add your secret key'
#### create a virtual host file:
      sudo nano /etc/apache2/sites-available/FlaskApp.conf
      <VirtualHost *:80>
 	ServerName mywebsite.com
 	ServerAdmin admin@mywebsite.com
 	WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
 	<Directory /var/www/FlaskApp/FlaskApp/>
 		Order allow,deny
 		Allow from all
 	</Directory>
 	Alias /static /var/www/FlaskApp/FlaskApp/static
 	<Directory /var/www/FlaskApp/FlaskApp/static/>
 		Order allow,deny
 		Allow from all
 	</Directory>
 	ErrorLog ${APACHE_LOG_DIR}/error.log
 	LogLevel warn
 	CustomLog ${APACHE_LOG_DIR}/access.log combined
      </VirtualHost>
 #### To get install certain softwares we use:
      pip install sqlalchemy
      pip install flask
      pip install oauth2client
      pip install pyscopg2
      pip install requests
#### To get connect to database:
##### Install postgresql:
      sudo apt-get install postgresql
##### To get login into postgres:
      sudo su - postgres
##### To get enter into database shell
      psql
##### create user 
      CREATE USER item_catalog WITH PASSWORD 'password';
##### To get Alter:
      ALTER USER item_catalog CREATEDB;
##### To get database name with user:
      CREATE DATABASE item_catalog WITH user item_catalog;
##### To get connect to database:
      \c item_catalog
##### To get revoke from permissions:
      REVOKE ALL ON SCHEMA public FROM public;
##### To grant such permisions:
      GRANT ALL ON SCHEMA public TO catalog;
##### To get exist and logout
      \q
      exit
##### Now change the database connection in both database_setup.py and __init__.py as :
      engine = create_engine('postgresql://item_catalog:password@localhost/item_catalog')
##### Now our appilication is ready.
#### Setting google oauth2client details:
##### Login to your developer console and select the project and edit OAuth details as following:
###### redirect URI
      http://ip.xip.io\login
      http://ip.xip.io\gconnect
      http://ip.xip.io\callback
      
##### Finally we must restart our server:
      sudo service apache2 restart







     


