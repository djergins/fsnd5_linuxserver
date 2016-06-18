public ip: 52.40.144.216
**Configuration Changes made:**
- 1. Create the grader : adduser grader
- 2. Grader Password: 8E@nbags!
- 3. Validate Creation
root@ip-10-20-28-232:~# cat /etc/passwd | grep grader
grader:x:1000:1000:Udacity,,,:/home/grader:/bin/bash
- 4. Give grader the ability to sudo: visudo
grader  ALL=(ALL:ALL) ALL
- 5. Update list of packages and then upgrade
apt-get update
apt-get upgrade
- 6.change ssh configuration
vim /etc/ssh/sshd_config
Set port to 2200
change PermitRootLogin to no
- 7. Restart SSH Service:
/etc/init.d/ssh restart
- 8. Generate an ssh-key
Switch to local git bash
ssh-keygen
Passphrase:5Pend!ng
ssh-copy-id -i /c/Users/Daniel/.ssh/id_rsa.pub grader@52.39.81.91 -p2200
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
grader@52.39.81.91's password:
Permission denied, please try again.
grader@52.39.81.91's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh -p '2200' 'grader@52.39.81.91'"
and check to make sure that only the key(s) you wanted were added.
- 9. Setup the firewall
sudo ufw allow 2200/tcp
sudo ufw allow 80/tcp
sudo ufw allow 123/udp
sudo ufw enable
- 10. Configure to UTC
sudo dpkg-reconfigure tzdata
choose none of the above then UTC
sudo apt-get install ntp
sudo vim /etc/ntp.conf
replace with North American servers:
server 0.north-america.pool.ntp.org
server 1.north-america.pool.ntp.org
server 2.north-america.pool.ntp.org
server 3.north-america.pool.ntp.org
(apt-get in front of installs)
- 11. Install apache2
- 12. Install libapache2-mod-wsgi
- 13. Install git
- 14. Install python-dev (for flask)
- 15. install python-pip
- 16. install Flask
- 17. create myapp.wsgi in /var/www/html
- 18. modify /etc/apache2/sites-enabled/000-default.conf to contain WSGIScriptAlias / /var/www/html/myapp.wsgi 
- 19. restart apache: apache2ctl restart
- 20. create finalproject directory in /var/www/html
- 21. cd ~, git clone https://github.com/djergins/fullstack-nanodegree-vm.git
- 22. have to change paths in open functions in finalproject.py e.g. open(/var/www/html/finalproject/client_secrets.json). On server they can't be found otherwise.
- 23. Install postgresql
- 24. cd ~
- 25.  sudo cp -r fullstack-nanodegree-vm/vagrant/catalog/finalproject/ /var/www/html/finalproject/
- 26. pip install Flask-SQLAlchemy
- 27. pip install sqlalchemy_utils
- 28. apt-get install python-psycopg2
- 29. apt-get isntall libpq-dev
- 30. Perform initial configuration of postgresql(http://stackoverflow.com/questions/1471571/how-to-configure-postgresql-for-the-first-time)
ALTER USER postgres with encrypted password 'Cr0ssmen2013!';
sudo vim /etc/postgresql/9.1/main/pg_hba.conf
local      all     postgres     peer md5
sudo /etc/init.d/postgresql restart
createuser -U postgres -d -e -E -l -P -r -s grader
create restaurantmenuwithusers as catalog and grant necessary privileges:
                                    Access privileges
 Schema |       Name        |   Type   |   Access privileges   | Column access privileges
--------+-------------------+----------+-----------------------+--------------------------
 public | menu_item         | table    | catalog=arwdx/catalog |
 public | menu_item_id_seq  | sequence | catalog=rwU/catalog   |
 public | restaurant        | table    | catalog=arwdx/catalog |
 public | restaurant_id_seq | sequence | catalog=rwU/catalog   |
 public | user              | table    | catalog=arwdx/catalog |
 public | user_id_seq       | sequence | catalog=rwU/catalog   |
(6 rows)
- 31. install oauth2client
- 32. Add server to google developer console for oauth2 success
- 33. Add server to facebook developer console for oauth2 success.
- 34. Install the following versions to get authentication working properly:

pip install werkzeug==0.8.3
pip install flask==0.9
pip install Flask-Login==0.1.3

- 35. Visit http://ec2-52-39-81-91.us-west-2.compute.amazonaws.com/ and validate that the application is working properly.
- 36.Profit!

**Software Installed:**
ntp
apache2
libapache2-mod-wsgi
python-setuptools
git
python-dev
pip
Flask
postgresql
Flask-SQLAlchemy
sqlalchemy_utils
python-psycopg2
libpq-dev
oauth2client

**Third-Party Sources Used:**

stackoverflow.com (for the many problems I hit with apache2, postgres, and setting my app up to work with both.)
postgresql.org(documentation, mainly for granting of priviliges.)
Udacity Forums(For the point where I corrupted my sudoers.d file and had to start over from scratch. Good to know I'm not the only one who totally borked my box.)

sqlalchemy sqlalchemy.org
http://sqlalchemy-utils.readthedocs.io/en/latest/
Used both of the above to set up database creation if it didn't exist and figure out how to convert my connection string from an sqlite connection string to a postgres connection string.

https://help.ubuntu.com/community/AutomaticSecurityUpdates -how to automatically update your system.

SSH-Key Generation: https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server

Set up local UTC:
https://help.ubuntu.com/community/UbuntuTime#Using_the_Command_Line_.28terminal.29