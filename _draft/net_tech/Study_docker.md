# 1.第一条命令
docker run ubuntu:15.10 /bin/echo "Hello world"
# 2.第二条命令
docker run -it ubuntu:15.10 /bin/bash
# 3.第三条命令
docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"

解释：
-i:标准输入流
-d：daemon后台运行
-t：terminal终端模式
run：运行镜像，run之后他就叫容器，类似对象与实例
ubuntu:15.10:镜像名称。：后为tag
/bin/bash ： 运行镜像后输入的命令
# 4.第四条命令
docker ps

docker logs xxx

docker stop xxx
# 5.第五条命令
docker commit -m="has update" -a="runoob" e218edb10161 runoob/ubuntu:v2

解释：
-m:message
-a:author
e218edb10161:镜像名称
runoob/ubuntu:v2:镜像名:标签

# 容器之间共享网络
docker network create netname
docker run --net netname img