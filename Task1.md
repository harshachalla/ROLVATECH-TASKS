# Crate the tables and load 1 gb of data

**1.Install mysql on one node**
* Install mysql by using following command

`sudo yum install mysql-server`

**2.downloded sample csv file** 

**3.log into mysql server by using command `mysql`. If u setup any password command is `mysql -p`**

**4.In mysql create databse by using the command `CREATE DATABASE db1;`(db1 is the database name which i used).**

**5.use db1 by using the command `use db1`.**

**6.now create a table like sample downloaded file structure by using like below,i am going create a table Salesrecord**

CREATE  TABLE salesrecordnew (
  `Region` VARCHAR(150) NOT NULL,
  `Country` VARCHAR(150) NOT NULL,
  `Item Type` VARCHAR(150) NOT NULL,
   `Sales Channel` VARCHAR(150) NOT NULL,
  `Order Priority` VARCHAR(150) NOT NULL,
  `Order Date` TIMESTAMP,
  `Order ID` int,
  `Ship Date` TIMESTAMP,
  `Units Sold` int,
  `Unit Price` int,
  `Unit Cost` int,
  `Total Revenue` int,
  `Total Cost` int,
  `Total Profit` int )

**7.now use `show tables;` command to view what are tables in the database.**

**8.now load the sample csv file which is in our local system in to virtual machine by using following steps**

* step-a: open ssh terminal 
* step-b:top-right-corner click on settings symbol there u can see upload file so u upload by this way it will take some time 

***9.To find the path of csv file where it is stored use the following command `find -name /home "*.csv"`**

**10.you can see the file path so now load the file in to mysql by using below command**

LOAD DATA Local INFILE '/home/dubbayugandhar/sales-record2.csv' 
INTO TABLE salesrecordnew
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

**11.using sqoop tool to load the mysql data in to hdfs by using folowing command**

**`sqoop import --connect jdbc:mysql://localhost:3306/db1 --username root --password hadoop123 --table Salesrecord --m 1`**

* --m 1(used to make all the data in one table)

* master-node(local host name) 3306(portnumber oflocal host)

**12.to grant permission of virtualmachine following command is used**

* step-1
`ifconfig`

* step-2
`find the net number`

* step-3
`grant all privileges on *.* to 'root'@'10.160.0.2' IDENTIFIED BY 'hadoop123' WITH GRANT OPTION`


* hadoop123 is my password

**12.so the data is present /user/root folder**


**13.creating hive external table by using the following syntax**

CREATE EXTERNAL TABLE IF NOT EXISTS salesrecord
(`Region` string,
  `Country` string,
  `Item Type` string,
   `Sales Channel` string,
  `Order Priority` string,
  `Order Date` int,
  `Order ID` int,
  `Ship Date` int,
  `Units Sold` int,
  `Unit Price` int,
  `Unit Cost` int,
  `Total Revenue` int,
  `Total Cost` int,
  `Total Profit` int
  )
  ROW FORMAT DELIMITED
  FIELDS TERMINATED BY ','
  STORED AS TEXTFILE
  LOCATION '/user/root/Salesrecord';
