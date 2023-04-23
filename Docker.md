# Docker

Docker是轻量级的虚拟机环境，只会虚拟出少量的硬件设备：网卡，其余资源使用本地linux的资源（不虚拟CPU，内存等），虚拟出的环境叫做容器。不同容器是相互隔离的，不会出现资源冲突。可以给每一个容器分配固定的硬件资源，防止某些程序占用太多服务器资源（mongodb）。docker并不能安装操作系统，只能安装应用程序，基于主机的linux内核。

docker为了快速打包和部署软件环境，引入了镜像机制。镜像是容器的基础，所有容器都是运行在镜像上，镜像对于容器是只读权限，不能被容器修改，保证不会因为某一个容器修改了镜像导致其他所有容器运行失败。

## 1. 操作镜像

- 下载镜像

  ```shell
  docker pull python:3.10
  ```

- 查看安装的镜像列表

  ```shell
  docker images
  ```

- 查看选定镜像的详细信息

  ```shell
  docker inspect python:3.10
  ```

- 删除镜像

  ```shell
  docker rmi python:3.10
  ```

- 保存镜像到本地

  ```shell
  docker save python:3.10 > {Path}
  ```

- 从本地文件加载镜像

  ```shell
  docker load < {Path}
  ```

## 2. 操作容器

- 创建容器并进入

  ```shell
  # -it: i以交互模式运行容器，t为容器重新分配一个伪输入终端
  docker run -it --name p1 python:3.10 bash
  ```

- 退出容器（如果是通过run创建的容器则停止容器，如果是start运行后再通过exec进入的容器则不停止）

  ```bash
  exit
  ```

- 查看当前正在工作的容器（-a显示所有容器包括停止运行的）

  ```shell
  docker ps
  ```

- 启动容器

  ```shell
  docker start p1
  ```

- 暂停/恢复容器

  ```shell
  docker pause p1
  docker unpause p1
  ```

- 进入正在运行的容器

  ```shell
  docker exec -it p1 bash
  ```

- 查看某个容器的详细信息

  ```shell
  docker inspect p1
  ```

- 停止容器

  ```shell
  docker stop p1
  ```

- 删除容器（一定要先停止容器）

  ```shell
  docker rm p1
  ```


## 3. Docker网络环境

默认情况下，Docker环境会给容器分配动态IP地址，导致下一次启动容器时，IP地址发生改变。

可以单独创建一个Docker内部的网段。

```shell
# 创建内部网段
docker network create --subnet=172.18.0.0/16 mynet
# 删除内部网段
docker network rm mynet
# 创建容器绑定网段（不能使用172.18.0.1，因为这是默认网关地址）
docker run -it --net mynet --ip 172.18.0.2 python:3.10 bash
```

端口映射：可以把容器端口映射到宿主机端口，这样其他主机就能访问容器。

```shell
# 映射一个端口，9500为宿主机端口，5000位容器端口
docker run -it -p 9500:5000 python:3.10 bash
# 映射两个端口
docker run -it -p 9500:5000 -p 9600:3306 python:3.10 bash
```

目录挂载技术：为了能把一部分业务数据保存在Docker环境之外，或者把宿主机的文件传入容器，所以需要给容器挂载宿主机目录。也可以使用ftp，但是如果无法启动容器，那么容器中的重要文件就无法提取出来。Docker环境支持挂载多个目录。

```shell
docker run -it -v /root/test:/root/project python:3.10 bash
```

## 4. 创建项目需要的docker容器

### python容器

```shell
# -d:创建容器直接后台运行，进入容器使用exec，退出容器也不会停止
docker run -it -d --name p1 -p 9500:5000 -v ~/dockerDoc:/root/project --net mynet --ip 172.18.0.2 python:3.10 bash
```

### mysql容器

- 下载mysql镜像

  ```shell
  # 默认下载最新版本latest
  docker pull mysql
  # 下载指定版本
  docker pull mysql:8.0.18
  ```

- 创建mysql容器，做好端口映射和目录挂载

  ```shell
  # 不需要-it因为不需要交互终端 -e指定参数：root密码
  docker run --name m1 -p 4306:3306 --net mynet --ip 172.18.0.3 -v ~/dockerSQL:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=asd12311 -d mysql:latest
  ```

## 5. 上传flask项目到容器

- 运行python程序得到字节码文件（pyc），删除多余的文件。压缩为tar。

  ```shell
  # 		  压缩后的压缩文件路径		   压缩前的文件夹路径
  tar -cvf ~/dockerDoc/project1.tar /project1
  ```

- 将文件上传至宿主机

- 在宿主机上解压改文件至容器挂载的目录

  ```shell
  tar -xvf project1.tar -C /root/project
  ```

- 进入容器，运行字节码文件

  ```shell
  docker exec -it p1 bash
  ```

  ```bash
  cd /root/project
  python app.pyc
  ```

- 上述方法不能退出运行，如果想要后台运行，并且把运行日志输入到文件

  ```python
  nohup python app.pyc > logs.txt
  ```

  之后，直接关闭终端窗口，该进程会在容器中保持后台运行

