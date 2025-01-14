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

SET PASSWORD FOR 'root'@'localhost' = PASSWORD('new_password');  curl -i -m 60 -X POST http://localhost:8001/certificates 
-F "cert=$(cat cert.pem)" 
-F "key=$(cat key.pem)" 
-F "snis=domain.net"
If the ALTER USER command doesn't work use:  

UPDATE mysql.user SET authentication_string = PASSWORD('new_password')     WHERE User = 'root' AND Host = 'localhost';  
Now exit  

To stop instance started manually  

sudo kill `cat /var/run/mysqld/mysqld.pid`  
Restart mysql  

sudo systemctl start mysql  


heroku run gunicorn echobot:app # run with cloud src at local  
heroku local echobot:app # run on local src  
...heroku git:remote -a your-first-heroku-app  
#install postgre  
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04  
#Install VertualEnviroment   
https://stackoverflow.com/questions/13855463/bash-mkvirtualenv-command-not-found  
Since I just went though a drag, I'll try to write the answer I'd have wished for two hours ago. This is for people who   don't just want the copy&paste solution  

First: Do you wonder why copying and pasting paths works for some people while it doesn't work for others?** The main reason, solutions differ are different python versions, 2.x or 3.x. There are actually distinct versions of virtualenv and virtualenvwrapper that work with either python 2 or 3. If you are on python 2 install like so:  

sudo pip install virutalenv  
sudo pip install virtualenvwrapper  
If you are planning to use python 3 install the related python 3 versions  

sudo pip3 install virtualenv  
sudo pip3 install virtualenvwrapper  
You've successfully installed the packages for your python version and are all set, right? Well, try it. Type workon into your terminal. Your terminal will not be able to find the command (workon is a command of virtualenvwrapper). Of course it won't. Workon is an executable that will only be available to you once you load/source the file virtualenvwrapper.sh. But the official installation guide has you covered on this one, right?. Just open your .bash_profile and insert the following, it says in the documentation:  sudo lsof -i :6379 | grep LISTEN


export WORKON_HOME=$HOME/.virtualenvs  
export PROJECT_HOME=$HOME/Devel  
source /usr/local/bin/virtualenvwrapper.sh  
Especially the command source /usr/local/bin/virtualenvwrapper.sh seems helpful since the command seems to load/source the desired file virtualenvwrapper.sh that contains all the commands you want to work with like workon and mkvirtualenv. But yeah, no. When following the official installation guide, you are very likely to receive the error from the initial post: mkvirtualenv: command not found. Still no command is being found and you are still frustrated. So whats the problem here? The problem is that virtualenvwrapper.sh is not were you are looking for it right now. Short reminder ... you are looking here:  

source /usr/local/bin/virtualenvwrapper.sh  
But there is a pretty straight forward way to finding the desired file. Just type  

which virtualenvwrapper  
to your terminal. This will search your PATH for the file, since it is very likely to be in some folder that is included in the PATH of your system.  

If your system is very exotic, the desired file will hide outside of a PATH folder. In that case you can find the path to virtalenvwrapper.sh with the shell command find / -name virtualenvwrapper.sh  

Your result may look something like this: /Library/Frameworks/Python.framework/Versions/3.7/bin/virtualenvwrapper.sh Congratulations. You have found your missing file!. Now all you have to do is changing one command in your .bash_profile. Just change:  

source "/usr/local/bin/virtualenvwrapper.sh"  
to:  
sudo lsof -i :6379 | grep LISTEN

"/Library/Frameworks/Python.framework/Versions/3.7/bin/virtualenvwrapper.sh"  
Congratulations. Virtualenvwrapper does now work on your system. But you can do one more thing to enhance your solution. If you've found the file virtualenvwrapper.sh with the command which virtualenvwrapper.sh you know that it is inside of a folder of the PATH. So if you just write the filename, your file system will assume the file is inside of a PATH folder. So you you don't have to write out the full path. Just type:  

source "virtualenvwrapper.sh"  
Thats it. You are no longer frustrated. You have solved your problem. Hopefully.  

Change Postgres, Mongo config for all ip remote access  
sudo vim /etc/postgresql/10/main/postgresql.conf  
127.0.0.0 to 0.0.0.0  
sudo vim /etc/mongod.conf   
127.0.0.0 to 0.0.0.0  

Check port open  
netstat -vatpn | grep 27017  
total number connection  
netstat -ant | grep :5556 | awk '{print $6}' | sort | uniq -c | sort -n  

Restart Mongo service  
sudo systemctl restart mongod  

###Kill a proceess run on port  
sudo fuser -k 5001/tcp  

#DOcker remove all runnning container  
docker rm -f $(docker ps -a -q); docker rmi $(docker images -q)    
#Copy file to server (secure copy file)    
 scp * sigma@192.168.1.200:/home/sigma/SSE/  
#Copy file from server to local  
scp username@remote:/file/to/send /where/to/put  
#Send file btw 2 host  
scp username@remote_1:/file/to/send username@remote_2:/where/to/put  
#SCP copy to vagrant host  
scp -P 2222 -i /home/leo/Desktop/Demo/Vagrant/.vagrant/machines/default/virtualbox/private_key * vagrant@127.0.0.1:/home/vagrant/sse

#Count total tcp connections  
netstat -ant | grep :9002 | awk '{print $6}' | sort | uniq -c | sort -n  
#Docker remove all network  
docker network ls -q | xargs docker network rm  

#Kill process by file name  
ps aux | grep -i "node index.js" | awk {'print $2'} | xargs kill -9  
#Stop all service deploy by docker swarm  
docker swarm init --advertise-addr 192.168.99.121  
docker stack deploy --compose-file docker-compose.yml stackdemo  
docker stack rm stackdemo  
docker service ls   
docker stack ps stackdemo  

#Leave node  
docker swarm leave  
#SHow swarm join token  
docker swarm join-token worker  

#Find all process listen on port  
sudo lsof -i :6379 | grep LISTEN  
sudo kill 953   

#Stop all containers  
docker kill $(docker ps -q)  

#Clear all table on postgres  
psql -U postgres -W db  
DROP SCHEMA public CASCADE;  
CREATE SCHEMA public;  
GRANT ALL ON SCHEMA public TO postgres;  
GRANT ALL ON SCHEMA public TO public;  
COMMENT ON SCHEMA public IS 'standard public schema'; 
#View all table on db  
SELECT * FROM pg_catalog.pg_tables WHERE schemaname != 'pg_catalog' AND schemaname != 'information_schema';  

#Show docker log on swarm when container can not start  
while true; do docker logs -f $(docker ps -q -f name=es_master1); sleep 1; done  
while true; do docker logs -f $(docker ps -q -f id=adsadsadsaa); sleep 1; done  
#Rebuild and restart container  
docker-compose up -d --force-recreate --no-deps --build pub  

#Add ssl to KONG  
```
https://collectiveidea.com/blog/archives/2016/01/12/lets-encrypt-with-a-rails-app-on-heroku  
curl -i -m 60 -X POST http://localhost:8001/certificates -F "cert=$(cat cert.pem)" -F "key=$(cat privkey.pem)" -F "snis=kong.sigma-solutions.vn"
```  
Deleting all the volumes (For DB)  
docker system prune  
docker volume prune  

docker volume ls  
docker volume rm <name_of_volume>  

#Show swarm node with label  
docker node ls -q | xargs docker node inspect \   
  -f '{{ .ID }} [{{ .Description.Hostname }}]: {{ .Spec.Labels }}'   
  
 #Set label for node  
 docker node update --label-add node_name=kidssy_kong kidssy-kong  

#Ubuntu Run script when start up   
edit file /etc/rc.local  
#!/bin/bash
sudo ./startup.sh  

sudo chmod +x /etc/rc.local  
systemctl status rc-local  
sudo systemctl enable rc-local  
sudo systemctl start rc-local.service  

sudo /etc/rc.local  

#Change hostname  
sudo hostnamectl set-hostname --static abc.com  
sudo hostname abc.com  
sudo docker node update --label-add db=true 8zcy7zl4zb2hveghwoy10o4we  

#POSTGRES truncate all data in table  
TRUNCATE TABLE "user" CASCADE;  

#Update service in swarm global mode  
docker service update --force sched_busybox-global  
#List all nodes  
sudo docker node ls | grep Ready  
#Tunning mysql  
mysql –u root –p  
SET GLOBAL max_connections = 512;  
# vi /etc/my.cnf  /usr/share/mysql/my-default.cnf;   
max_connections = 512  

#Docker stop all containers  
docker stop $(docker ps -a -q)  
docker rm $(docker ps -a -q)  

#List all pip package install on docker  
apt list --installed | grep python3-pip  

#Sqlchema select in 
`from sqlalchemy.sql.expression import case

ordering = case(
    {id: index for index, id in enumerate(my_list_of_ids)},
    value=Shoe.id
 )
Shoe.query.filter(Shoe.id.in_(my_list_of_ids)).order_by(ordering).all()
`      
  
Postgres Backup    
`docker exec -i pg_container_name pg_dump --username pg_username [--password pg_password] database_name > /desired/path/on/your/machine/dump.sql`

Restore  
`docker exec -i pg_container_name psql --username pg_username [--password pg_password] database_name < /path/on/your/machine/dump.sql`  

#Hash key for facebook login from SHA1  
echo SHA1_here | xxd -r -p | openssl base64  

#Show all node with label  

`docker node ls -q | xargs docker node inspect -f '{{ .ID }} [{{ .Description.Hostname }}]: {{ .Spec.Labels }}'`  
#You can adjust that to use a range for prettier formatting instead of printing the default map:  

`docker node ls -q | xargs docker node inspect -f '{{ .ID }} [{{ .Description.Hostname }}]: {{ range $k, $v := .Spec.Labels }}{{ $k }}={{ $v }} {{end}}'`  

#Docker deamon logs  
journalctl -u docker.service  

#Grant sudo for docker  
sudo usermod -aG docker ubuntu  


#SHow full log for docker stack  
docker stack ps --no-trunc test | grep 'container failed:'  
#backup and restore postgresdb from docker  
sudo docker exec -i 2f1765f27bba pg_dump -s --username postgres  user > user_dump_s.sql  
sudo docker exec -i 0d7997de4376 psql --username postgres  user < user_dump_s.sql  
#backup with full data  
sudo docker exec -i 2f1765f27bba pg_dump  --username postgres user > user_dump_s.sql  

find and update JAVA_HOME  
`export JAVA_HOME=$(readlink -ze /usr/bin/javac | xargs -0 dirname -z | xargs -0 dirname)
`  
//Find largest folder size in ubuntu  
$sudo du -a /deploy | sort -n -r | head -n 20  
//Start psql  
psql DBNAME USERNAME  
OR  
sudo -u postgres psql  

#Docker swarm remove labels node  
docker node update --label-rm host q2pzbwgojx5v9ynxpg1cre4m1  


#Add user to ubuntu server and ssh key    
Create Home Directory + .ssh Directory  

`mkdir -p /home/mynewuser/.ssh`  
Create Authorized Keys File  

`touch /home/mynewuser/.ssh/authorized_keys`  
Create User + Set Home Directory  

`useradd -d /home/mynewuser mynewuser`  
Add User to sudo Group  

`usermod -aG sudo mynewuser`  
Set Permissions  

`chown -R mynewuser:mynewuser /home/mynewuser/`  
`chown root:root /home/mynewuser`  
`chmod 700 /home/mynewuser/.ssh`  
`chmod 644 /home/mynewuser/.ssh/authorized_keys`  
Set Password on User  

If you want to be able to log in as the user without an SSH key, setting a password will allow that, as long as PasswordAuthentication is enabled in /etc/ssh/sshd_config.  

passwd mynewuser  

#Run docker without sudo  
` sudo gpasswd -a $USER docker`  

#Enable Tab-autocomplete in ubuntu  
`sudo apt install bash-completion`  
`sudo apt install --reinstall bash-completion`  
`chsh -s /bin/bash`  

#Kong logs  
/usr/local/kong/logs  
Filename(s):  
error.log  
access.log  
admin_access.log  
# Git save credential  
git config --global credential.helper store  

