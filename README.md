Linux Server Configuration Project
what the project aim to ?
here , we want to deploy project to the linux server machine with all requirements such as securing it and updating the database, here the details to accses the project on the internet:

The IP Address: http://34.220.97.82/
SSH Port: 2200
URL using DNS: ec2-34-220-97-82.us-west-2.compute.amazonaws.com


We should connect to the server machine using these two commands
to access the file which contains the pair key of the srever machine Downloads here is the directory name ahmed_udacity.pem is the file name

chmod 600 ~/Downloads/ahmed_udacity.pem
 ssh -i ~/Downloads/ahmed_udacity.pem ubuntu@34.220.97.82
How to configure the server ?
1. we should update all packages
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
and we should enable automatic security updates

sudo apt-get install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
2. we should change timezone to UTC and fix language issues
sudo timedatectl set-timezone UTC
sudo update-locale LANG=en_US.utf8 LANGUAGE=en_US.utf8 LC_ALL=en_US.utf8
3. here, we create a new user grader and give him sudo access
sudo adduser grader
sudo nano /etc/sudoers.d/grader 
to be enabled to get the required premissions, you should add the following text: grader ALL=(ALL) ALL

4. here, we setup SSH keys for grader
On local machine (not server machine) ssh-keygen Then choose the path for storing public and private keys
then on remote machine home as user grader
sudo su - grader
make new directory called .ssh

mkdir .ssh
make new file in the .ssh directory called authorized_keys

touch .ssh/authorized_keys 
then we want to change the mode of accessing the directory an the file

sudo chmod 700 .ssh
sudo chmod 600 .ssh/authorized_keys
then let's edit the authorized key file

nano .ssh/authorized_keys 
Then we should paste here the contents of the public key created on the local machine

5. then we want to change the SSH port from 22 to 2200 , enforce key-based authentication , disable login for the root user
sudo nano /etc/ssh/sshd_config
Then change the following:

Find the Port line and edit it to 2200 and remove the # letter from the beginning of the line
Find the PasswordAuthentication line and edit it to no.
Find the PermitRootLogin line and edit it to no.
Save the file and run sudo service ssh restart to get the required changes enabled
6. we should configure the uncomplicated firewall (UFW)
sudo ufw default deny incoming
sudo ufw default allow outgoing
here we want to allow port 2200 with tcp protcol and allow www and ntp (network time protocol) and enable the firewall

sudo ufw allow 2200/tcp
sudo ufw allow www
sudo ufw allow ntp
sudo ufw enable
7. we want to install Apache2 and mod-wsgi for python2 and Git
sudo apt-get install apache2 libapache2-mod-wsgi-py2 git
8. here, we install and configure PostgreSQL
sudo apt-get install libpq-dev python2-dev
sudo apt-get install postgresql postgresql-contrib
sudo su - postgres
psql
Then we write :

CREATE USER catalog WITH PASSWORD '12345';
CREATE DATABASE catalog WITH OWNER catalog;
\c catalog
REVOKE ALL ON SCHEMA public FROM public;
GRANT ALL ON SCHEMA public TO catalog;
\q
exit
Note: In your catalog project you should change database engine in project.py & lotsofmenu.py & database_setup.py to

engine = create_engine('postgresql://catalog:12345@localhost/catalog')
9. then we should Clone the Catalog app ( the required app to be deployed to linux server machine ) from GitHub and Configure it
cd /var/www/
sudo mkdir catalog
sudo chown grader:grader catalog
git clone https://github.com/ahmedattar/Item-Catalog
cd catalog
nano catalog.wsgi
Then we should add the following content in catalog.wsgi file

#!/usr/bin/python2
import sys
sys.stdout = sys.stderr
sys.path.insert(0,"/var/www/catalog")
from project import app as application
we here , want to Install app dependencies

sudo apt-get -qqy install python python-pip
sudo -H pip2 install --upgrade pip
sudo -H pip2 install flask packaging oauth2client redis passlib flask-httpauth
sudo -H pip2 install sqlalchemy flask-sqlalchemy psycopg2 bleach requests
10. here , we configure apache server
sudo nano /etc/apache2/sites-enabled/000-default.conf
Then add the following content:

# serve catalog app
<VirtualHost *:80>
  ServerName 34.220.97.82
  ServerAlias ec2-34-220-97-82.us-west-2.compute.amazonaws.com
  ServerAdmin ahmedattar6868@gmail.com
  DocumentRoot /var/www/catalog
  WSGIDaemonProcess catalog user=grader group=grader
  WSGIScriptAlias / /var/www/catalog/catalog.wsgi

  <Directory /var/www/catalog>
    WSGIProcessGroup catalog
    WSGIApplicationGroup %{GLOBAL}
    Require all granted
  </Directory>

  ErrorLog ${APACHE_LOG_DIR}/error.log
  LogLevel warn
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

11. Restart Apache Server to get the final updates
sudo service apache2 restart
The IP Address: 34.220.97.82
URL using DNS: ec2-34-220-97-82.us-west-2.compute.amazonaws.com
what resources , I used in this project ?
Amazon EC2 Linux Instances to know about the instances in amazon 'aws 'and how it is work and how to deal with them.
Apache Server Configuration Files : to know about how to configure Apache server file.
Deploy a Flask Application on an Ubuntu VPS : to know about dploying flask applications on Ubuntu virtual private server
Set Up Apache Virtual Hosts on Ubuntu : to know how to deal with Apache virtual hosts on Ubuntu
mod_wsgi documentation , Flask mod_wsgi (Apache) : to deal with mod_wsgi files , which are used to configure Apache servers
Automatic Security Updates for the system : to make the system is updated continously to fight any probable security risk.
Stack Overflow Website : for any problem in your code, you can ask in this important site about it , this website has many programmers and experts to answer you.
Ask Ubuntu Website : for any problem in ubuntu (linux distribution) and how to deal with it.
