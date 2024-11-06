## 单独构建 powerjob-server

```shell
#copy jar 包
cp -rf ../powerjob-server-starter/target/*.jar ./powerjob-server.jar

# 删除 旧镜像

sudo docker rmi -f powerjob-server:v1

# 构建 镜像
sudo docker build -t powerjob-server:v1 .

#启动容器
sudo docker run -d \
       --network=host \
       --name powerjob-server \
       -p 7700:7700 -p 10086:10086 -p 5001:5005 -p 10001:10000 \
       -e JVMOPTIONS="-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.port=10000 -Dcom.sun.management.jmxremote.rmi.port=10000 -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false" \
       -e PARAMS="--oms.swagger.enable=true --spring.profiles.active=daily --spring.datasource.core.jdbc-url=jdbc:mysql://192.168.2.27:3306/powerjob-daily?useUnicode=true&characterEncoding=UTF-8 --oms.mongodb.enable=false"  \
       -v /home/yinyunlong/powerjob/server:/root/powerjob/server -v ~/.m2:/root/.m2 \
       powerjob-server:v1



sudo docker run -it powerjob-server:v1 /bin/bash

#访问网站
http://127.0.0.1:7700/#/oms/home
ADMIN
powerjob_admin

```