This playbook is based on:
https://github.com/ansible/ansible-examples/tree/master/lamp_simple_rhel7

``
docker run --rm --privileged -p 5000:80 -v /sys/fs/cgroup:/sys/fs/cgroup:ro --name=lamp-1 --network ansible -d kodekloud/centos-ssh-enabled:master
``

Remember to set innodb buffer pool size low to reduce memory consumption

## Tasks:

#### Common
1. Install Dependencies: libselinux-python, libsemanage-python, firewalld
Let us continue to improve our LAMP stack E-Commerce application. Now that we have learned to develop playbooks and various modules, let's further develop our playbooks to install packages and configure applications.



Let us start with the first step of installing common dependencies on both the servers. We need the following packages installed on both web and db servers.

libselinux-python
libsemanage-python
firewalld.

Playbook: ~/playbooks/lamp-stack-playbooks/deploy-lamp-stack.yml

#### DB Tasks
1. Install MariaDB Packages
2. Configure MySQL Configuration File
3. Create MariaDB LogFiles
4. Create MariaDB PID Directories
5. Restart MariaDB
6. Start Firewalld Service
7. Add Firewalld rule
8. Create Application Database
9. Create Application Database User
#
Let us now configure MariaDB Service on the lamp-db server. Update the playbook to add a play to perform the following tasks on lamp-db
1. Install the following packages:
mariadb-server
MySQL-python
2. Copy the MySQL Configuration file
located at files/my.cnf to /etc/my.cnf
3. Start and enable the mariadb Service
4. Start and enable the firewalld Service
5. Insert firewalld rule
Allow mysql_port - 3306/tcp zone - public
6. Create Application Database
Use the dbname variable from inventory as the name of the database
7. Create Application Database User
Use inventory variables dbuser dbpassword from inventory.
host should be set to IP address of web server 172.20.1.100
priv should be set to *.*:ALL
8. Copy db-load-script.sql file to /tmp directory on the database server
9. Load Inventory data by running the below shell command on the database server
mysql -f < /tmp/db-load-script.sql

Playbook: ~/playbooks/lamp-stack-playbooks/deploy-lamp-stack.yml



#### Web Tasks
1. Install httpd and PHP dependencies
2. Install git for downloading source code
3. Install firewalld
4. Start firewalld status
5. Add firewalld rule
6. Start httpd service
7. Download source code from repo
8. Create index.php file from template.

# 
 1. Install Web Server
Install httpd, php and php-mysql packages
2. Install Git to download source code
package: git
3. Start and enable the firewalld service
4. Insert firewalld rule for httpd
For port use httpd_port/tcp variable.
4. Set index.php as the default page
Modify line "DirectoryIndex index.html" to "DirectoryIndex index.php" in file /etc/httpd/conf/httpd.conf
5. Start and enable the httpd service
6. Clone the source code from the repository
The address of the URL is given in the inventory file. Use the right variable. Clone it to /var/www/html/
7. Copy the custom index.php file
from files/index.php to /var/www/html/index.php
Playbook: ~/playbooks/lamp-stack-playbooks/deploy-lamp-stack.yml


