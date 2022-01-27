# Building LAMP stacks (web server technology)

<p align="center">
  <img width="350" src="https://github.com/nuclearcheesecake/lampstacks/blob/main/images/intro.jpg">
</p>

Working with back-end technology often requires a programmer to find ways to help servers talk to websites, while wrangling data that could provide insight to companies. A LAMP stack is a configuration of different technologies that simplifies this process, allowing data to flow from databases onto webpages, with scripting power to manipulate outputs. To create a LAMP stack, you would need some variation of the following:

- L - A Linux distribution, to run everything on
- A - Apache HTTP web server, to speak to the web
- M - MySQL/MSSQL/Some other database, to store relevant information
- P - PHP/Python, to create useful scripts to work with the data and websites

Here is a helpful visual from [this](https://www.liquidweb.com/kb/what-is-a-lamp-stack/) website, which also provides some further reading. The image is rather helpful to understand the architecture used to create a LAMP stack, and how it all fits together:

<p align="center">
  <img width="325" src="https://github.com/nuclearcheesecake/lampstacks/blob/main/images/expl.jpg">
</p>

So, to implement my own LAMPs, I chose two different approaches. I have worked with Oracle databases and PL/SQL extensively during my studies, thus I branched out to use two types of databases I previously only dabbled in. I also used this chance to work with two new Linux distributions. The LAMPs I built thus used:
- MySQL, implemented on Ubuntu
- Microsoft SQL Server (MSSQL), implemented on CentOS

Let's start stacking, then!

## Table of Contents

1. [Ubuntu 20.04 / MySQL LAMP](#1)
  - 1.1 [List of installed packages](#2)
  - 1.2 [Getting Apache to work](#3)
  - 1.3 [Building the database](#4)
  - 1.4 [PHP scripting](#5)
2. [CentOS 7 / Microsoft SQL Server LAMP](#6)
  - 2.1 [List of installed packages](#7)
  - 2.2 [Getting Apache to work](#8)
  - 2.3 [Building the database](#9)
  - 2.4 [PHP scripting](#10)

<a name="1"></a>
# 1. Ubuntu 20.04 / MySQL LAMP

<a name="2"></a>
## 1.1 List of installed packages

For Ubuntu, outside of all of the dependencies and the pre-installed Ubuntu packages, these are the ones I installed to work with the LAMP:
- apache2
- mysql-server
- php
- libapache2-mod-php
- php-mysql

This is fortunately a rather all-encompassing suite that allowed me to complete all of the following steps. To see how to install and configure these packages, see [here](https://hackernoon.com/how-to-install-and-configure-php-for-apache-and-mysql-wb1m33z1), but it boils down to:

```bash
sudo apt-get install apache2
sudo apt-get install mysql-server
sudo mysql_secure_installation
sudo apt-get install php libapache2-mod-php php-mysql
```

<a name="3"></a>
## 1.2 Getting Apache to work

To set up our web server, run the following commands

```bash
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl status apache2
```

You should then see the following:

<p align="center">
  <img width="325" src="https://github.com/nuclearcheesecake/lampstacks/blob/main/images/expl.jpg">
</p>

This means that your web server is running. Great! We can see the default Apache page in-browser then, by typing "http://" + your server's local IP, or just "localhost/". It should display like this:

<p align="center">
  <img width="325" src="https://github.com/nuclearcheesecake/lampstacks/blob/main/images/expl.jpg">
</p>

We can use Apache's Root Directory to include files that will become other pages on this server. Usually, this directory is /var/www/html/ - any files in here will be processed as part of the web server.


<a name="4"></a>
## 1.3 Building the database

Now we can work with our data. For this exercise, I have created a database with the following structure:

structure

```bash
sudo systemctl start mysql
sudo systemctl enable mysql
```

<a name="5"></a>
## 1.4 PHP scripting

```bash
sudo nano /var/www/html/info.php
```

```php
<?php
phpinfo();
?>
```

<a name="6"></a>
# 2. CentOS 7 / Microsoft SQL Server LAMP

<a name="7"></a>
## 2.1 List of installed packages

<a name="8"></a>
## 2.2 Getting Apache to work

<a name="9"></a>
## 2.3 Building the database

<a name="10"></a>
## 2.4 PHP scripting

