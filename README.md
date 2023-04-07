
# CLIENT SERVER ARCHITECTURE USING A MYSQL RELATIONAL DATABASE MANAGEMENT SYSTEM

## **Implement a Client Server Architecture using MySQL Database Management System (DBMS)**

- ### To demonstrate a basic client-server using MySQL Relational Database Management System (RDBMS), follow the below instructions ###

- Create and configure two Linux-based virtual servers (EC2 instances in AWS)

```
`mysql server`
`mysql client`
```

- On mysql server Linux Server install MySQL Server software

`sudo apt-get update`

`sudo apt-get install mysql-server`

![mysql-server update status](./images/mysql-server%20update%20status.png)

- Once the installation is complete, enable the MySQL service by running the following command

`sudo systemctl enable mysql`

- You can verify that MySQL is running and accepting connections by running the following command

`sudo systemctl status mysql`

![mysql-server running status](./images/mysql-server%20running%20status.png)

- While installing mqsql-server, the default settings was installed, there is need to run a script to improve the security of mysql-server.
**NB;** When i ran the script `sudo mysql_secure_installation` the operation failed with an error message indicating that the authentication method used for the root user does not store authentication data in the MySQL server. To change authentication parameters for the root user in this scenario, i used the ALTER USER command instead of SET PASSWORD. You can enter the ALTER USER command in the MySQL client, like so: (Rmember to change new password to your desired password)

`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '<new-password>';`

-Then run the command again, press Yes for subsequent prompt.

`sudo mysql_secure_installation`

- On mysql client Linux Server install MySQL **Client** software

`sudo apt-get update`

`sudo apt-get install mysql-client`

![mysql-client update status](./images/mysql-client%20update%20status.png)

- By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses.
Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups.
For extra security, do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the **specific local IP address of your ‘mysql client**


- You might need to configure MySQL server to allow connections from remote hosts. 

`sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`

Replace the **bind- address** from ‘127.0.0.1’ to **‘0.0.0.0’** 

save and exit with :wq!

### Next is to create a database in the mysql-server, with a new username, password and grant the user full privilege on the database. ###

`sudo mysql -p`

- create a new MySQL user called remote_user with the password '<password>' and allow them to connect from any host

`CREATE USER 'remote_user'@'%' IDENTIFIED WITH mysql_native_password BY '<password>';`

-  create a new database called test_db

`CREATE DATABASE test_db;`


- grant all privileges on the 'test_db' database to the 'remote_user' user

`GRANT ALL ON test_db.* TO 'remote_user'@'%' WITH GRANT OPTION;`


- To flush the MySQL privileges to ensure that the changes made to the user privileges are in effect

`FLUSH PRIVILEGES;`


`SHOW DATABASES;`

`exit`


- restart the mysql service

`sudo systemctl restart mysql`


 - From mysql client Linux Server connect remotely to mysql server Database Engine without using SSH.

 `mysql -h <ip-address of the database server> -u remote_user -p`

 ![client-server connected succesfully](./images/show-database%20on%20mysqlclient.png)


 









 *A fully functional MySql Client-Server set up has been successfully deployed.*


## **Thank You!** ##

 











