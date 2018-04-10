# 1. Installing Openifire

## a. Run command for Installing OpenFire( require java to be preinstalled)
```bash
sudo apt-get install gdebi

sudo wget [http://download.igniterealtime.org/openfire/openfire_4.2.3_all.deb](http://download.igniterealtime.org/openfire/openfire_4.2.3_all.deb)

sudo gdebi openfire_4.2.3_all.deb
```

## b . Run the command for installing xamp if not installed
 src https://linoxide.com/ubuntu-how-to/install-xampp-stack-ubuntu-16-04-terminal/
 
  _Change the version in to latest of xamp server available and *mysql should be installed on it*

## c. Start

## c. Configuring the mysql and importing the database
   ## 1) Importing Data to mysql server
  ### i) Login to mysql
```bash
mysql -u root -p
```
### ii) Create Database name openfire
```bash
  CREATE DATABASE openfire;
```
### iii) Exit mysql and after pasting the openfire.sql file into the directory run

```bash 
mysql -u root -p openfire < /home/ubuntu/openFireNew/openfire.sql
```
### iii) Check database
```
show databases;
use openfire;
show tables; 
```

