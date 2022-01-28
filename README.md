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
  <img width="450" src="https://github.com/nuclearcheesecake/lampstacks/blob/main/images/image%20(12).png">
</p>

This means that your web server is running. Great! We can see the default Apache page in-browser then, by typing "http://" + your server's local IP, or just "localhost/". It should display like this:

<p align="center">
  <img width="450" src="https://github.com/nuclearcheesecake/lampstacks/blob/main/images/image%20(13).png">
</p>

We can use Apache's Root Directory to include files that will become other pages on this server. Usually, this directory is /var/www/html/ - any files in here will be processed as part of the web server.


<a name="4"></a>
## 1.3 Building the database

Now we can work with our data. For this exercise, I have created a database (called "rftest") with the following structure:

Tables:
- Sale (sale_ID, prod_ID, sm_ID, sale_quantity, sale_total)
- Salesman (sm_ID, sm_fullname, sm_commission)
- Product (prod_ID, prod_name, prod_price, prod_stock)

I also create two triggers:
- updatestock, so that when a sale is made, the appropriate amount of inventory is deducted from a product's stock in the Product table
- updatecommission, that increments a salesman's commission by 5% of the sale total

We can see the results below. Firstly, we must start and enable MySQL:

```bash
sudo systemctl start mysql
sudo systemctl enable mysql
```

We can then look at what is stored inside of the databse:

<p align="center">
  <img width="450" src="https://github.com/nuclearcheesecake/lampstacks/blob/main/images/image%20(16).png">
</p>
<p align="center">
  <img width="450" src="https://github.com/nuclearcheesecake/lampstacks/blob/main/images/image%20(17).png">
</p>

We can also observe that when a sale is made, the triggers fire:

<p align="center">
  <img width="450" src="https://github.com/nuclearcheesecake/lampstacks/blob/main/images/image%20(19).png">
</p>
<p align="center">
  <img width="450" src="https://github.com/nuclearcheesecake/lampstacks/blob/main/images/image%20(20).png">
</p>



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


The triggers were created as:

- updatestock
```sql
CREATE TRIGGER dbo.updatestock ON dbo.sale FOR INSERT
AS
declare @prod_ID int;
declare @sale_quantity FLOAT (20);

select @prod_ID=i.prod_ID from inserted i;
select @sale_quantity=i.sale_quantity from inserted i;

UPDATE dbo.product
SET prod_stock = (SELECT prod_stock from dbo.product where prod_ID = @prod_id) - @sale_quantity
WHERE prod_ID = @prod_ID;
```

- updatecommission
```sql
CREATE TRIGGER dbo.updatecommission ON dbo.sale FOR INSERT
AS
declare @sm_ID int;
declare @sale_total FLOAT (20);

select @sm_IDint=i.sm_IDint from inserted i;
select @sale_total=i.sale_total from inserted i;

UPDATE dbo.salesman
SET sm_commission = (SELECT sm_commission from dbo.salesman where sm_ID= @sm_ID) + 0.05*@sale_total
WHERE sm_ID = @sm_ID;
```

<a name="10"></a>
## 2.4 PHP scripting

