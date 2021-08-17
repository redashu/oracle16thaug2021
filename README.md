# Namespaces in Docker & containers

<img src="ns.png">

## Intro to Cgroups 

<img src="cg.png">

## 

```
172  docker  run -tid --name ashuc1  oraclelinux:8.4  ping fb.com 
  173  docker  ps
  174  docker  stats
  175  history 
  176  docker  exec  -itd  ashuc1  ping google.com 
  177  docker  stats
  178  docker  top  ashuc1
  
```

## LImit Ram 

```
 docker  run -itd --name ashuc2  --memory 100m alpine ping fb.com 
 
```

### limit ram and cpu 

```
docker  run -itd --name ashuc3 --cpu-shares=20   --memory 100m alpine ping fb.com
```

### restart policy 

[restart policy](https://docs.docker.com/config/containers/start-containers-automatically/)


## putting restart policy 

```
docker  run -itd --name myname --restart always alpine ping localhost

```

### building java based image 

```
[ashu@ip-172-31-79-145 javacode]$ docker  build  -t  ashujava:v1  . 
Sending build context to Docker daemon  3.072kB
Step 1/7 : FROM openjdk
 ---> f4489eef8885
Step 2/7 : LABEL name=ashutoshh
 ---> Running in 68177992bdd9
Removing intermediate container 68177992bdd9
 ---> ab3054b31d0a
Step 3/7 : RUN mkdir /jcode
 ---> Running in 407a6bba13e9
Removing intermediate container 407a6bba13e9
 ---> 0724490b4758
Step 4/7 : ADD hello.java  /jcode/hello.java
 ---> 886a776e77dd
Step 5/7 : WORKDIR  /jcode
 ---> Running in 8c34640a6bd2
Removing intermediate container 8c34640a6bd2
 ---> 55ad684ca817
Step 6/7 : RUN javac  hello.java
 ---> Running in 84eaa6d18d14
Removing intermediate container 84eaa6d18d14
 ---> af815d67d928
Step 7/7 : CMD ["java","myclass"]
 ---> Running in 801832c7da62
Removing intermediate container 801832c7da62
 ---> b74137e6f206
Successfully built b74137e6f206
Successfully tagged ashujava:v1

```

### creation container 

```
 docker  run -itd --name jvashuc1  ashujava:v1  
  208  docker  ps
  209  docker  logs -f  jvashuc1
  210  history 
  211  docker  stats
  
```

### checking java container details 

```
[ashu@ip-172-31-79-145 myimages]$ docker  exec -it  jvashuc1  bash 
bash-4.4# java -version 
openjdk version "16.0.2" 2021-07-20
OpenJDK Runtime Environment (build 16.0.2+7-67)
OpenJDK 64-Bit Server VM (build 16.0.2+7-67, mixed mode, sharing)
bash-4.4# 
bash-4.4# 
bash-4.4# cat  /etc/os-release 
NAME="Oracle Linux Server"
VERSION="8.4"
ID="ol"
ID_LIKE="fedora"
VARIANT="Server"
VARIANT_ID="server"
VERSION_ID="8.4"
PLATFORM_ID="platform:el8"
PRETTY_NAME="Oracle Linux Server 8.4"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:oracle:linux:8:4:server"
HOME_URL="https://linux.oracle.com/"
BUG_REPORT_URL="https://bugzilla.oracle.com/"

ORACLE_BUGZILLA_PRODUCT="Oracle Linux 8"
ORACLE_BUGZILLA_PRODUCT_VERSION=8.4
ORACLE_SUPPORT_PRODUCT="Oracle Linux"
ORACLE_SUPPORT_PRODUCT_VERSION=8.4

```

### building image 

```
ashu@ip-172-31-79-145 myimages]$ cd  javacode/
[ashu@ip-172-31-79-145 javacode]$ ls
Dockerfile  hello.java  jdk8.dockerfile
[ashu@ip-172-31-79-145 javacode]$ docker  build -t  ashujava:v2  -f  jdk8.dockerfile  . 
Sending build context to Docker daemon  4.096kB
Step 1/8 : FROM oraclelinux:8.4
 ---> fcf3cbfc22ac
Step 2/8 : LABEL name=ashutoshh
 ---> Using cache
 ---> 6ac21e84f537
Step 3/8 : RUN  dnf install java-1.8.0-openjdk.x86_64 java-1.8.0-openjdk-devel.x86_64 -y
 ---> Running in bfb52c66f712
```

### webapp containerization 

<img src="webs.png">

### DNFs installing httpd 

```
[ashu@ip-172-31-79-145 beginner-html-site-styled]$ ls
CODE_OF_CONDUCT.md  Dockerfile  images  index.html  LICENSE  README.md  styles
[ashu@ip-172-31-79-145 beginner-html-site-styled]$ docker build -t  ashuhttpd:aug17th2021 . 
Sending build context to Docker daemon  63.49kB
Step 1/5 : FROM oraclelinux:8.4
 ---> fcf3cbfc22ac
Step 2/5 : LABEL name=ashutoshh
 ---> Using cache
 ---> 6ac21e84f537
Step 3/5 : RUN dnf install httpd -y
 ---> Running in fd2b9ca4f3b8

```
### creating 

```
 docker  run -itd  --name ashuwebc1  -p  1234:80   ashuhttpd:aug17th2021
```

### docker recap 

<img src="dcap.png">

## image sharing 

<img src="imgshare.png">

## Docker public registry 

<img src="reg.png">

## docker image name 

<img src="imgname.png">

## docker hub image push 

```
[ashu@ip-172-31-79-145 myimages]$ docker  tag   ashuhttpd:aug17th2021  dockerashu/ashuhttpd:aug17th2021 
[ashu@ip-172-31-79-145 myimages]$ 
[ashu@ip-172-31-79-145 myimages]$ 
[ashu@ip-172-31-79-145 myimages]$ docker  login -u  dockerashu
Password: 
WARNING! Your password will be stored unencrypted in /home/ashu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[ashu@ip-172-31-79-145 myimages]$ docker  push  dockerashu/ashuhttpd:aug17th2021 
The push refers to repository [docker.io/dockerashu/ashuhttpd]
0d7372999182: Pushed 
3cf5b4d5d8b0: Pushed 
89ca13798c53: Mounted from library/oraclelinux 
aug17th2021: digest: sha256:9a1ac822cd2c7a36e64c4b66260ce7c74883f60cb45013019d88dbd92d91bce0 size: 951
[ashu@ip-172-31-79-145 myimages]$ docker  logout 
Removing login credentials for https://index.docker.io/v1/


```


### from a different docker engine 

```
❯ docker  pull dockerashu/ashuhttpd:aug17th2021
aug17th2021: Pulling from dockerashu/ashuhttpd
2d6c3304745e: Already exists 
14f175d153cf: Pull complete 
c75d42f4581e: Pull complete 
Digest: sha256:9a1ac822cd2c7a36e64c4b66260ce7c74883f60cb45013019d88dbd92d91bce0
Status: Downloaded newer image for dockerashu/ashuhttpd:aug17th2021
docker.io/dockerashu/ashuhttpd:aug17th2021
❯ docker  images
REPOSITORY             TAG           IMAGE ID       CREATED        SIZE
dockerashu/ashuhttpd   aug17th2021   ceb1faf02f8a   2 hours ago    394MB
openjdk                latest        f4489eef8885   4 days ago     467MB
oraclelinux            8.4           fcf3cbfc22ac   4 days ago     247MB
alpine                 latest        021b3423115f   10 days ago    5.6MB
busybox                latest        69593048aa3a   2 months ago   1.24MB


```

## cloud based registry 

<img src="cl.png">

##  DB in container format 

```
[ashu@ip-172-31-79-145 myimages]$ docker  run  -itd --name ashudb  -e  MYSQL_ROOT_PASSWORD=AshuDb088  mysql 
Unable to find image 'mysql:latest' locally
latest: Pulling from library/mysql
33847f680f63: Pull complete 
5cb67864e624: Pull complete 
1a2b594783f5: Pull complete 
b30e406dd925: Pull complete 
48901e306e4c: Pull complete 
603d2b7147fd: Pull complete 
802aa684c1c4: Pull complete 
715d3c143a06: Pull complete 
6978e1b7a511: Pull complete 
f0d78b0ac1be: Pull complete 
35a94d251ed1: Pull complete 
36f75719b1a9: Pull complete 
Digest: sha256:8b928a5117cf5c2238c7a09cd28c2e801ac98f91c3f8203a8938ae51f14700fd
Status: Downloaded newer image for mysql:latest
a82d838cfd773930267021a0e30304ccb3eeb305fbd4d8100344adfb0c039006

```

### checking logs for db container readiness

```
[ashu@ip-172-31-79-145 myimages]$ docker  logs  -f  ashudb
2021-08-17 09:46:23+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.26-1debian10 started.
2021-08-17 09:46:23+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2021-08-17 09:46:23+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.26-1debian10 started.
2021-08-17 09:46:23+00:00 [Note] [Entrypoint]: Initializing database files
2021-08-17T09:46:23.974214Z 0 [Warning] [MY-010139] [Server] Changed limits: max_open_files: 1024 (requested 8161)
2021-08-17T09:46:23.974220Z 0 [Warning] [MY-010142] [Server] Changed limits: table_open_cache: 431 (requested 4000)
2021-08-17T09:46:23.974712Z 0 [System] [MY-013169] [Server] /us

```

## do docker exec then login to db server 

```
root@a82d838cfd77:/# mysql  -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.26 MySQL Community Server - GPL

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 

```

### doing mysql query 

```
root@a82d838cfd77:/# mysql  -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.26 MySQL Community Server - GPL

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)

mysql> create  database  oracletraining;
Query OK, 1 row affected (0.01 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| oracletraining     |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

```

## MSSQL server 


<img src="mssql.png">

### create 2019 db container 

```
docker  run -itd --name ashumssql  -e  "ACCEPT_EULA=Y"  -e  "MSSQL_SA_PASSWORD=Mcr099123@2"  --restart always  mcr.microsoft.com/mssql/server:2019-latest

```

### checking base os of container 

```
ashu@ip-172-31-79-145 myimages]$ docker  exec -it  ashumssql bash 
fmssql@9f7f19c18af0:/$ 
mssql@9f7f19c18af0:/$ cat  /etc/os-release 
NAME="Ubuntu"
VERSION="20.04.2 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.2 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal

```

### connecting to mssql db 

```
[ashu@ip-172-31-79-145 myimages]$ docker  exec -it  ashumssql bash 
mssql@9f7f19c18af0:/$ 
mssql@9f7f19c18af0:/$ /opt/mssql-tools/bin/sqlcmd  -S localhost  -U SA -P 
Sqlcmd: '-P': Missing argument. Enter '-?' for help.
mssql@9f7f19c18af0:/$ /opt/mssql-tools/bin/sqlcmd  -S localhost  -U SA -P "Mcr099123@2"
1> 
2> 
3> create database testDB;
4> show databases;
5> 

```

