# Backend-Note
14

It happens when your password is missing.

Steps to change password when you have forgotten:

Stop MySQL Server (on Linux):

sudo systemctl stop mysql
Start the database without loading the grant tables or enabling networking:

sudo mysqld_safe --skip-grant-tables --skip-networking &
The ampersand at the end of this command will make this process run in the
background so you can continue to use your terminal and run #mysql -u root, it will not ask for password.

If you get error like as below:

2018-02-12T08:57:39.826071Z mysqld_safe Directory '/var/run/mysqld' for UNIX
socket file don't exists. mysql -u root ERROR 2002 (HY000): Can't connect to local MySQL server through socket
'/var/run/mysqld/mysqld.sock' (2) [1]+ Exit 1

Make MySQL service directory.

sudo mkdir /var/run/mysqld
Give MySQL user permission to write to the service directory.

sudo chown mysql: /var/run/mysqld
Run the same command in step 2 to run mysql in background.

Run mysql -u root you will get mysql console without entering password.

Run these commands

FLUSH PRIVILEGES;
For MySQL 5.7.6 and newer

ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
For MySQL 5.7.5 and older

SET PASSWORD FOR 'root'@'localhost' = PASSWORD('new_password');
If the ALTER USER command doesn't work use:

UPDATE mysql.user SET authentication_string = PASSWORD('new_password')     WHERE User = 'root' AND Host = 'localhost';
Now exit

To stop instance started manually

sudo kill `cat /var/run/mysqld/mysqld.pid`
Restart mysql

sudo systemctl start mysql
