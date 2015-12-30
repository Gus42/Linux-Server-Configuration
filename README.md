# Linux-Server-Configuration
Udacity P5, Full Stack - Configure a Server with Linux

###i. 
The IP address: 52.33.224.137 

###ii. 
SSH port: 2200

###iii. 
http://52.33.224.137 ot http://ec2-52-33-224-137.us-west-2.compute.amazonaws.com

###iv. 
##SW Intsalled:
-apt-get apache2

-apt-get install libapache2-mod-wsgi

-apt-get install postgresql

-apt-get install git

-apt-get install libapache2-mod-wsgi python-dev

-apt-get install python-pip

-apt-get install python-psycopg2

-apt-get install libpq-dev python-dev

-pip install virtualenv

-pip install requests

-pip install sqlalchemy

-pip install Flask

-pip install httplib2

-pip install oauth2client

-pip install werkzeug==0.8.3 //theese last three fixed a problem with the google sign-in

-pip install flask==0.9

-pip install Flask-Login==0.1.3

## Main commands and changes
###Create a new user named grader: 
`add user grader`
###Give the grader the permission to sudo: 
`visudo` add "grader ALL=(ALL:ALL) ALL" after the root
And create the ssh to allow the remote login of grader 
/home/grader/.ssh/authorized_keys `chmod 700 .ssh` `chmod 644 .ssh/authorized_keys`
###Update all currently installed packages:
`apt-get update` and `apt-get upgrade`
###Change the SSH port from 22 to 2200:
`nano /etc/ssh/sshd_config`
set port 2200 and PermitRootLogin no
###Configure the Uncomplicated Firewall:
`ufw default deny incoming`
`ufw default allow outgoing`
`ufw allow 2200/tcp`
`ufw allow 80/tcp`
`ufw allow 123/udp`
`ufw enable`
###Configure the local timezone to UTC:
It was alredy set to UTC, typing `date` -> Wed Dec 30 12:55:43 UTC 2015
###Install and configure Apache to serve a Python mod_wsgi application:
Install Apache and mod_wsgi
`nano /etc/apache2/sites-available/catalog.conf` and paste:
<VirtualHost *:80>
      ServerName 52.33.224.137
      ServerAdmin admin@52.33.224.137
      ServerAlias ec2-52-33-224-137.us-west-2.compute.amazonaws.com
      WSGIScriptAlias / /var/www/catalog/catalog.wsgi
      <Directory /var/www/catalog/catalog/>
          Order allow,deny
          Allow from all
      </Directory>
      Alias /static /var/www/catalog/catalog/static
      <Directory /var/www/catalog/catalog/static/>
          Order allow,deny
          Allow from all
      </Directory>
      ErrorLog ${APACHE_LOG_DIR}/error.log
      LogLevel warn
      CustomLog ${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>

`a2ensite catalog`
in /var/www/catalog `nano catalog.wsgi` and paste:
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/catalog/")
from catalog import app as application
application.secret_key = 'asdeggio'

`service apache2 restart`
###Install and configure PostgreSQL:
`adduser catalog`  `su - postgre` `psql` and create the database with the password dbpass and connect to it

###Install git, clone and setup your Catalog App project:
Install and `git config --global user.name "Gus42"` and `git config --global user.email "gus815@gmail.com"`
`git clone https://github.com/Gus42/Linux-Server-Configuration.git` and move to directory /var/www/catalog/catalog/
in /var/www/catalog `nano .htaccess` and paste "RedirectMatch 404 /\.git"
For the oauth of google: in the dev console, per this app, in credits, in js origins insert http://52.33.224.137 and http://ec2-52-33-224-137.us-west-2.compute.amazonaws.com end for the redirect uri insert http://ec2-52-33-224-137.us-west-2.compute.amazonaws.com/oauth2callback
For the oauth of facebook: insert for the app url: http://52.33.224.137
then:
`service apache2 restart`

###v.  

- http://askubuntu.com/

- https://www.google.co.uk

- https://discussions.udacity.com/c/nd004-p5-linux-based-server-configuration

- http://stackoverflow.com/

- https://pypi.python.org/

- http://flask-sqlalchemy.pocoo.org/2.1/

- http://flask.pocoo.org/docs/0.10/

- https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server

- https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-12-04

- https://www.digitalocean.com/community/tutorials/how-to-setup-a-firewall-with-ufw-on-an-ubuntu-and-debian-cloud-server
