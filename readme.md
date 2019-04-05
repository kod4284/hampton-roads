# Hampton Roads
-------------------

Introduction
------------

ECO Districts Hampton Roads is a progressive project in Hampton Roads, Virginia. The
building foundation for ECO Districts derives an emphasis on three imperatives--climate action, resilience, and equity. With over 5,000 homes and 12,000 residents in the community, in order to progressively transform a community of this size, communication is key. With the push of modern technology through websites and mobile applications today, the managing director of ECO Districts Hampton Roads, Gary Harris, decided that a web-based platform would promote community involvement which is where our team plays a role.


Server Configuration
--------------------
Linux Sever Configuration for Hampton Roads application

The Hampton Roads application is now running on the Linux sever on the Apache2 environment.

Visit http://hamptonroads.ga to see it.

If the link doesn't work, try http://50.17.107.48.xip.io

Note: if you use your personal VM environment like vagrant or VMware, go to see "Keep the sever up-to-date" section

Server Information
------------------
public IP Address: 50.17.107.48
private key is not provided here

Open ports are:
* SSH Port: 22
* HTTP: 80
* NTP: 123
```
To                         Action      From
--                         ------      ----
22                       ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
123                        ALLOW       Anywhere
22 (v6)                  ALLOW       Anywhere (v6)
80/tcp (v6)                ALLOW       Anywhere (v6)
123 (v6)                   ALLOW       Anywhere (v6)
```

Instruction to SSH
------------------
Download the private key and locate it under your path `~/.ssh/`

Open a terminal window and type:
```shell
ssh username@50.17.107.48 -p 22 -i ~/.ssh/keyname
```

Add a new user
--------------
After login-in via the terminal, type:
```shell
sudo adduser <new username>
sudo touch /etc/sudoers.d/<new username>
sudo nano /etc/sudoers.d/<new username>
```
And on the nano widow, type:
```shell
<new username> ALL=(ALL:ALL) ALL
```
type CTRL+X, 'y' and ENTER to save and exit

making new ssh key by using ssh-keygen
--------------------------------------
On the client computer, open terminal and type:
```shell
ssh-keygen
```
By following the step, you can generate private and public key in the path ~/.ssh

Now you should copy the value of key into the server.

On your server side terminal, type
```shell
sudo nano .ssh/authorized_keys
```
Copy the public key value into this and save

Give permission by typing
```shell
chmod 700 .ssh
chmod 644 .ssh/authorized_keys
```

Change the SSH port from 22 to 2200
-----------------------------------
```shell
sudo ufw allow 2200
sudo ufw allow 80
sudo ufw allow 123
sudo ufw enable
```

Keep the sever up-to-date
-------------------------
```shell
sudo apt-get update
sudo apt-get upgrade
```

Upgrade pip
-----------
```shell
python -m pip install --upgrade pip
```

Install Apache2 and mod_wsgi to run Python Flask application
------------------------------------------------------------

```shell
sudo apt-get install apache2
sudo apt-get install libapache2-mod-wsgi-py3
sudo apache2ctl restart
```

Install git
-----------
```shell
sudo apt-get install git
```
Install pip
-----------
```shell
sudo apt-get install python-pip
```
```shell
sudo apt-get install python-pip3
```
Install SqlAlchemy
------------------
```shell
sudo -H pip3 install Flask-SQLAlchemy
````

Install Flask
-------------
```shell
pip install flask
```
```shell
pip3 install Flask
```
If those doesn't work try:
For python2:
```shell
pip -H install flask
```
For python3:
```shell
pip3 -H install Flask
```
Install PostgreSQL and set up DB and user
----------------------------------
```shell
sudo apt-get install postgresql
sudo -u postgres createdb hamptonRoads.db
sudo -u postgres createuser daewoong
sudo -u postgres psql
psql=# alter user daewoong with encrypted password daewoong;
psql=# grant all privileges on database hamptonRoads.db to daewoong;
```

Edit Apache configuation file
----------------------
To edit:
```shell
sudo nano /etc/apache2/sites-enabled/000-default.conf
```
Add the lines of the codes into the file:
```
<VirtualHost *:80>
DocumentRoot /var/www/html
WSGIScriptAlias / /var/www/html/app.wsgi
        <Directory /var/www/html/>
            Order allow,deny
            Allow from all
         </Directory>
</VirtualHost>

```
Create the .wsgi File
---------------------
```
import sys
sys.path.insert(0, '/var/www/html/')
from __init__ import app as application
application.secret_key = 'super_secret_key'
```

See error log
-------------
```shell
sudo tail /var/log/apache2/error.log
```


Summary of the third party resources used
-----------------------------------------
When linux server is not updated: https://serverfault.com/questions/265410/ubuntu-server-message-says-packages-can-be-updated-but-apt-get-does-not-update

How to make username and password for database in PostgreSQL: https://medium.com/coding-blocks/creating-user-database-and-adding-access-on-postgresql-8bfcd2f4a91e

Change ssh port: https://www.godaddy.com/help/changing-the-ssh-port-for-your-linux-server-7306
