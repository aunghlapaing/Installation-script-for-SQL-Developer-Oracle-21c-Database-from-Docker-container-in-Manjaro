# Installation-script-for-SQL-Developer-Oracle-21c-Database-from-Docker-container-in-Manjaro
Installation guide for SQL Developer + Oracle 21c Database from Docker container in manjaro arch linux based

#Script for Download sql developer in manjaro arch linux based
- ------------------------------------------- - 
1. download zip for the sql developer
2. unzip
3. move to opt directory
4. run sqldeveloper.sh in the sqldeveloper

echo 'export PATH=$PATH:opt/sqldeveloper/sqldeveloper.sh/bin' | sudo tee -a ~/.bashrc


#Script for installation Oracle 21c from Docker container
Docker 
- --------------------------- - 
1. Make sure your system have already install docker
   docker -v
2. check docker status 
   sudo systemctl status docker
3. activate docker
   sudo system start docker
4. Enable docker
   sudo systemctl enable docker
   
   
#oracle database 21c docker image installation from docker
- --------------------------------------- - 
1. Go to oracle Container Registry.com to read the documentations
   https://container-registry.oracle.com/ords/f?p=113:10::::::
2. login into Oracle Container Registry
   docker login container-registry.oracle.com
   username:
   password:
3. pull docker image 
   docker pull container-registry-oracle.com/database/enterprise:latest (desire version)
4. check the pulled docker images
   sudo docker images
5. custom config
   docker run -d --name <container_name> \
    -p <host_port>:1521 -p <host_port>:5500 \
    -e ORACLE_SID=<your_SID> \
    -e ORACLE_PDB=<your_PDBname> \
    -e ORACLE_PWD=<your_database_password> \
    -e INIT_SGA_SIZE=<your_database_SGA_memory_MB> \
    -e INIT_PGA_SIZE=<your_database_PGA_memory_MB> \
    -e ORACLE_EDITION=<your_database_edition> \
    -e ORACLE_CHARACTERSET=<your_character_set> \
    -e ENABLE_ARCHIVELOG=true \
    -v [<host_mount_point>:]/opt/oracle/oradata \
    container-registry.oracle.com/database/enterprise:21.3.0.0
    
docker run -d --name oracle21c -p 1521:1521 -p 5500:5500 -e ORACLE_PWD=oracle21c container-registry.oracle.com/database/free

6. Preparation stage - > check preparation %
   docker logs <dbname>
7. check docker ps
   sudo docker ps -- 
   show -- > database docker file status 
   
8. Check the docker listener
docker exec -it oracle21c bash

9. Check the docke port is running
lsnrctl status

   
#Connect to SQL developer (Outside from container)
- ---------------------------------- -
1. Create new connection
2. give a database name
3. username as SYS or SYSTEM
4. password as your password <oracle21c>
5. Authentication type as Default
6. Role as SYSDBA
7. Connection type as basic
8. Hostname as localhost
9. Port as 1521 <port of docker container>
10. SID as FREE
11. Service name as FREE
   
   
#Connect to database server within container (can see the documentation on container-registry.com)
- ------------------------- - 
   1. check port first 
      docker port <dbname>
   2. connect to the database
      docker exec -it <oracle-db> sqlplus sys/<your_password>@FREE as sysdba

#Connect to database sever outside container
- -------------------------- - 
  1. check database status
     sudo docker ps
  2. contect to database from sql developer
     sudo docker exec -it oracle-db bash
     cd /opt/oracle
  
     docker exec -it oracle-db sqlplus sys/<your password>@//localhost:<port mapped to 1521>/FREE as sysdba
     docker exec -it oracle-db sqlplus sys/oracle123@//localhost:1521/FREE as sysdba
     
#If docker container stop docker image
- ----------------------------- - 
1. sudo docker ps (check currently running docker images)
 if (not expected one is not running){
 	sudo docker start <expected name>
 }
 
 2. want to stop (sudo docker stop <expected one>)
 
2. If (want to remove current docker container){
	sudo docker rm <expected name>
}

3. Want to rename (sudo docker rename oracle21c oracle21c_old)

