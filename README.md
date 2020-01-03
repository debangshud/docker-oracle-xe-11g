docker-oracle-xe-11g
============================

Oracle Express Edition 11g Release 2 on Ubuntu 18.04 LTS

## Installation(Local)
```
git clone https://github.com/wnameless/docker-oracle-xe-11g.git
cd docker-oracle-xe-11g
docker build -t wnameless/oracle-xe-11g .
```

## Installation(DockerHub)
```
docker pull wnameless/oracle-xe-11g-r2
```
SSH server has been removed since 18.04, please use "docker exec"

## Quick Start

Run with 1521 port opened:
```
docker run -d -p 49161:1521 wnameless/oracle-xe-11g-r2
```

Run this, if you want the database to be connected remotely:
```
docker run -d -p 49161:1521 -e ORACLE_ALLOW_REMOTE=true wnameless/oracle-xe-11g-r2
```

For performance concern, you may want to disable the disk asynch IO:
```
docker run -d -p 49161:1521 -e ORACLE_DISABLE_ASYNCH_IO=true wnameless/oracle-xe-11g-r2
```

Enable XDB user with default password: xdb, run this:
```
docker run -d -p 49161:1521 -e ORACLE_ENABLE_XDB=true wnameless/oracle-xe-11g-r2
```

For APEX user:
```
docker run -d -p 49161:1521 -p 8080:8080 wnameless/oracle-xe-11g-r2
```

For persisted volume, create a directory /opt/data/oracle/11g-r2 and change permission to R/W:
```
docker run -d \
-p 49160:22 \
-p 49161:1521 \
-p 18080:8080 \
-e ORACLE_ALLOW_REMOTE=true \
-e ORACLE_DISABLE_ASYNCH_IO=true \
-e ORACLE_ENABLE_XDB=true \
--mount source=oracle_vol,target=/opt/data/oracle/11g-r2 \
wnameless/oracle-xe-11g-r2
```



```
# Login http://localhost:8080/apex/apex_admin with following credential:
username: ADMIN
password: admin

Password changed to Barclays1$
```

By default, the password verification is disable(password never expired)<br/>
Connect database with following setting:
```
hostname: localhost
port: 49161
sid: xe
username: system
password: oracle
```

Password for SYS & SYSTEM
```
oracle
```

Support custom DB Initialization and running shell scripts
```
# Dockerfile
FROM wnameless/oracle-xe-11g-r2

ADD init.sql /docker-entrypoint-initdb.d/
ADD script.sh /docker-entrypoint-initdb.d/
```
Running order is alphabetically. 

Create a schema named smith:
```
CREATE USER smith IDENTIFIED BY password;
GRANT CONNECT TO smith;
GRANT CONNECT, RESOURCE, DBA TO smith;
GRANT CREATE SESSION TO smith;
GRANT UNLIMITED TABLESPACE TO smith;
```

