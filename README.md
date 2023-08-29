# cdh

=================INSTALLING MYSQL SERVER AND DAEMONS=========

wget https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm 

md5sum mysql57-community-release-el7-9.noarch.rpm

sudo rpm -ivh mysql57-community-release-el7-9.noarch.rpm

sudo yum install mysql-server

sudo yum install --nogpgcheck mysql-server -y              (in case of rds do only this steps)

sudo systemctl start mysqld

sudo systemctl status mysqld

sudo grep 'temporary password' /var/log/mysqld.log

sudo mysql_secure_installation



Enter password for user root: :paste the temporary password which you get in above command                     P@ssw0rd          
New password: P@ssw0rd
Re-enter new Password: P@ssw0rd
Change the root password? [Y/n] n
Remove anonymous users? [Y/n] y
Disallow root login remotely? [Y/n] y
Remove test database and access it? [Y/n] n
Reload privilege tables n? [Y/n] y

==============CREATING DATABASES ===============

mysql -u root -p            (without rds)

mysql -h mysql-training.cccbgjvd9tbf.ap-south-1.rds.amazonaws.com  -u Admin -p    (for rds)

CREATE DATABASE scm DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON scm.* TO 'scm'@'%' IDENTIFIED BY 'P@ssw0rd';

create database hive DEFAULT CHARACTER SET utf8;
grant all on hive.* TO 'hive'@'%' IDENTIFIED BY 'P@ssw0rd';

create database hue DEFAULT CHARACTER SET utf8;
grant all on hue.* TO 'hue'@'%' IDENTIFIED BY 'P@ssw0rd';

create database rman DEFAULT CHARACTER SET utf8;
grant all on rman.* TO 'rman'@'%' IDENTIFIED BY 'P@ssw0rd';

create database navs DEFAULT CHARACTER SET utf8;
grant all on navs.* TO 'navs'@'%' IDENTIFIED BY 'P@ssw0rd';

create database navms DEFAULT CHARACTER SET utf8;
grant all on navms.* TO 'navms'@'%' IDENTIFIED BY 'P@ssw0rd';

create database oozie DEFAULT CHARACTER SET utf8;
grant all on oozie.* TO 'oozie'@'%' IDENTIFIED BY 'P@ssw0rd';

create database actmo DEFAULT CHARACTER SET utf8;
grant all on actmo.* TO 'actmo'@'%' IDENTIFIED BY 'P@ssw0rd';

create database sentry DEFAULT CHARACTER SET utf8;
grant all on sentry.* TO 'sentry'@'%' IDENTIFIED BY 'P@ssw0rd';

CREATE USER 'temp'@'%' IDENTIFIED BY 'P@ssw0rd';
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'temp'@'%' WITH GRANT OPTION;

show databases;
exit;

---------CONFIGURATION OF CM-------------(webserver ip)
{USE WEB SERVER IPS HERE}

sudo nano /etc/yum.repos.d/cloudera-manager.repo

for 5.16

[cloudera-manager]
# Packages for Cloudera Manager, Version 5, on RedHat or CentOS 7 x86_64
name=Cloudera Manager
baseurl= http://ip-172-31-42-127.ap-south-1.compute.internal/cm5.16/
gpgkey = http://ip-172-31-42-127.ap-south-1.compute.internal/cm5.16/RPM-GPG-KEY-cloudera
gpgcheck = 0

for 6.2.0

[cloudera-manager]
# Packages for Cloudera Manager, Version 5, on RedHat or CentOS 7 x86_64
name=Cloudera Manager
baseurl= http://ip-10-0-1-221.ap-south-1.compute.internal/cm6.2.0/
gpgkey = http://ip-10-0-1-221.ap-south-1.compute.internal/cm6.2.0/RPM-GPG-KEY-cloudera
gpgcheck = 0






sudo yum clean all

sudo yum makecache

sudo yum install java -y

sudo yum install cloudera-manager-server cloudera-manager-daemons -y

sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql -h ip-172-31-40-103.ap-south-1.compute.internal scm scm P@ssw0rd 




sudo /opt/cloudera/cm/schema/scm_prepare_database.sh mysql -h ip-10-0-11-86.ap-south-1.compute.internal scm scm P@ssw0rd (for 6.2.0)




sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql -h mysql-training.cccbgjvd9tbf.ap-south-1.rds.amazonaws.com scm scm P@ssw0rd                 

(use here rds endpoint)


{USE HERE databse create machine IP}

sudo service cloudera-scm-server start

sudo service cloudera-scm-server status


---------TO CHECK STATUS OF CDM -------------

sudo netstat -tulnp


to run a job in cm 
yarn jar /opt/cloudera/parcels/CDH/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar
    

---------ON WEB BROWSER-------------

Public ip:7180 {USE HERE WEB BROWSER}

http://ip-172-31-32-93.ap-south-1.compute.internal/cdh5.16

http://ip-172-31-32-93.ap-south-1.compute.internal/cm5.16/

http://ip-10-0-2-73.ap-south-1.compute.internal/cm5.16/RPM-GPG-KEY-cloudera


---------CLUSTER SETUP
SETUP DATABASES   -------------

{ALWAYS COPY CLOUDERA MANAGER PRIVATE DNS AND PASTE}

USE PASSWORD {P@ssw0rd} 


DC=HADOOPSECURITY,DC=LOCAL
CN=HADOOPSECURITY-HADOOP-AD-CA,DC=HADOOPSECURITY,DC=LOCAL
