#centos安装docker

## Docker的版本
Docker分为社区版（CE）和企业版（EE）两个版本，社区版本可以免费使用，而企业版则需要付费使用，对于我们个人开发者或小企业来说，一般是使用社区版的。

Docker CE有三个更新频道，分别为stable、test、nightly，stable是稳定版本，test是测试后的预发布版本，而nightly则是开发中准备在下一个版本正式发布的版本，我们可以根据自己的需求下载安装。

## 删除旧的Docker版本

可能有些Linux预先安装Docker，但一般版本比较旧，所以可以先执行以下代码来删除旧版本的Docker。
```
$ sudo yum remove docker \
              docker-client \
              docker-client-latest \
              docker-common \
              docker-latest \
              docker-latest-logrotate \
              docker-logrotate \
              docker-selinux \
              docker-engine-selinux \
              docker-engine
```

## 指定安装版本
```
$ sudo yum-config-manager \
--add-repo \
https://download.docker.com/linux/centos/docker-ce.repo
```

## 使用yum安装Docker
```
$ sudo yum install docker-ce
```

## Docker：docker国内镜像加速
vim /etc/docker/daemon.json
```
{
  "registry-mirrors": [
    "https://registry.docker-cn.com",
    "http://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn"
  ]
}
```

启动Docker服务器
# 启动docker守护进程
$ sudo systemctl start docker

测试安装是否成功
通过上面几种方式安装了Docker之后，我们可以通过下面的方法来检测安装是否成功。

打印Docker版本
# 打印docker版本
$ docker version 

拉取镜像并运行容器
# 拉取hello-world镜像
docker pull hello-world

# 使用hello-world运行一个容器
docker run hello-world

运行上面的命令之后，如果有如下图所示的输出结果，则说明安装已经成功了。
```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## Docker的基本概念
镜像（Image）、容器（Container）与仓库（Repository），这三个是Docker中最基本也是最核心的概念，对这三个概念的掌握与理解，是学习Docker的关键。
镜像（Image）
什么是Docker的镜像？

Docker本质上是一个运行在Linux操作系统上的应用，而Linux操作系统分为内核和用户空间，无论是CentOS还是Ubuntu，都是在启动内核之后，通过挂载Root文件系统来提供用户空间的，而Docker镜像就是一个Root文件系统。

Docker镜像是一个特殊的文件系统，提供容器运行时所需的程序、库、资源、配置等文件，另外还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。

镜像是一个静态的概念，不包含任何动态数据，其内容在构建之后也不会被改变。

### 下面的命令是一些对镜像的基本操作，如下：
查看镜像列表

# 列出所有镜像
```
docker image ls
```

由于我们前面已经拉取了hello-world镜像，所以会输出下面的内容：
```
REPOSITORY                                      TAG                 IMAGE ID            CREATED             SIZE
hello-world                                     latest              fce289e99eb9        7 months ago        1.84kB
```

下面的命令也一样可以查看本地的镜像列表，而且写法更简洁。
# 列表所有镜像
```
docker images
```

从仓库拉取镜像

前面我们已经演示过使用docker pull命令拉取了hello-world镜像了，当然使用docker image pull命令也是一样的。

一般默认是从Docker Hub上拉取镜像的，Docker Hub是Docker官方提供的镜像仓库服务（Docker Registry），有大量官方或第三方镜像供我们使用，比如我们可以在命令行中输入下面的命令直接拉取一个CentOS镜像：
```
docker pull centos
```

docker pull命令的完整写法如下：
```
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
```

拉取一个镜像，需要指定Docker Registry的地址和端口号，默认是Docker Hub，还需要指定仓库名和标签，仓库名和标签唯一确定一个镜像，而标签是可能省略，如果省略，则默认使用latest作为标签名，另外，仓库名则由作者名和软件名组成。

那么，我们上面使用CentOS，那是因为省略作者名，则作者名library，表示Docker官方的镜像，所以上面的命令等同于：
```
docker pull library/centos:latest
```

因此，如果拉取非官方的第三方镜像，则需要指定完整仓库名，如下：
```
docker pull mysql/mysql-server:latest
```
运行镜像

使用docker run命令，可以通过镜像创建一个容器，如下：
```
docker run -it centos /bin/bash
```

删除镜像

当本地有些镜像我们不需要时，那我们也可以删除该镜像，以节省存储空间，不过要注意，如果有使用该镜像创建的容器未删除，则不允许删除镜像。
# image_name表示镜像名，image_id表示镜像id
```
dockere image rm image_name/image_id
```

删除镜像的快捷命令：
```
docker rmi image_name/image_id
```


容器（Container）
Docker的镜像是用于生成容器的模板，镜像分层的，镜像与容器的关系，就是面向对象编程中类与对象的关系，我们定好每一个类，然后使用类创建对象，对应到Docker的使用上，则是构建好每一个镜像，然后使用镜像创建我们需要的容器。

启动和停止容器

启动容器有两种方式，一种是我们前面已经介绍过的，使用docker run命令通过镜像创建一个全新的容器，如下：
```
docker run hello-world
```

另外一种启动容器的方式就是启动一个已经停止运行的容器：
# container_id表示容器的id
```
docker start container_id
```

要停止正在运行的容器可以使用docker container stop或docker stop命令，如下：
# container_id表示容器的id
```
docker stop container_id
```

查看所有容器

如果要查看本地所有的容器，可以使用docker container ls命令：
# 查看所有容器
```
docker container ls
```

查看所有容器也有简洁的写法，如下：
# 查看所有容器
```
docker ps
```

删除容器

我们也可以使用docker container rm命令，或简洁的写法docker rm命令来删除容器，不过不允许删除正在运行的容器，因此如果要删除的话，就必须先停止容器。
# container_id表示容器id，通过docker ps可以看到容器id
```
$ docker rm container_id
```

当我们需要批量删除所有容器，可以用下面的命令：
# 删除所有容器
```
docker rm $(docker ps -q)
```

# 删除所有退出的容器
```
docker container prune
```

进入容器

# 进入容器，container_id表示容器的id，command表示Linux命令，如/bin/bash
```
docker exec -it container_id command
```

仓库（Repository）
在前面的例子中，我们使用两种方式构建镜像，构建完成之后，可以在本地运行镜像，生成容器，但如果在更多的服务器运行镜像呢？很明显，这时候我们需要一个可以让我们集中存储和分发镜像的服务，就像Github可以让我们自己存储和分发代码一样。

Docker Hub就是Docker提供用于存储和分布镜像的官方Docker Registry，也是默认的Registry，其网址为https://hub.docker.com，前面我们使用docker pull命令便从Docker Hub上拉取镜像。

Docker Hub有很多官方或其他开发提供的高质量镜像供我们使用，当然，如果要将我们自己构建的镜像上传到Docker Hub上，我们需要在Docker Hub上注册一个账号，然后把自己在本地构建的镜像发送到Docker Hub的仓库当中，Docker Registry包含很多个仓库，每个仓库对应多个标签，不同标签对应一个软件的不同版本。
Docker的组成与架构
在安装好并启动了Docker之后，我们可以使用在命令行中使用Docker命令操作Docker，比如我们使用如下命令打印Docker的版本信息。
```
docker verion
```


## docker pull默认下载的镜像位置
/var/lib/docker/containers/

```
查询并删除所有容器

docker stop $(docker ps -q) & docker rm $(docker ps -aq)

查询并删除所有images

docker rmi `docker images -q`或者docker rmi $(docker images -q) 

可能有删不干净的，加上 -f : 

docker rmi $(docker images -q) -f
```

## docker-compose 安装使用
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose


-d 是后台运行，不加 -d 参数是前台运行，ctrl+c可以终止
docker-compose up -d

重新创建
docker-compose up --build

查看容器运行情况
docker-compose ps

停止并删除容器并删除volume
docker-compose down --volumes

停止容器
docker-compose stop

停止并删除容器
docker-compose down

启动容器
docker-compose start

进入mynginx容器内部
docker-compose exec nginx bash

重启单个容器
docker-compose restart nginx

您可以设置杀死容器之前等待停止的时间（以秒为单位）
docker-compose restart -t 30 nginx
```


$ docker exec -i some-mysql sh -c 'exec mysql -uroot -p"$MYSQL_ROOT_PASSWORD"' < /some/path/on/your/host/all-databases.sql