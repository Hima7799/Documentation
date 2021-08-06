# Documentation
 Day1:
 # docker2nd-Aug
**1)Docker installation.**
    a)From docks.docker.com we have downloaded docker.
    b)After downloaded we got error while installing (Docker desktop requires server services to be enabled)
              steps for error rectification
                    1)control panel->programs and features->Turn Windows features on or off-> enable  Hyper-V
                    2)in search type services.msc -> server -> automatic/Manual

**2)pull ubuntu image**
open cmd prompt
```
               docker pull ubuntu                   #pulls the image
               docker ps -l                          #list all images
               docker run -i -t ubuntu /bin/bash      #runs the ubuntu image
```
**3)setup python notebook**
a)Installing python
```
docker run -ti -d ubuntu: latest            #running ubuntu
docker ps                                   #To get images
apt-get update                              #Updating ubuntu to latest version 
apt-get install python3                     #Install python 

```
**Day2:
# 4-aug-2021
Task:
  1.pull docker postgres
  2.network ubuntu and postgres docker containers
  3.connect to postgres from notebook and create one row in a table
 **pulled docker postgres**
   1.docker pull postgres:alpine
   2.docker images
   3.docker run --name dtbse -e POSTGRES_PASSWORD=password -d -p 5432:5432 postgres:alpine
   4.docker ps
   5.docker exec -it dtbse 
   @@ got error beacause didnt mention bash
   6.docker exec -it dtbse bash
   7.pwd
   8.ls
   9.psql
   @@ doesnt connected
   10.psql --help
   @@ using this got the command
   11.psql -U postgres
   @@ final stage-- got pulled and entered into postgres
   
   **Network ubuntu and postgres
  
1.docker network ls                               
2.docker network create --driver driver_name network_name
  exact command used by mine(docker network create --driver bridge new_network) 
3.docker run -it --network=new_network ubuntu:latest /bin/bash 
4.docker run -it --network=new_network --name hima_postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres
5.docker inspect new_network
@@ got connected network


**connect to postgres from notebook
  
  1.pip install ipython-sql
  2.pip list
  3.pip install psycopg2
  @@ used this but got error
  4.pip install psycopg2-binary
  @@got installed
  5.pip install sqlalchemy
  6.%load_ext sql
  7.from sqlalchemy import create_engine
  8.%sql dialect+driver://username:password@host:port/database
  @@ got error Connection info needed in SQLAlchemy format, example:
               postgresql://username:password@hostname/dbname
               or an existing connection: dict_keys([])
               invalid literal for int() with base 10: 'port'
               Connection info needed in SQLAlchemy format, example:
               postgresql://username:password@hostname/dbname
               or an existing connection: dict_keys([])
    got errors and stucked
  **Day3:
    
    connected to postgres from jupyter :
      1.docker pull jupyter/datascience-notebook
      2.mkdir name
      3.cd name what created
      4.docker run --rm -p 8030:8888 -e JUPYTER_ENABLE_LAB=yes -v "c:/Users/chitluri.himabindhu/note":/home/acn/work jupyter/datascience-notebook
      then we connect to the jupyter through browser.
      
      creating a table in jupyter:
       before creating a table jupyter we have follow some commands--
       1. docker inspect dtbse (name of postgres file)
       2.docker exec -it dtbse bash
       3.su postgres
       4.\conninfo
       
       after that we can create a table by following:
       1.pip install psycopg2-binary
       2.import psycopg2  
       3.import pprint
       4.conn = psycopg2.connect("postgres://postgres:password@172.17.0.3:5432/postgres")
       5.!pip install ipython-sql
       6.%load_ext sql
       7.%sql sqlite://
       8.%%sql
          CREATE TABLE EMP(firstname varchar(50),lastname varchar(50));
          INSERT INTO EMP VALUES('tom','Mitchell');
          INSERT INTO EMP VALUES('jack','ryan');
       9.%sql SELECT * from EMP;
 
 
**Refrences:

**Day1:Docker:

  installation-> https://docs.docker.com/docker-for-windows/install/
  errors-> https://www.bing.com/videos/search?view=detail&mid=C12FF81E191DA0CA296AC12FF81E191DA0CA296A&q=docker
  ubuntu -> http://www.servermom.org/pull-docker-images-run-docker-containers/3225/#:~:text=To%20pull%20an%20image%2C%20use%20%E2%80%9C%20docker%20pull,your%20server%2C%20use%20%E2%80%9C%20docker%20images%20%E2%80%9D%20command
  python -> https://www.datasciencelearner.com/install-and-run-python-in-docker-container
  **Day2:
 1. https://gregtczap.com/blog/docker-postgres-ubuntu-localhost/
 2. https://www.geeksforgeeks.org/creating-a-network-in-docker-and-connecting-a-container-to-that-network/#:~:text=%20%20%201%20Step%201%3A%20.%20In,next%20step%20connect%20the%20container%20to...%20More%20
 3. https://www.bing.com/videos/search?view=detail&mid=56AFAB710730DE000BDE56AFAB710730DE000BDE&q=connect
 4. https://www.youtube.com/watch?v=aHbE3pTyG-Q
 5. https://www.youtube.com/watch?v=2PDkXviEMD0
 6. https://shravan-kuchkula.github.io/sql/postgres-jupyter/#step-1-first-make-sure-you-install-homebrew-if-you-dont-already-have-it
 7. https://medium.com/analytics-vidhya/postgresql-integration-with-jupyter-notebook-deb97579a38d
 8. https://blog.panoply.io/connecting-jupyter-notebook-with-postgresql-for-python-data-analysis#:~:text=%20Connecting%20Python%20pandas%20and%20Jupyter%20Notebooks%20to,following%20along%20in%20your%20own%20Jupyter...%20More%20
 **Day3-
   https://www.datacamp.com/community/tutorials/sql-interface-within-jupyterlab
   (https://medium.com/analytics-vidhya/postgresql-integration-with-jupyter-notebook-deb97579a38d
