---
title: Install PostgreSQL on Kubuntu/Ubuntu 19.10
author: andre
date: '2020-03-03'
slug: install-postgresql-on-kubuntu-ubuntu-19-10
categories:
  - SQL
  - Linux
tags:
  - Kubuntu
  - PostgreSQL
subtitle: ''
summary: ''
authors: []
lastmod: '2020-03-03T07:39:49-05:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
toc: true
toc: float
markup: blackfriday
subtitle: Creating a pace to store your data.
summary: Creating a pace to store your data.
---

{{% toc %}}

# Installation

In this post, I'm documenting my steps for installing [PostgreSQL](https://www.postgresql.org/) on  [Kubuntu 19.10](https://kubuntu.org/news/kubuntu-19-10-is-released-today/) 

Get the latest PostgreSQL using the [PostgreSQL Apt Repository](https://www.postgresql.org/download/linux/ubuntu/). You can do this by completing the following steps in your terminal:

1. Create the file /etc/apt/sources.list.d/pgdg.list using the Nano text editor. 

```{bash eval=FALSE, echo=TRUE}
cd /etc/apt/sources.list.d/
sudo nano pgdg.list
```
2. Paste the following code to the text file:

`deb http://apt.postgresql.org/pub/repos/apt/ eoan-pgdg main`

Pres Ctrl+o to save the file and Ctrl+x to exit nano. 

3. Import the repository signing key, and update the package lists

```{bash eval=FALSE, echo=TRUE}
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add
```
4. Update your Ubuntu repository information. 
```{bash eval=FALSE, echo=TRUE}
sudo apt-get update
```
5. Install the latest version of PostgreSQL. At the time I wrote this, the latest available stable version is version 12. 

```{bash eval=FALSE, echo=TRUE}
sudo apt-get install postgresql-12
```

6.  After completing the installation, you should get the following output in your terminal: 

```{bash eval=FALSE, echo=TRUE}
Success. You can now start the database server using:

    pg_ctlcluster 12 main start

Ver Cluster Port Status Owner    Data directory              Log file
12  main    5432 down   postgres /var/lib/postgresql/12/main /var/log/postgresql/postgresql-12-main.log
```
7.  You can run the following command to confirm that PostgreSQL is installed and to identify the installed version. 

```{bash eval=FALSE, echo=TRUE}
sudo -u postgres psql -c "SELECT version();"
PostgreSQL 12.2 (Ubuntu 12.2-2.pgdg19.10+1) on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 9.2.1-9ubuntu2) 9.2.1 20191008, 64-bit
(1 row)
```
8. Start the database server using the instructions provided in the previous step.
```{bash eval=FALSE, echo=TRUE}
sudo systemctl start postgresql@12-main
```
9. During installation, a default `postgres` super user was created. You can change to this super user and open PostgreSQL in your terminal using the following commands:
```{bash eval=FALSE, echo=TRUE}
sudo su - postgres
psql
```
10. Quit PostgreSQL(psql) using the following:
```{bash eval=FALSE, echo=TRUE}
\q
```
11. Then switch to your Ubuntu user account (my account name is dre) and create a PostgreSQL user (I'm using dre again). 
```{bash eval=FALSE, echo=TRUE}
su dre
sudo su - postgres -c "createuser dre"
```
12. Create a database. I'm creating a database called Cincinnati. In this database, I'm going to store all my Cincinnati data. 
```{bash eval=FALSE, echo=TRUE}
sudo su - postgres -c "createdb cincinnati"
```
13. Activate the postgres super user and open PostgreSQL. 
```{bash eval=FALSE, echo=TRUE}
sudo -u postgres psql
```
14. Grant the PostgreSQL user you created superuser privileges to access the database you created. 
```{bash eval=FALSE, echo=TRUE}
grant all privileges on database cincinnati to dre;
```
15. Create a password for your user. You won't be able to access the database without it. I've also created a password for the postgres superuser. 
```{bash eval=FALSE, echo=TRUE}
ALTER USER dre PASSWORD 'password';
ALTER USER postgres PASSWORD '<My Root Password>';
```
You should now be able to access your database through [pgAdmin](https://www.pgadmin.org/) or [DBeaver](https://dbeaver.io/).

# Remove PostgreSQL
To remove PostgreSQL, you can do the following in your terminal. 

1. If you like, remove all your databases  with:
```{bash eval=FALSE, echo=TRUE}
sudo su - postgres -c "dropdb cincinnati"
```

2. Use the dpkg command to list all the PostgreSQL packages and server versions. 

```{bash eval=FALSE, echo=TRUE}
dpkg -l | grep postgres
```

3. Delete the configuration and database files.
`sudo pg_dropcluster --stop <version> <cluster>` 
```{bash eval=FALSE, echo=TRUE}
sudo pg_dropcluster --stop 11 main
sudo pg_dropcluster --stop 12 main
```
4. Use a wild card search to remove all PostgreSQL packages.

```{bash eval=FALSE, echo=TRUE}
sudo apt-get --purge remove postgresql\*
```
5. After removing the PostgreSQL packages, delete the PostgreSQL folders, users, and groups. 

```{bash eval=FALSE, echo=TRUE}
sudo rm -r /etc/postgresql/
sudo rm -r /etc/postgresql-common/
sudo rm -r /var/lib/postgresql/
sudo userdel -r postgres
sudo groupdel postgres
sudo userdel -r dre
sudo groupdel dre
```

# Importing Data

In future posts, I'll show how to import data used for analysis into the PostgreSQL database I've created. 


# References
1. [How To Remove PostgreSQL](https://www.liquidweb.com/kb/how-to-remove-postgresql/) 

1. [How to thoroughly purge and reinstall PostgreSQL on ubuntu?](https://stackoverflow.com/questions/2748607/how-to-thoroughly-purge-and-reinstall-postgresql-on-ubuntu)

1. [Linux downloads (Ubuntu)](https://www.postgresql.org/download/linux/ubuntu/)

1. [3 Ways to Create a Text File Quickly Through the Linux Terminal](https://vitux.com/3-ways-to-create-a-text-file-quickly-through-the-linux-terminal/)

1. [How to Use Nano, the Linux Command Line Text Editor](https://linuxize.com/post/how-to-use-nano-text-editor/)

1. [How to Install PostgreSQL on Ubuntu 18.04](https://linuxize.com/post/how-to-install-postgresql-on-ubuntu-18-04/)

1. [PostgreSQL: dropdb](https://www.postgresql.org/docs/9.1/app-dropdb.html)

