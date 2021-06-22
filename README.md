### Docker

## 1 Docker Overview

Docker是一个用来开发，装运和运行应用的开放平台，Docker让你可以让你的应用脱离你现有的基础环境，因此你可以快速的交付你的软件。有了Docker，你可以使用管理你应用的方式来管理你的基础环境。一旦掌握了Docker关于装运，测试和快速代码部署的这些技术优点，你可以大大的减少代码编写与生产部署运行的时间延迟。

### 1.1 The Docker Platform

Docker提供了打包和运行应用程序的能力，应用程序会运行在一个相对松散且独立的环境中，这个环境我们称之为容器，这种独立性和安全性，可以让你在给定的host主机中同时运行很多容器。这个容器是轻量级的并且包含了运行指定引用的所有需要的依赖和环境，所以你不用依赖当前host主机上安装的环境。在工作中，你可以很轻松的分享你的容器，并且能确保，以同样的方式获得到的你分享的容器是一模一样的。

Docker提供工具和平台来管理你容器的生命周期，包括：

- 使用容器来开发你的应用和应用的支撑组件
- 容器将成为你分散部署和测试程序的基本单元
- 当你准备好部署你的应用到生产环境，作为一个容器或者是一个精心准备的服务。不管你的生产环境是一个本地的数据中心，还是一个云提供商，或者两者的混合，Docker的使用都是一样有效的

### 1.2 What can i use Docker for?

**快速，持续交付你的应用程序**

Docker在容器中提供给你应用程序所需的一切服务，并且通过管理容器，让开发人员你的生产变得标准化，流水线化，Docker指定了开发及部署的环境规范，控制着开发的生命周期，Docker非常善于持续化的集成和持续化的交付(CI/CD)工作流。

想想以下场景：

- 你手下的程序猿在本地写代码，用Docker容器进行他们作品的分享
- 他们使用Docker将他们的应用部署到测试环境进行自动化或者手动的测试
- 当开发者发现bugs，他们可以在开发环境中修复，并且重新将应用部署到测试环境来测试和验证
- 当测试完成，进行生产环境的部署简单到只要更新image到生产环境那么简单

**响应式部署和伸缩**

Docker基于容器的平台实现了高轻便的工作负载，Docker容器可以运行在开发者的本地笔记本上，在数据中心的物理或者虚拟机上，在云服务提供商上，或者是一个混合的环境中

Docker的可移植性和轻量级的性质让他可以很容易的去动态的管理工作负载，可以根据业务的需求来对应用和服务的规模进行增加或者是缩小，这种伸缩几乎是实时的。

**在相同的硬件中运行更多的工作负载(workloads)**

Docker是轻量级的并且响应速度很快，他提供了可以独立生存的，成本效益优异的选择，用以替代hypervisor-based技术(VirtualBox、VMware)的虚拟机技术，所以你可以用Docker技术提高你的算力来达成更高的业务目标。Docker非常适合高密度的环境下使用，如果你想让仅有的少量资源发挥更大的作用，那么Docker也适合在这些小型或者中型的部署你的应用。

### 1.3 Docker architecture

Docker使用一个客户端-服务器的架构。Docker客户端与Docker守护线程进行交互，Docker的守护线程会进行大量的构建相关的操作，运行和分发你的Docker容器。Docker客户端和守护线程可以在同样的系统中运行，或者你可以用Docker客户端连接远程的Docker守护线程。Docker客户端和守护线程之间的交互是通过Unix套接字或者网络接口，使用REST风格的API进行的。另外，你可以组合使用一系列的容器进行应用的开发。



![architecture](/Users/liuxiangren/docker-learning/architecture.svg)

#### 1.3.1 Docker daemon

Docker的守护线程(dockerd)监听Docker API的请求，并且管理者Docker对象，像是images，containers，networks，还有volumes。一个守护线程同样可以与其他守护线程进行交互，来管理Docker的服务。

#### 1.3.2 Docker client

Docker 客户端(docker)是程序猿与Docker交互的主要的方式。当你使用诸如docker run这样的命令，那么客户端会将这些命令发送给Docker守护线程(dockerd)，守护线程将其搬运出来。docker命令使用Docker API。Docker客户端可以和过个Docker 守护线程进行通信。

#### 1.3.3 Docker registries

Docker注册表存储着Docker的images。Docker Hub是一个公共的注册表，任何人都可以使用它，并且Docker默认配置就是去Docker Hub上查找相关的images。你当然可以使用你的私有注册表。

当你使用docker pull或者docker run这样的命令时，你需要的images将会被从你配置的注册表中拉取下来。当你使用docker push命令时，你的image将会被发布到你配置的注册表中。

#### 1.3.4 Docker objects

当你使用Docker，你实际上就是在创建并且使用images、containers、networks、volumes、plugins和其他的对象，下面的小节将会对这些进行一个简要的概览。

**Images**

image是一个只读的模板，它是用来创建Docker Container的，通常，一个image会基于另一个image，还有一些额外的定制功能。举个例子，你可能会创建一个image，它基于ubuntu的image，但是安装了Apache的web服务器还有你开发的应用，同样的还有一些用于你运行自己应用的配置。

你可以创建你自己的镜像，或者你可以只通过依赖别人在注册表中发布的images。如果创建自己的image，你需要一个Dockerfile，使用简单的语法定义用来创建和运行image的步骤。Dockerfile中的每一个指令会在image创建一个层layer。当你改变Dockerfile并且重新构建image时，只有这些layer需要进行改变和重构，这也是让images变得如此轻量小巧和快速的一部分原因。

**Containers**

container是image的可运行的实例。你可以通过Docker API或者CLI。进行create、start、stop、move或delete一个container，你可以将一个container连接到一个或者更多的网络中，给他添加存储，或者以当前状态的container进行image的构建。

默认情况下，一个container对于别的container和他的宿主机是相对独立的。你可以通过控制容器的network，存储或者其他的container或者宿主机的底层子系统，来决定container的隔离方式。

container是由创建他的image来决定的，同样也取决你在运行或者创建它时，指定了哪些参数。当一个container被删除，任何对它状态的改变，都将不会被记录与持久化存储起来。

**Example docker run command**

下面就是一个运行ubuntu容器的命令，将ubuntu的/bin/bash命令绑定到你的命令行会话窗口中，使你可以操作这个ubuntu容器。

```dockerfile
$ docker run -i -t ubuntu /bin/bash
```

当你使用这个命令的时候，将会发生的事情(假定你使用默认的注册表配置)：

1. 如果你本地没有ubuntu的image，Docker将会从你配置的注册表中拉取。同样的你可以手动的运行docker pull ubuntu命令进行ubuntu image的拉取
2. Docker创建一个新的container，同样的你可以手动的运行docker container create命令，来创建新的caontainer
3. Docker会分配给这个container一个可读可写的文件系统，作为其基本的layer。这允许一个正在运行的container可以在它本地的文件系统中，进行创建或者修改他的文件或者目录
4. 由于你没有还指定任何的网络配置，Docker将会创建一个网络接口用以连接容器的默认网络。默认的网络配置包括，分配一个IP地址给这个container。默认的，这个container不能使用宿主机的网络，进行外部网络的连接。
5. Docker创建了这个ubuntu的容器，并且运行了/bin/bash命令。由于这个容器是可交互的运行状态，其绑定了你的命令行终端(由于-i和-t标识)，你可以通过你的命令行终端进行container的操作，而container的输出将会展现在你的命令行终端上来。
6. 当你使用exit命令来结束/bin/bash命令的时候，这个container将会停止，但并不会被删除，你可以重新运行或者删掉它

### 1.4 The underlying technology

Docker是使用Go语言进行开发的，使用了Go语言的一些Linxu内核的长处。Docker使用使用namespaces的技术来提供工作空间的独立性，就是container，当你运行一个container，Docker会为其创建一系列的namespaces。

这些namespaces提供了layer的独立性，container中的每一层都在一个独立的namespace下运行，并且只能通过namespace进行获取

### 1.5 镜像和容器的关系

镜像就好比Java类，容器就好比是Java类创建出来的对象

### 1.6 layer技术

Docker支持通过扩展现有的镜像，创建新的镜像。实际上，Docker Hub中99%的镜像都是通过在base镜像中安装和配置需要的软件构建出来的

```markdown
Dockerfile
#命令1
FROM debian
#		-	0 Kernel bootfs
#		-	1 Debian	==  Base Image

#命令2
RUN apt-get install emacs
#		-	0 Kernel bootfs
#		-	1	Debian  ==> 	Base Image
#		-	2	add emacs ==>		Image

#命令3
apt-get install apache2
#		-	0 Kernel bootfs
#		-	1	Debian  ==> 	Base Image
#		-	2	add emacs ==>		Image
#		- 3	add apache 	==>		Image
```

从上面的DockerFile命令可以看出，新镜像时从base镜像一层一层叠加生成的。没安装一个软件，就在现有的镜像基础上增加一层。

镜像分层最大的好处就是共享资源。比如说有多个镜像都从相同的base镜像构建而来，那么Docker Host只需在磁盘上保存一份base镜像；同时内存中也只需加载一份base镜像，就可以为所有容器服务了。而且镜像的每一层都可以被共享。

如果多个容器共享一份基础镜像，当某个容器修改了基础镜像的内容，比如/etc下的文件，这时其他容器的/etc是不会被修改的，修改只会被限制在单个容器内。这就是容器的Copy-on-Write特性。

### 1.7 **可写的容器层**

当容器启动时，一个新的可写层被加载到镜像的顶部。这一层通常被称作"容器层"，"容器层"之下的都叫做"镜像层"。

```markdown
#		-	0 Kernel bootfs
#		-	1	Debian  ==> 	Base Image
#		-	2	add emacs ==>		Image
#		- 3	add apache 	==>		Image
#		-	4	writable ===> Container
```

所有对容器的改动-无论添加、删除、还是修改文件都只会发生在容器层中。只有容器层是可写的，容器层下面的所有镜像层都是只读的。

镜像层数量可能会很多，所有的镜像层会联合在一起组成一个统一的文件系统。如果不同层中有一个相同路径的文件，比如/a，上层的/a会覆盖下层的/a，也就是说用户只能访问到上层中的文件/a。在容器层中，用户看到的是一个叠加之后的文件系统。

| 文件操作 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| 添加文件 | 在容器中创建文件时，新文件被添加到容器层中                   |
| 读取文件 | 在容器中读取某个文件时，Docker会从上往下一次在各个镜像层中查找此文件。一旦找到，立即将其复制到容器层，然后打开并读入内存中 |
| 修改文件 | 在容器中修改已存在的文件时，Docker会从上往下一次在各个镜像层中查找此文件，一旦找到将其复制到容器层，然后进行修改 |
| 删除文件 | 在同期中删除文件时，Docker也是从上往下一次在镜像层中查找此文件。找到后，会在容器层中记录下此删除操作。(知识记录删除操作) |

只有当需要修改时才复制一份数据，这种特性被称为Cpoy-on-Write。可见，容器层保存的是竞相变化的部分，不会对镜像本身进行任何的修改

```markdown
总结下来就是：容器层记录对镜像的修改，所有镜像层都是只读的，不会被容器修改，所以镜像可以被多个容器共享。
```

### 1.8 Volume数据卷

实际上我们的容器就好像是一个简易版的操作系统，只不过系统中只安装了我们的程序运行时所需要的环境，前面说到我们的容器是可以被删除的，那，如果将容器删除了，容器中的程序产生的需要被持久化的数据该怎么办呢？容器运行的时候我们可以进入容器中去查看，容器一旦删除就相当于清空了可以写入的容器层，数据就消失了。

所以数据卷就是来解决这个问题的，是用来将数据持久化到我们宿主机上，与容器间实现数据共享，简单的说就是将宿主机的目录映射到容器中的目录，应用程序在容器中的目录读写数据会同步到宿主机上，这样容器产生的数据就可以持久化了，比如我们的数据库容器，就可以把数据存储到我们宿主机上的真实磁盘中。

### 1.9 Registry注册中心

Docker使用Registry来保存用户构建的镜像。Registry分为公共和私有两种。Docker公司运营公共的Registry叫做Docker Hub。用户可以在Docker Hub中注册账号，分享并保存自己的镜像。

Docker公司提供了公共的镜像仓库(http://hub.docker.com，Docker称之为Repository)提供了庞大的镜像集合供用户使用。

一个Docker Registry中可以包含多个仓库(Repository)；每个仓库可以包含过个标签(Tag)；每个标签对应着一个镜像。

通常，一个仓库会包含同一个软件不同版本的镜像，而标签对应该软件的各个版本。我们可以通过<仓库名>:<标签>的格式来指定具体是这个软件哪个版本的镜像。如果不给出标签，则将以latest作为默认的标签。



## 2 Docker命令

### 2.1 images

```shell
#查看所有的docker镜像,centos上面写的是Repository仓库，tag可以理解为版本标签
docker images -a
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
centos       latest    300e315adb2f   6 months ago   209MB

#docker hub上的搜索，默认的就是docker官方的registry
docker search [mysql填上名字]
NAME              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
redis             Redis is an open source key-value store that…   9586      [OK]       
bitnami/redis     Bitnami Redis Docker Image                      185                  [OK]

#通过收藏来过滤，这个意思是大于2500的
docker search [mysql] --filter stars=2500

#列出所有镜像的id
docker images -q
```

#### 2.1.1 pull

```shell
#从registry中拉取镜像
docker pull mysql
Using default tag: latest 	- 默认拉取最新版latest
latest: Pulling from library/mysql
69692152171a: Pull complete 	- 进行分layer下载，image就是一个一个独立的layer组成的
1651b0be3df3: Pull complete 
951da7386bc8: Pull complete 
0f86c95aa242: Pull complete 
37ba2d8bd4fe: Pull complete 
6d278bb05e94: Pull complete 
497efbd93a3e: Pull complete 
f7fddf10c2c2: Pull complete 
16415d159dfb: Pull complete 
0e530ffc6b73: Pull complete 
b0a4a1a77178: Pull complete 
cd90f92aa9ef: Pull complete 
Digest: sha256:d50098d7fcb25b1fcb24e2d3247cae3fc55815d64fec640dc395840f8fa80969  - 签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest	- 真实地址

#从registry中拉取镜像，带版本号的
docker pull mysql:5.7
5.7: Pulling from library/mysql
69692152171a: Already exists - image基于分layer的原理，很多layer都是共用的
1651b0be3df3: Already exists 
951da7386bc8: Already exists 
0f86c95aa242: Already exists 
37ba2d8bd4fe: Already exists 
6d278bb05e94: Already exists 
497efbd93a3e: Already exists 
a023ae82eef5: Pull complete 
e76c35f20ee7: Pull complete 
e887524d2ef9: Pull complete 
ccb65627e1c3: Pull complete 
Digest: sha256:a682e3c78fc5bd941e9db080b4796c75f69a28a8cad65677c23f7a9f18ba21fa
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

#### 2.1.2 rmi

```shell
#remove image
docker rmi -f 2c9028880e58
Untagged: mysql:5.7
Untagged: mysql@sha256:a682e3c78fc5bd941e9db080b4796c75f69a28a8cad65677c23f7a9f18ba21fa
Deleted: sha256:2c9028880e5814e8923c278d7e2059f9066d56608a21cd3f83a01e3337bacd68
Deleted: sha256:c49c5c776f1bc87cdfff451ef39ce16a1ef45829e10203f4d9a153a6889ec15e
Deleted: sha256:8345316eca77700e62470611446529113579712a787d356e5c8656a41c244aee
Deleted: sha256:8ae51b87111404bd3e3bde4115ea2fe3fd2bb2cf67158460423c361a24df156b
Deleted: sha256:9d5afda6f6dcf8dd59aef5c02099f1d3b3b0c9ae4f2bb7a61627613e8cdfe562

#删除所有的镜像
docker rmi -f $(docker images -qa)

#删除指定镜像
docker rmi -f 'IMAGE_ID'

#删除多个指定镜像的
docker rmi -f 'IMAGE_ID1' 'IMAGE_ID2'....

#删除镜像 docker rmi repository名:tag标签
docker rmi centos:latest
```

### 2.2 container

```shell
#小栗子
docker pull centos

#docker run [可选传参数] image
#参数说明
#	--name="Name" 容器的名字 tomcat01、tomcat02，用来区分容器
#	-d						或者"--detch"，后台方式的运行
#	--interactive|-i		Keep STDIN(标准输入流) open even if not attached
#	--tty|-t		Allocate a pseudo-TTY（虚拟终端）
# -it						使用交互方式运行，进入容器查看内容
# --publish|-p	Publish a container's port(s) to the host
#		-p ip:主机端口:容器端口
#		-p 主机端口:容器端口(常用)
#		-p 容器端口

#查看所有容器
docker ps -a

#退出容器
exit

#退出但不关闭容器
ctrl+p+q [mac系统]

docker ps -f status=exited
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
容器ID					所属镜像						 创建时间		容器状态		容器端口	 容器名称
```

#### 2.2.1 查看停止的容器

```shell
docker ps -f status=exited 	#-f就是过滤的意思
```

#### 2.2.2 查看所有的容器(包括运行和停止的)

```shell
docker ps -a
```

#### 2.2.3 查看最后一次运行的容器

```shell
docker ps -l
```

#### 2.2.4 列出最近创建的n个容器

```shell
docker ps -n 5
```

#### 2.2.5 创建与启动容器

```shell
docker run [OPTIONS] IMAGE [COMMAND] [ARGS...]
```

- -i ：表示运行容器

- -t ：表示容器启动后会进入命令行。加入-i -t之后，容器创建就能登录进容器的命令行。即分配一个伪终端

- --name：为创建的容器命名

- -v：表示目录映射关系(前者是宿主机目录，后者是映射到宿主机上的目录)，可以使用多个-v做过个目录文件的映射。

  注意：最好做目录映射，在宿主机上做修改，然后共享到容器上

- -d：在run后面加上-d的参数，则会创建一个守护式的容器在后台运行(这样创建容器后不会自动登录容器，如果只加-i -t这两个参数，创建容器后就会自动进入容器内)

- -p：表示端口映射，前者是宿主机端口，后者是容器内的映射端口。可以使用多个-p做多个端口映射

- -P：随机使用宿主机的可用端口与容器内暴露的端口映射

#### 2.2.6 创建并进入容器

下面这行命令的意思是通过镜像AA创建一个容器BB，运行容器并机内容器的/bin/bash。

```shell
docker run -it --name 容器名称 镜像名称:镜像标签 /bin/bash
```

**注意：**容器运行必须有一个前台进程，如果没有前台进行执行，容器认为是空闲状态，就会自动退出

**案例：启动redis**

```shell
docker run --name mynginx -P nginx
命令运行之后，nginx就启动了，终端显示nginx的log信息

再重新打开一个窗口
docker ps -a
docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                     NAMES
2b852fe45c40   nginx     "/docker-entrypoint.…"   39 seconds ago   Up 37 seconds   0.0.0.0:55000->80/tcp, :::55000->80/tcp   mynginx

可以看到映射对应的端口为55000，打开浏览器访问localhost:55000看到Welcome to nginx部署成功

打开启动nginx的窗口按ctrl+c，进行退出
删除mynginx
docker rm mynginx|2b852fe45c40
```

启动另一种方式

```shell
docker run -it --name mynginx -p 80:80 nginx
同样也是可以启动的
```



#### 2.2.7 守护方式创建容器

```shell
docker run -di --name 容器名称 镜像名称:标签
```

#### 2.2.8 登录守护式容器的方式

```shell
docker exec -it 容器名称|容器ID /bin/bash
```

#### 2.2.9 停止与启动容器

```shell
#停止容器
docker stop 容器名称|容器ID

#启动容器
docker start 容器名称|容器ID
```

#### 2.2.10 文件拷贝

如果我们需要将文件拷贝到容器内可以使用cp命令。

```shell
docker cp 需要拷贝的文件或目录 容器名称:容器目录
```

也可以将文件从容器内拷贝出来。

```shell
docker cp 容器名称:容器目录 需要拷贝的文件或目录
```

#### 2.2.11 目录挂载(容器数据卷操作)

我们可以在创建容器的时候，将宿主机的目录与容器进行映射，这样我们就可以通过修改宿主机某个目录的文件从而去影响容器，而且这个操作时双向绑定 的，也就是说容器内的操作也会影响到宿主机，实现备份功能。

但是容器被删除的时候，宿主机的内容并不会被删除。如果多个容器挂载同一个目录，其中一个容器被删除，其他的容器的内容也不会受到影响。

```markdown
容器与宿主机之间的数据卷属于引用的关系，数据卷是从外界挂载到容器内部中的，所以可以脱离容器的生命周期而独立存在，正式由于数据卷的生命周期并不等同于容器的生命周期，在容器退出或者删除以后，数据卷仍然不会受到影响，数据卷的生命周期会一直持续到没有容器使用它为止。
```

创建容器添加-v参数，格式为 宿主机目录:容器目录，例如：

```shell
docker run -di -v /mydata/docker_centos/data:/usr/local/data --name centos7-01 cnetos:7
#多目录挂载
docker run -di -v /宿主机目录:/容器目录 -v /宿主机目录2:/容器目录2 镜像名
```

```markdown
目录挂载操作可能出现权限不足的情况，这时应为CentOS7中的安全模块SELinux把权限禁掉了，在docker run时通过 --privilege=true给该容器加权限来解决挂载的目录没有权限的问题
```

#### 2.2.12 匿名挂载

匿名挂载只需要写容器目录即可，容器对外应的目录会在/var/lib/docker/volumes中生成

```shell
#匿名挂载
docker run -di -v /usr/local/data --name centos7-02 centos:7
#查看volume数据卷信息
docker volume ls

DRIVER    VOLUME NAME
local     8ba4acaa40dd5fe7080c08b2d2a02384bf2a46896b23a46307b27593be5bb5e0
local     42dbda6ff6205b531063188e12bf5be24a30698b2793fea1135aa727e68f358d
local     592eeacc571a62374df8b5d947a9ed894047438379c1789ea7fbe1f1a49b1766
local     841d2a1e4498398744ad7ea34cf14de7468ebc44b03b7e7b71eef00d625b6673
local     1526ad59cc04316aa098868d04d78501cc29591395afbae82bf62c05a6d891c8

匿名方式怎么找对应的宿主机路径
docker inspect container_name #对应找其中的Mounts里面的Source

问：MacOS中找到的地址是错误的
答：
1.cd ~/Library/Containers/com.docker.docker/Data/vms/0
2.screen tty网上这样说的，但是没有试验成功，还是显示指定比较好吧

```

#### 2.2.13 具名挂载

具名挂载就是给数据卷起个名字，容器对外应的目录会在/var/lib/docker/volume中生成。

```shell
#具名挂载,这个路径只要你指定，docker会帮你创建/usr/local/data
docker run -di -v docker_centos_data:/usr/local/data --name centos7-03 centos:7

#查看volume
docker volume ls
```



#### 2.2.14 指定目录挂载

一开始给大家讲解的挂载方式就是属于指定目录挂载，这种方式的挂载不会再/var/lib/docker/volume目录生成内容

```shell
docker run -di /mydata/docker_centos_data:/usr/local/data --name centos7-01 centos:7

#多目录挂载
docker run -di -v /宿主机目录:/容器目录 -v /宿主机目录1:/容器目录1 镜像名

#创建挂载卷
docker run -di --name mynginx -p 8081:80 -v /Users/xxx/docker_volume/:/mynginx/docker_volume nginx  
522c982dbd1fb219def78123a5a65c6fa1c1ed39df00ce203663b14a43d91d0c

这样对应修改容器中的docker_volume中的内容
touch my.txt
找到宿主机的docker_volume目录页出现了my.txt文件，修改的话两者都可见的
```

#### 2.2.15 查看目录挂载关系

```shell
#可以先创建目录卷
docker volume create hello
#给容器挂载hello卷
docker run -d -v hello:/world nginx ls /world
```

docker[中文手册](https://www.php.cn/manual/view/36147.html)





通过docker volume inspect 数据卷名称，可以查看该数据卷对应宿主机的目录地址。

```shell
docker inspect mynginx
"Mounts": [
            {
                "Type": "bind",
                "Source": "/host_mnt/Users/xxx/docker_volume",
                "Destination": "/mynginx/docker_volume",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],
```

#### 2.2.16 目录挂载的只读/只写

```shell
#只读，只能通过修改宿主机内容实现对容器的数据管理
docker run -it -v /宿主机目录:/容器目录:ro 镜像名

#读写，默认。宿主机和容器可以进行双向操作
docker run -it -v /宿主机目录:/容器目录:rw 镜像名
```

#### 2.2.17 volumes-from(继承)

```shell
#容器centos7-01 指定目录挂载
docker run -di -v /mydata/docker_centos/data:/usr/local/data --name centos7-01 centos:7
#容器centos7-04 和 centos7-05相当于继承centos7-01容器的挂载目录
docker run -di --volumes-from centos7-01 --name centos7-04 centos:7
docker run -di --volumes-from centos7-01 --name centos7-05 centos:7
```



#### 2.2.18 查看容器的IP地址

我们可以通过以下命令查看容器的元信息

```shell
docker inspect 容器名称|容器ID
```

也可以直接执行下面的命令直接输出IP地址

```shell
docker inspect --format='{{.NetworkSettings.IPAddress}}' 容器名称|容器ID
```

我们一台服务器上部署了多个服务，一般都是将Container的port映射到宿主机上，通过宿主机的IP进行访问的



#### 2.2.19 删除容器

```shell
#删除之前容器必须处于非运行状态
docker rm 容器名称|容器ID
```





## 3 Docker镜像构建

我们可以通过公共仓库拉取镜像使用，但是，有些时候公共仓库拉取的镜像并不符合我们的呃需求。尽管已经从繁琐的部署工作中解放出来了，但是实际开发时，我们可能希望镜像包含整个项目的完整环境，在其他机器上拉取打包完整的镜像，直接运行即可。

Docker支持自己构建镜像，还支持将自己构建的进行上传至公共仓库，镜像构建可以通过以下两种方式来实现：

- docker commit ：从容器创建一个新的镜像；
- docker build：配合着dockerfile文件创建镜像

下面我们先通过docker commit来实现镜像的构建。

目标：接下来我们通过基础镜像centos:7，在该镜像中安装JDK和tomcat以后将其制作为一个新的镜像mycentos:7。

### 3.1 创建容器

```shell
#拉取镜像
docker pull centos:7

#创建容器
docker run -di --name centos7 centos:7
```

### 3.2 拷贝资源

```shell
#将宿主机的JDK和tomcat拷贝至容器
docker cp ~/Download/jdk1.8.0_281.jdk.tar.gz centos7:/root
dcoker cp ~/Download/apache-tomcat-8.5.56.tar.gz cnetos7:/root
```

### 3.3 进入容器

```shell
#这个/bin/bash可以直接写成 bash
docker exec -it centos7 bash
#那么就可以看到，jdk和tomcat都cp到容器内部了
```

### 3.4 JDK与Tomcat安装

```shell
#进入容器
dcoker exec -it centos7 bash
#切换至/root目录
cd /root
#创建java和tomcat目录
mkdir -p /usr/local/java
mkdir -p /usr/local/tomcat
#将jdk和tomcat解压至容器/usr/local/java 和 /usr/local/tomcat目录中
tar -zxvf jdk1.8.0_281.jdk.tar.gz -C /usr/local/java
tar -zxvf apache-tomcat-8.5.56.tar.gz -C /usr/local/tomcat
#配置jdk环境变量
vi /etc/profile
#在环境变量中添加以下内容
export JAVA_HOME=/usr/local/java/jdk1.8.0_281/
export PATH=$PATH:$JAVA_HOME/bin
#修改完成后，保存并退出
#重新加载环境变量文件
source /etc/profile
#测试环境变量是否配置成功
java -version
#删除容器里内的jdk和tomcat安装包
rm jdk1.8.0_281.jdk.tar.gz apache-tomcat-8.5.56.tar.gz -rf
```

这样虽然安装上去了，但是并没有在启动centos容器的时候指定暴露的端口号，最终的情况，在宿主机通过tomcat默认端口号8080访问，也是访问不到的，只能在容器内访问

```shell
curl http://localhost:8080
```

### 3.5 构建镜像

```shell
docker commit [OPTIONS] CONTAINER_NAME [REPOSITORY[:TAG]]
docker commit -a="myhelloworld" -m="jdk8 and tomcat8" centos7 mycentos:7
```

- -a：提交镜像作者
- -c：使用Dockerfile指令来创建镜像
- -m：提交时说明的文字
- -p：在commit时，将容器展厅



### 3.7 使用自己的镜像

```shell
#创建一个容器
docker run -di --name mycentos7 -p 8081:8080 mycentos:7

#进入咱们创建的容器
docker exec -it mycentos7 bash
```



**步骤的回顾**

```markdown
1.去官网下载了centos:7的镜像
2.根据官网镜像创建了一个容器
3.将宿主机的jdk和tomcat的资源拷贝进入容器
4.在容器内安装了jdk和tomcat
5.将这个已安装的jdk和tomcat的容器重新构建为一个新的镜像mycentos:7
```

我们操作了这么多步骤，其实有一个更可控，更简便的方式来创建我们自己的镜像

## 4 Dockerfile

在Docker中构建镜像像最常用的方式，就是使用Dockerfile。Dockerfile是一个用来构建镜像的文本文件，文本内容包含了一条条构建镜像所需的指令和说明。[官方API](https://docs.docker.com/engine/reference/builder/)

### 4.1 Dockerfile常用指令

#### 4.1.1 FROM

```markdown
语法：FROM <image>:<tag>
指明构建的新景祥是来自于哪个基础镜像，如果没有选择tag，那么默认值为latest
```

例子

```markdown
FROM centos:7
```

如果不以任何镜像为基础，那么写法为：FROM scratch。官方说明：scratch镜像是一个空镜像，可以用于构建busybox等超小镜像，可以说是真正的从零开始构建属于自己的镜像。

#### 4.1.2 ~~MAINTAINER(deprecated)~~

```markdown
语法：MAINTAINER <name>
指明镜像维护者及其联系方式(一般是邮箱地址)。官方说明已经过时，推荐使用LABEL。
```

```shell
MAINTAINER sennerming <sennerming@qq.com>
```

#### 4.1.3 LABEL

```shell
语法：LABEL <key>=<value> <key>=<value> <key>=<value>....
功能是为镜像指定标签。也可以使用LABEL来指定镜像的作者
```

#### 4.1.4 RUN

```shell
语法：RUN <COMMAND>
构建镜像时运行shell命令，比如构建新的镜像中我们想在/usr/local目录下创建一个java目录
```

```shell
RUN mkdir -p /usr/local/java
```

#### 4.1.5 ADD

```shell
语法：ADD <src>...<dest>
拷贝文件或目录到镜像中。src可以是一个本地文件或者是一个本地压缩文件，压缩文件会自动解压。还可以是一个url，如果把src写成一个url，那么ADD就类似于wget命令，然后自动下载和解压。
```

```shell
ADD jdk1.8.0_281.jdk.tar.gz /usr/local/java
```



#### 4.1.6 COPY

```shell
语法：ADD <src>...<dest>
拷贝文件或目录到镜像中。用法同ADD，只是不支持自动下载和解压
```

```shell
COPY jdk1.8.0_281.jdk.tar.gz /usr/local/java
```

#### 4.1.7 EXPOSE

```shell
语法：EXPOSE <port> [<port>/<protocal>...]
暴露容器运行时的监听端口给外部，可以指定端口是监听TCP还是UDP，如果未指定协议，则默认为TCP
```

```shell
EXPOSE 80/tcp
EXPOSE 80/udp
```

#### 4.1.8 ENV

```shell
语法：ENV <key> <value>添加单个，ENV <key>=<value>...添加多个
设置容器内环境变量
```

```shell
ENV JAVA_HOME /usr/local/java/jdk-1.8.0_281/
```

#### 4.1.9 CMD

```shell
语法：
CMD ["executable","param1","prarm2"]，比如：CMD ["/usr/local/tomcat/bin/catalina.sh","run"]
CMD ["param1","param2"]，比如：CMD ["echo","$JAVA_HOME"]
CMD command param1 param2，比如：CMD echo $JAVA_HOME

启动容器时执行的Shell命令。在Dockerfile中只能有一条CMD命令。如果有一条CMD指令。如果设置了多条CMD，只有最后一条CMD会生效
```

```shell
CMD echo $JAVA_HOME
```

```markdown
如果创建容器的时候指定了命令，则CMD命令会被替代。加入镜像叫centos:7，创建容器时命令时：docker run -it --name centos7 centos:7 echo "helloworld"或者docker run -it --name centos7 centos:7 /bin/bash，就不会输出$JAVA_HOME的环境变量信息了，因为CMD命令被echo "helloworld"、/bin/bash覆盖了。
```



#### 4.1.10 ENTRYPOINT

```shell
语法：
ENTRYPOINT ["executable","param1","param2"]，比如：ENTRYPOINT ["/usr/local/tomcat/bin/catalina.sh","run"]
ENTRYPOINT command param1 param2，比如：ENTRYPOINT echo $JAVA_HOME
```

启动容器时执行的Shell命令，同CMD类似，不会被docker run 命令行指定的参数所覆盖。在Dockerfile中只能有一条ENTRYPOINT指令。如果设置了多条ENTRYPOINT，只有最后一条ENTRYPOINT会生效。

```shell
ENTRYPOINT echo $JAVA_HOME
```

```shell
如果在Dockerfile中同时写了ENTRYPOINT和CMD，并且CMD指令不是一个完整的可执行命令，那么CMD指定的内容将会作为ENTRYPOINT的参数；

如果在Dockerfile中同时写了ENTRYPOINT和CMD，并且CMD是一个完整的指令，那么它两会互相覆盖，谁在最后谁生效
```

#### 4.1.11 WORDIR

```shell
语法：WORKDIR /path/to/workdir
为RUN、CMD、ENTRYPOINT以及COPY和AND设置工作目录。docker run一进来就是这个目录
```

```shell
WORKDIR /usr/local
```

#### 4.1.12 VOLUME

```shell
指定容器挂载点到宿主机自动生成的目录或其他容器。一般的使用场景为需求持久化存储数据时。
```

```shell
#容器的/var/lib/mysql目录会在运行时自动挂载为匿名卷，匿名卷在宿主机的/var/lib/docker/volumes目录下
VOLUME ["/var/lib/mysql"]
```

```shell
一般不会再Dockerfile中用到，更常见的还是在docker run的时候通过 -v 指定数据卷。
```





### 4.2 Dockerfile实战

#### 4.2.1 关于 . 的理解

我们在使用docker build命令去构建镜像时，往往会看到命令最后会有一个"."号。他究竟是什么意思呢？

```markdown
很多人以为是用来指定Dockerfile文件所在的位置的，但其实-f参数才是用来指定Dockerfile的路径的，那么”.”号就行是用来做什么的呢？
Docker在运行时分为Docker引擎(服务端守护进程)和客户端工具，我们日常使用各种docker命令，其实就是在客户端工具与Docker引擎进行交互。
当我们使用docker build 命令来构建镜像的时候，这个构建过程其实是在Docker引擎中完成的，而不是在本机环境。如果在Dockerfile中使用了一些ADD等指令来操作文件，如何让Docker引擎获取这些文件呢？
这里就有一个镜像构建上下文的概念，当构建的时候，由用户指定构建镜像时的上下文路径，而docker build会将这个路径下所有的文件都打包上传给Docker引擎，引擎内将这些内容展开后，就能获取到上下文中的文件了。
```

举个例子：我的宿主机JDK在/root目录下，Dockerfile文件在/usr/lcoal/dockfile目录下，文件内容如下：

```shell
ADD jdk-1.8.0_281_linux-x64_bin.tar.gz /usr/local/java
```

那么构建镜像时的命令就该这样写：

```shell
docker build -f /usr/local/dockerfile/Dockerfile -t mycentos:7 /root
```

再举个栗子：

我的宿主机JDK文件和Dockerfile文件都在/usr/local/dockerfile目录下，文件内容如下：

```shell
ADD jdk-1.8.0_281_linux-x64_bin.tar.gz /usr/local/java
```

那么构建镜像时的命令则这样写：

```shell
docker build -f /usr/local/dockerfile/Dockerfile -t mycentos:7 .
```

#### 4.2.2 进行搭建

接下来我们通过基础镜像centos:7，在该镜像中安装jdk和tomcat，然后将其制作为一个新的镜像mycentos:7。

```shel
#创建目录
mkdir -p /usr/loacl/dockerfile
```



```shell
#编写Dockerfile文件
vi Dockerfile
```

Dockerfile文件内容

```shell
#指明构建的新镜像是来自于centos:7基础镜像
FROM centos：7
#通过镜像标签声明了作者的信息
LABEL maintainer="musician.club"
#设置工作目录
WORKDIR /usr/local
#新镜像构建成功以后创建指定目录
RUN mkdir -p /usr/local/java && mkdir -p /usr/local/tomcat
#拷贝文件到镜像中并解压
ADD jdk1.8.0_281.jdk.tar.gz /usr/local/java
ADD apache-tomcat-8.5.56.tar.gz /usr/local/tomcat
#暴露容器运行的8080监听端口给外部
EXPOSE 8080
#设置容器内JAVA_HOME环境变量
ENV JAVA_HOME /usr/local/java/jdk1.8.0_281/
ENV PATH $PATH:$JAVA_HOME/bin
#启动容器时启动tomcat，run运行阻塞控制台进行log输出，容器里面必须有个前台任务在执行，要不然容器就推出了
CMD ["/usr/local/tomcat/apache-tomcat-8.5.56/bin/catalina.sh","run"]
```

docker镜像并没有进行任何程序的运行

```shell
docker pull hello-world

docker run -di myhelloworld hello-world

docker ps	#这个是查不到的，因为上面没有前台的任务

退出原因
1、docker容器运行必须有一个前台进程， 如果没有前台进程执行，容器认为空闲，就会自行退出
2、容器运行的命令如果不是那些一直挂起的命令（ 运行top，tail、循环等），就是会自动退出
```

```markdown
解决方案
方案1：
网上有很多介绍，就是起一个死循环进程，让他不停的循环下去，前台永远有进程执行，那么容器就不会退出了

[root@localhost opt]# docker run -d -p 8000:8000 alpine /bin/sh -c "while true; do echo hello world; sleep 1; done"

方案2：
[root@localhost opt]# docker run -dit -p 8000:8000 alpine /bin/bash

添加-it 参数交互运行
添加-d 参数后台运行
这样就能启动一直停留在后台运行。
```



构建镜像

```shell
#-f /usr/local/dockerfile/Dockerfile 这个表示Dockerfile文件在哪
#~/Download/ 为了能让ADD命令找到jdk还有tomcat的压缩包
docker build -f /usr/local/dockerfile/Dockerfile -t mycentos:7 ~/Download/
```

通过我么我们自己的image创建容器

```shell
docker run -di --name mycentos7 -p 8080:8080 mycentos:7
```



### 4.3 Docker镜像的备份恢复和迁移

学会了如果构建镜像，为了备份该镜像，我们又几个以下的选择：

- 我们可以将指定镜像保存成tar归档文件，需要使用时将tar包恢复为镜像即可；
- 登录Dockerhub注册中心，将镜像推送至DockerHub仓库方便使用
- 搭建私有镜像仓库，将镜像推送至私有镜像仓库方便使用

接下来通过tar归档文件的方式实现镜像备份恢复和迁移

#### 4.3.1 镜像备份

使用docker save将指定镜像保存成tar归档文件。

```shell
docker save [OPTIONS] IMAGE [IMAGE...]
docker save -o /root/mycentos7.tar mycentos:7
```



```markdown
-o : 镜像打包后归档文件输出的目录
```

#### 4.3.2 镜像恢复

使用docker load导入docker save 命令导出的镜像归档文件。

```shell
docker load [OPTIONS]
docker load -i mycentos7.tar
```

- --input，-i：指定导入的文件

- --quiet，-q：精简输出信息

### 4.4 镜像迁移

镜像迁移同时涉及到了上面两个操作，备份和恢复

我们可以将任何一个Docker镜像从一台机器迁移到另一台机器。在迁移过程中，首先我们要把容器构建为Docker镜像。然后，该Docker镜像被作为tar包文件保存到本地。此时只需要拷贝或移动该镜像到我们想要的机器上，恢复该镜像并运行容器即可。

当然除了这种方式之外，我们还可以使用镜像仓库实现镜像的备份恢复和迁移，接下来我们就学习一下如何使用DockerHub的镜像仓库。

## 5 Docker Hub

之前我们使用的镜像都是从DockerHub公共仓库拉取的，我们也学习了如何制作自己的镜像，但是通过tar包的方式实现镜像的备份恢复迁移对于团队协作开发并不是特别的友好，我们也可以将镜像推送至DockerHub仓库方便使用。

**注意：如果构建的镜像内携带了项目数据，建议还是使用私有仓库比较好**

### 5.1 登录账号

通过docker login命令输入账号密码登录DockerHub

```shell
docker login
Authenticating with existing credentials...
Login Succeeded
```

#### 5.1.2 推送镜像至仓库

为了方便测试，我们将hello-world镜像拉取至本地，然后再上传至DockerHub仓库中

先给镜像设置标签：docker tag [local-image]:[tagname] [new-repo]:[tagname]

再将镜像推送至仓库 docker push [new-repo]:[tagname]

```shell
docker tag hello-world:latest sennerming/test-hello-world:1.0.0
docker push sennerming/test-hello-world:1.0.0
```

#### 5.1.3 查看仓库

```shell
docker images可以查看到，登录docker hub也可以看得到
```

#### 5.1.4 拉取镜像

```shell
docker pull sennerming/test-hello-world:1.0.0
```

#### 5.1.5 退出Docker Hub

通过docker logout命令退出Docker Hub

```shell
docker logout
Removing login credentials for ....
```

Docker Hub镜像仓库的使用，在生产上使用Docker镜像可能包含我们的代码、配置等信息，不想让外部人员获取，只允许内网的开发人员下载，怎么解决呢？可以通过构架私有镜像仓库进行实现

### 5.2 DockerHub私有仓库搭建

DockerHub，你项目大的话，上传上去要收费吧，可以通过一个registry镜像构建一个私有的“DockerHub”

Docker Hub为我们提供了很多官方进行和个人上传的镜像，我们可以下载机构或个人提供的镜像，也可以上传我们自己的本地镜像，但缺点是：

- 由于网络的原因，从DockerHub下载和上传镜像速度可能会比较慢
- 在生产上使用的Docker镜像可能包含我们的代码、配置信息等，不想被外部人员获取，只允许内网的开发人员下载。

为了解决以上问题，Docker官方提供了一个叫做registry的镜像用于搭建本地私有仓库使用，在内部网络大件的Docker私有仓库可以使内网人员下载、上传都非常徐苏，不受外网带宽等因素的影响，同时不再内网的人员也无法下载我们的镜像，并且私有仓库也支持配置仓库认证功能。接下来详细讲解registry私有仓库的搭建过程。

#### 5.2.1 拉取私有仓库镜像

拉取私有仓库镜像

````shell
docker pull registry
````

#### 5.2.2 运行registry

```shell
docker run -d -p 5000:5000 --name myregistry -v /Users/xxx/docker/registry:/var/lib/registry registry
```

-d后台运行  -p指定端口 -v把registry的镜像路径/var/lib/registry映射到本机的Users/huangenai/docker/registry

```shell
//查看运行容器
docker ps
```

进入容器

```shell
//进入容器  REGISTRY_CONTAINER_ID是容器id 在上一步骤中获得 
sudo docker attach REGISTRY_CONTAINER_ID
```

#### 5.2.3 修改配置

修改daemon.json文件。

```shell
本地仓库非安全配置 user/<username>/.docker/daemon.json  [MAC OS]
```



```shell
vi /etc/docker/daemon.json [Centos7]
```

```json
#127.0.0.1就是私有镜像仓库搭建的IP地址
cat .docker/daemon.json 
{
  "insecure-registries" : [
    "127.0.0.1:5000"
  ],
  "debug" : true,
  "experimental" : true,
  "registry-mirrors" : [
    "https://8q2dp9p9.mirror.aliyuncs.com"
  ]
}
```

#### 5.2.4 查看仓库的镜像

```shell
curl -XGET http://127.0.0.1:5000/v2/_catalog
{"repositories":[]}
```

registry container打印出

```shell
time="2021-06-20T12:54:34.527333994Z" level=info msg="response completed" go.version=go1.11.2 http.request.host="127.0.0.1:5000" http.request.id=5d58a113-c5ed-4403-a685-0299098d590e http.request.method=GET http.request.remoteaddr="172.17.0.1:64508" http.request.uri="/v2/_catalog" http.request.useragent="curl/7.64.1" http.response.contenttype="application/json; charset=utf-8" http.response.duration=1.892364ms http.response.status=200 http.response.written=20 
172.17.0.1 - - [20/Jun/2021:12:54:34 +0000] "GET /v2/_catalog HTTP/1.1" 200 20 "" "curl/7.64.1"
```



#### 5.2.5 推送镜像至私有仓库

先给镜像设置标签

```shell
docker tag [local-image]:[tagname] [new-repo]:[tagname]
```

再将镜像推送至私有仓库

```shell
docker tag hello-world:latest 127.0.0.1:5000/test-hello-world:1.0.0
docker push 127.0.0.1:5000/test-hello-world:1.0.0

curl -XGET http://127.0.0.1:5000/v2/_catalog
{"repositories":["test-hello-world"]
```

再次查看，打开浏览器输入：http://127.0.0.1:50000/v2/_catalog可以看到私有仓库中已经上传的镜像

或者本地挂载卷目录查看

```shell
#使用命令查看当前目录下有哪些文件 ls
test-hello-world
username@appledeMBP repositories % pwd #查看当前路径
/Users/username/docker_volume/registry/docker/registry/v2/repositories
```



还可以再从这个地址拉下来运行

```shell
docker run -it --name my-hello-world 127.0.0.1:5000/test-hello-world:1.0.0
```



使用[Breezes](https://gitee.com/kbsonlong/Breezes)，实现web管理端

```shell
git clone https://git.oschina.net/kbsonlong/Breezes.git
```

找到里面的Dockerfile文件，修改如下

```shell
FROM centos

MAINTAINER Mr.tao <staugur@saintic.com>

ADD src /Breezes

ADD misc/supervisord.conf /etc/supervisord.conf

ADD requirements.txt /tmp

WORKDIR /Breezes

RUN yum -y update 
RUN yum -y install wget gcc python-devel 
RUN wget https://bootstrap.pypa.io/get-pip.py 
RUN python get-pip.py 
RUN pip install --timeout 30 --index https://pypi.douban.com/simple/ -r /tmp/requirements.txt

EXPOSE 10210

ENTRYPOINT ["supervisord"]
```

```shell
//构建镜像
docker build -t breezes .

//运行镜像
docker run -d -p 10210:10210 --restart=always -h breezes \
--name breezes breezes

//保存镜像到私有仓库
docker tag breezes 127.0.0.1:5000/breezes
docker push 127.0.0.1:5000/breezes
```

打开 http://0.0.0.0:10210/ui/

上面这个例子已经包含了如何创建一个镜像以及将它存入私有仓库了，这里就不再重复了。

参考文章：[Mac Docker私有镜像仓库](https://www.cnblogs.com/huangenai/p/10012672.html)



### 5.3 配置私有仓库的认证

#### 5.3.1 自签证书

私有仓库已经搭建好了，要确保私有仓库的安全性，还需要一个安全认证证书，防止发生意想不到的事情，所以还需要在搭建私有仓库的Docker主机上生成自签证书。一般都买证书，不用自签证书，到时候公司会买的。

创建证书存储目录

```shell
mkdir -p /usr/local/registry/certs
```

生成自签名证书命令

mac字签证书生成-[钥匙串访问使用手册](https://support.apple.com/zh-cn/guide/keychain-access/kyca8916/mac)

下面是centos的自签证书生成办法

```shell
openssl req -newkey rsa:2048 -nodes -sha256 -keyout /usr/local/registry/certs/domain.key -x509 -days 365 -out /usr/local/registry/certs/domain.crt
```

- openssl req：创建证书前ing请求等功能；
- -newkey：创建CSR证书签名文件和RSA私钥文件；
- rsa:2048：指定创建的RSA私钥长度为2048；
- -node：对私钥不进行加密
- -sha256：使用SHA256算法
- -keyout：创建的私钥文件名称及位置
- -x509：自签发证书格式
- -days：证书有效期；
- -out：指定CSR输出文件名称及位置

#### 5.3.2 生成自签名证书

通过openssl生成自签名证书，运行命令以后需要填写一些整数信息，里面最关键的部分是：Common Name(eg,your name or your server's hostname) [] : 127.0.0.1，这里填写的是私有仓库的IP地址。

#### 5.3.3 生成鉴权密码文件

```shell
#创建存储鉴权密码文件目录
mkdir -p /usr/local/registry/auth
#如果没有htpasswd功能需要安装httpd
yum intall -y httpd
#创建用户和密码
htpasswd -Bbn root 1234 > /usr/local/registry/auth/htpasswd
```

- htpasswd是apcahe http的基本认证文件，使用htpasswd命令可以生成用户及密码文件

#### 5.3.4 创建私有仓库容器

```shell
#下面的所有是一条命令喔
docker run -di --name registry -p 5000:5000 \
-v /mydata/docker_registry:/var/lib/registry \
-v /usr/local/registry/certs:/certs \
-v /usr/local/registry/auth:/auth \
-e "REGISTRY_AUTH=htpasswd" \
-e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
-e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
-e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
-e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
registry
```

#### 5.3.5 推送镜像至私有仓库失败

先给镜像设置标签

```shell
docker tag [local-image]:[tagname] [new-repo]:[tagname]
```

再将镜像推送至私有仓库

```shell
docker push [new-repo]:[tagname]
```

命令

```shell
docker tag hello-world:latest 127.0.0.1:5000/test-hello-world:1.0.0
docker push 127.0.0.1:5000/test-hello-world:1.0.0
```

如果直接push镜像肯定会失败，并且出现no basic auth credentials的错误，这时因为我们没有进行登录认证。

#### 5.3.6 登录账号

通过docker login命令输入账号密码登录私有仓库

```shell
#先docker logout
docker login 127.0.0.1:5000
Username: root
Password: 1234 #就是上面5.3.3中的的htpasswd -Bbn root 123这个
```

#### 5.3.7 再次推送

再次push镜像，发现已经可以推送成功了

```shell
docker push 127.0.0.1:5000/test-hello-world:1.0.0
```

#### 5.3.8 退出账号

通过docker logout命令退出账号

```shell
docker logout 127.0.0.1:5000
Removing login credentials for 127.0.0.1:5000
```

私有镜像仓库的搭建还可以通过Harbor实现，Harbor是由VMware公司开源的企业级Docker Registry管理项目，它包括权限管理(RBAC)、LDAP、日志审核、管理界面、自我注册、镜像复制和中文支持等功能。

## 6 Docker网络模式

Docker网络模式详解及容器间网络通信

当项目大规模使用Docker时，容器通信的问题也就产生了。要解决容器通信问题，必须先了解很多关于网络的知识。Docker作为目前最火的轻量级同期基数，有很多令人称道的功能，如Docker的镜像管理。然而，Docker同样有着很多不完善的地方，网络方面就是Docker的网络知识，以满足更高的网络需求。

比如我启动一个docker container，用docker inspect去查看，Networks中他的ip地址是随机生成的，你重启一次，IP地址随机一次，我应用程序的配置文件就要修改一次，这不就尴尬了嘛？

我们要实现，容器与容器之间使用容器名称进行通信，多厉害是吧？

打开装有docker的机器

```shell
#查看网络(linux命令)
ip a
会看到有个名为docker0的网络接口，以此创建bridge虚拟网桥
```



### 6.1 默认网络

安装Docker以后，会默认创建三种网络，可以通过docker network ls查看。

```shell
docker network ls
NETWORK ID     NAME           DRIVER    SCOPE
4f9ed9e89981   bridge         bridge    local
9ac2e09ac2af   hadoop-hbase   bridge    local
37f7906cf8ed   host           host      local
374f4cd242c9   none           null      local
```

在学习Docker网络之前，我们有必要先来了解一下这几种网络模式都是什么意思。

| 网络模式  | 简介                                                         |
| --------- | ------------------------------------------------------------ |
| bridge    | 为每一个容器分配、设置IP等，并将容器连接到一个docker0虚拟网桥，默认为该模式 |
| host      | 容器将不会虚拟出自己的网卡，配置自己的IP等，而是使用宿主机的IP和端口 |
| none      | 容器有独立的Network namespace，但并没有对其进行任何网络设置，如配置veth pair和网桥连接，IP等 |
| container | 新创建的容器不会创建自己的网卡和配置自己的IP，而是和一个指定的容器共享、端口范围等。 |

#### 6.1.1 bridge网络模式

在该模式中，Docker守护进程创建了一个虚拟以太网桥docker0，新建的容器会自动桥接到这个接口，附加在其上的任何网卡之间都能自动转发数据包。

默认情况下，守护进程会创建一对对等的虚拟设备接口veth pair，将其中一个接口设置为容器的eth0接口(容器的网卡)，另一个接口放置在宿主机的命名空间中，以类似vethxxx这样的名字命名，从而将宿主机上的所有容器都连接到这个内部网络上。

比如我运行一个机遇busybox镜像构建的容器bbox01，查看ip addr：

```shell
busybox被称为嵌入式Linux的瑞士军刀，整合了许多小的unix下的通用功能到一个小的可执行文件中。
```

```shell
docker run -it --name bbox01 busybox
```

然后宿主机通过ip addr(linux)查看信息如下：

```shell
#mac os
ipconfig
```

通过以上的比较可以发现，证实了之前所说的：守护进程会创建一对对等虚拟设备接口veth pair，将其中一个接口设置为容器的eth0接口(容器网卡)，另外一个接口放置在宿主机的命名空间中，以类似vethxxx这样的名字命名。

同时，守护进程还会从网桥docker0的私有地址空间中分配一个iP地址和子网给该容器，并设置docker0的ip地址为容器的默认网关。也可以安装 yum install -y bridge-utils以后，通过brctl show命令查看网桥信息。

对于每个容器的IP地址和Gateway信息，我们可以通过docker inspect 容器名称|容器ID 进行查看，在NetworkSettings节点中可以看到详细信息。

我们可以通过docker network inspect bridge查看所以bridge网络模式下的容器，在Containers节点中可以看到容器名称。

```shell
docker network inspect brigde
```



```markdown
关于bridge网络模式IDE使用，只需要在创建容器时通过参数--net bridge或者--network bridge指定即可，当然这也是创建容器默认使用的网络模式，也就是说这个参数是可以省略的
```

![bridge](/Users/liuxiangren/docker-learning/bridge.jpg)

Bridge桥接模式的实现步骤

1. Docker Daemon利用veth pair技术，在宿主机上创建一对对等虚拟网络接口设备，假设为veth0和veth1.而veth pair技术的特性可以保证无论哪一个veth接收到网络报文，都会将报文传输给另一方。
2. Docker Daemon将veth0附加到Docker Daemon创建的docker0网桥上。保证宿主机的网络报文可以发往veth0
3. Docker Daemon将veth1添加到Docker Container所属的namespace下，并被改名为eth0，如此一来，宿主机的网络报文若发往veth0，则立即会被Container的eth0接收，实现宿主机到Docker Container网络的连通性；同时，也保证Docker Container单独使用eth0，实现容器网络环境的隔离性。



docker的桥接网络使用虚拟网桥，bridge网络用于同一主机上的docker容器相互通信，连接到同一个网桥的docker容器可以相互通信，当我们启动docke时，会自动创建一个默认bridge网络，除非我们进行另外的配置，新创建的容器都会自动连接到这个网络，我们也可以自定义自己的bridge网络，docker文档建议使用自定义bridge网络，默认的bridge网络具有一定的缺陷

连接到同一bridge网络的容器可以相互访问彼此任意一个端口，如果不发布端口，外界将无法访问这些容器，在创建容器时，通过-p或是--publish指令发布端口

自定义bridge网络与默认bridge网络对比：

默认桥接网络中的容器只能通过IP地址访问其他容器（除非使用遗留的-link指令连接两个容器），而自定义桥接网络提供DNS解析，可以通过容器的名字或是别名访问其他容器
容器可以自由的进入或是退出自定义桥接网络，如果想要退出默认桥接网络，需要先停止容器的运行，然后重新创建该容器，并指定需要连接的其他网络
如果更改了默认桥接网络的网络配置，需要重新启动docker，并且由于默认桥接网络只有一个，因此所有容器的网络配置都是一样的，而用户自定义网络可以在创建时指定网络配置（例如默认网关、MTU等），不需要重启docker，灵活性更高
在默认桥接网络中，可以通过--link参数连接两个容器来共享环境变量，用户自定义网络中无法使用这种方式，但是docker提供了更好的方式：                 

1、多个容器可以使用docker volume（这是docker存储数据的一种方式，以后会介绍）挂载到同一个文件，在文件中指明环境变量，从而实现所容器的环境变量共享                                                                                                                                                                                                                   2、多个容器可以使用同一个docker-compose（与docker service有关）文件启动 ，可以在该文件中定义共享环境变量                                                  

 3、可以使用swarm services，并且通过  secrets 和 configs  （这两个还没看）实现环境变量共享

#### 6.1.2 host网络模式

```markdown
host网络模式需要在创建容器时通过参数--net host或者--network host指定
```

```markdown
采用host网络模式的Docker Container，可以直接使用宿主机的IP地址与外界进行通信，若宿主机的eth0是一个共有IP，那么容器也拥有这个公有的IP。同时容器内服务的端口也可以直接使用宿主机的端口，无需额外的NAT转换；
```

```markdown
host网络模式可以让容器共享宿主机网络栈，这样的好处是外部主机与容器直接通信，但是容器的网络缺少隔离性。
```

![host](/Users/liuxiangren/docker-learning/host.jpg)

比如我基于host网络模式创建了一个基于busybox镜像构建的容器bbox02，查看ip addr：

```shell
docker run -it --name bbox02 --network host busybox
```

#### 6.1.3 none网络模式

```markdown
none网络模式是指禁用网络功能，只有lo接口(local)的缩写，代表217.0.0.1，即localhot本地环回接口。在创建容器时通过参数 --net none 或者 --network none指定；
```

```markdown
none网络模式即不为Docker Container创建任何的网络环境，容器内部就只能使用loopback网络设备，不会再有其他的网络资源。可以说none模式为Docker Container做了极少的网络设定，但是俗话说的好“少”即是多，在没有网络配置的情况下，作为Docker开发者，才能在这基础上做其他无限多可能的网络定制开发。这也恰巧体现了Docker设计理念的开放。
```

比如我基于none网络模式创建了一个基于busybox镜像构建的容器bbox03，查看ip addr

```shell
docker run -it --name bbox03 --net none busybox
```

我们可以通过

```shell
docker network inspect none 查看所有none网络模式下的容器，在Containers节点中可以看到容器的名称
```

#### 6.1.4 container网络模式

```markdown
Container网络模式是Docker中一种较为特别的网络模式，在创建容器时通过参数--net container:已经运行的容器名称|ID 或者 --network container:已经运行的容器名称|ID 指定；
```

```markdown
处于这个模式下的Docker容器会共享一个网络栈，这样两个容器之间可以使用localhost高效快速通信。
```

![container](/Users/liuxiangren/docker-learning/container.jpg)

**Container网络模式即新创建的容器不会创建自己的网卡，配置自己的IP，而是和一个指定的容器共享IP、端口范围等。**同样两个容器除了网络方面相同之外，其他的如文件系统、进程列表等还是隔离的

#### 6.1.5 ~~link~~

docker run --link 可以用来连接两个容器，使得源容器(被连接的容器)和接收容器(主动去链接的容器)之间可以互相通信，并且接收容器可以获取源容器的一些数据，如源容器的环境变量。

这种方式官方已经不推荐使用，并且在未来版本可能会被移除了



### 6.2 使用自定义网络

虽然Docker提供的默认网络使用比较简单，但是为了保证各个容器中应用的安全性，在实际开发中更推荐使用自定义的网络进行容器管理，以及启用容器名称到IP地址的自动DNS解析

```markdown
从Docker1.10版本看是，Docker Daemon实现了一个内嵌的DNS Server，使容器可以直接通过容器名称进行通信。方法很简单，只要在创建容器时使用--name 为容器命名即可。
但是使用Docker DNS有个显示：只能在user-defined网络中使用。也就是说，默认的bridge网络是无法使用DNS的，所以我们就需要自定义网络。
```

默认的Bridge只支持容器内通过容器ip进行互通

可以开两个容器

```shell
docker run -it --name bbox01 busybox
docker run -it --name bbox05 busybox

通过docker inspect bbox01和docker inspect bbox05进行容器ip地址的查看

在bbox01下ping bbox05的ip可以ping同，同样的bbox05下ping bbox01的ip也是可以ping通的
```

如果一个container装的是tomcat，一个装的是nginx，那你这个一重启，又重新分配了ip，那就歇菜了

#### 6.2.1 创建网络

通过docker network create命令可以创建自定义网络模式，命令提示

```shell
docker network --help

Usage:  docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks
```

进一步查看docker network create命令使用详情，发现可以通过--driver指定网络模式且默认是bridge网络模式，提示如下：

```shell
docker network create --help
Usage:  docker network create [OPTIONS] NETWORK

Create a network

Options:
      --attachable           Enable manual container attachment
      --aux-address map      Auxiliary IPv4 or IPv6 addresses used by Network driver (default map[])
      --config-from string   The network from which to copy the configuration
      --config-only          Create a configuration only network
  -d, --driver string        Driver to manage the Network (default "bridge")
      --gateway strings      IPv4 or IPv6 Gateway for the master subnet
      --ingress              Create swarm routing-mesh network
      --internal             Restrict external access to the network
      --ip-range strings     Allocate container ip from a sub-range
      --ipam-driver string   IP Address Management Driver (default "default")
      --ipam-opt map         Set IPAM driver specific options (default map[])
      --ipv6                 Enable IPv6 networking
      --label list           Set metadata on a network
  -o, --opt map              Set driver specific options (default map[])
      --scope string         Control the network's scope
      --subnet strings       Subnet in CIDR format that represents a network segment
```

创建一个基于bridge网络模式的自定义网络模式custom_network，完整命令如下

```shell
docker network create custom_network
ae1edcfb0762cdb60a98e8fe98d10e2303d58a0304fe55d5d263cc2fa9e0bcba
```

通过docker network ls查看网络模式

```shell
docker network ls
NETWORK ID     NAME             DRIVER    SCOPE
4f9ed9e89981   bridge           bridge    local
ae1edcfb0762   custom_network   bridge    local
9ac2e09ac2af   hadoop-hbase     bridge    local
37f7906cf8ed   host             host      local
374f4cd242c9   none             null      local
```

通过自定义网络模式custom_network创建容器

```shell
docker run -it --name bbox02 --network custom_network busybox
docker run -it --name bbox03 --network custom_network busybox
```

通过docker inspect 容器名称|容器ID 查看容器的网络信息，在NetworkSettings节点中可以看到详细信息。

通过容器名进行网络访问

```shell
docker run -it --name bbox02 --network custom_network busybox
/ # ping bbox03
PING bbox03 (172.19.0.3): 56 data bytes
64 bytes from 172.19.0.3: seq=0 ttl=64 time=0.181 ms
64 bytes from 172.19.0.3: seq=1 ttl=64 time=0.174 ms
64 bytes from 172.19.0.3: seq=2 ttl=64 time=0.160 ms
64 bytes from 172.19.0.3: seq=3 ttl=64 time=0.111 ms


docker run -it --name bbox03 --network custom_network busybox
/ # ping bbox02
PING bbox02 (172.19.0.2): 56 data bytes
64 bytes from 172.19.0.2: seq=0 ttl=64 time=0.095 ms
64 bytes from 172.19.0.2: seq=1 ttl=64 time=0.167 ms
64 bytes from 172.19.0.2: seq=2 ttl=64 time=0.116 ms
64 bytes from 172.19.0.2: seq=3 ttl=64 time=0.083 ms
```

完全没有问题



#### 6.2.2 连接网络

通过docker network connect 网络名称 容器名称 为容器连接新的网络模式

```shell
docker network connect --help
Usage:  docker network connect [OPTIONS] NETWORK CONTAINER

Connect a container to a network

Options:
      --alias strings           Add network-scoped alias for the container
      --driver-opt strings      driver options for the network
      --ip string               IPv4 address (e.g., 172.30.100.104)
      --ip6 string              IPv6 address (e.g., 2001:db8::33)
      --link list               Add link to another container
      --link-local-ip strings   Add a link-local address for the container
```

```shell
docker network connect bridge bbox05
```

通过docker insepct 容器名称|ID 再次查看容器的网络信息，多增加了默认的Bridge。



#### 6.2.3 断开网络

通过docker network disconnect 网络名称 容器名称 命令断开网络

```shell
docker network disconnect custom_network bbox05
```

通过docker inspect 容器名称|ID 再次查看容器的网络信息，发现只剩下默认的bridge



#### 6.2.4 移除网络

可以通过docker network rm 网络名称  命令来移除自定义网络模式，网络模式移除成功后会返回网络模式名称。

```shel
docker network rm custom_network
```

**注意：如果通过某个自定义网络模式创建了容器，则该网络模式无法删除**



### 6.3 容器网络间通信

接下来我们要通过所学的知识实现容器间的网络通信。首先明确一点，容器之间要相互通信，必须要有属于同一个网络的网卡。我们先创建两个基于默认的bridge网络模式的容器

[参考文章](https://www.cnblogs.com/kevingrace/p/6590319.html)





1、创建一个自定义网络： 

```shell
$ docker network create my-net
```

可以指定子网、IP地址范围、网关等网络配置，更多请查看： docker network create ，移除自定义网络：

```shell
$ docker network rm my-net
```

移除自定义网络前先移除该网络上的所有容器

 

2、连接容器到自定义网络：

```shell
$ docker create --name my-nginx \
  --network my-net \
  --publish 8080:80 \
  nginx:latest
```

​    上述命令依据nginx镜像实例化一个容器，通过--network加入到用户自定义网络my-net中，同时发布了端口80到本地主机的8080端口，可以使用docker       network connect指令将运行的容器加入到对应的网络：

```shell
$ docker network connect my-net my-nginx
```

3、离开用户自定义网络：

 使用docker network disconnect命令

```shell
$ docker network disconnect my-net my-nginx
```

 4、使用IPv6

如果想要在容器中使用IPv6，首先要更改docker系统进程的配置，使其支持IPv6，请看这里： [enable the option](https://docs.docker.com/config/daemon/ipv6/)

在创建自定义网络时，通过--ipv6参数指定开启IPv6



5、启用从Docker容器到外部世界的转发

默认情况下，连接到默认桥接网络的容器的数据报不会被转发到外部。要启用转发，需要更改两个设置。这些不是Docker命令，它们会影响Docker主机的内核。

1、设置linux内核允许IP转发：

```shell
$ sysctl net.ipv4.conf.all.forwarding=1
```

2、将iptables FORWARD 的值更从DROP更改为ACCEPT：

```shel
$ sudo iptables -P FORWARD ACCEPT
```

这几个配置不会持久化，每次重启都需要重新配置，所以需要将它们添加到start-up script

文档中并未介绍用户自定义网络能否转发数据报到外部，我觉得默认情况下也是不能的，也需要做出上述配置

6、使用默认桥接网络

要点：

1、将容器连接到默认桥接网络：运行docker run指令时，未指定--network参数，则连接到默认桥接网络

2、配置默认桥接网络的方式：更改/etc/docker/daemon.json的内容即可，文档给出的例子如下：

```json
{
  "bip": "192.168.1.5/24",
  "fixed-cidr": "192.168.1.5/25",
  "fixed-cidr-v6": "2001:db8::/64",
  "mtu": 1500,
  "default-gateway": "10.20.1.1",
  "default-gateway-v6": "2001:db8:abcd::89",
  "dns": ["10.20.1.2","10.20.1.3"]
}
```

  3、如果我们开启了docker系统进程的IPv6支持，默认桥接网络也会默认开启IPv6支持，而且不能关闭，自定义网络可以通过是否指定--ipv6来决定是否开启IPv6    




## 7 Docker 搭建Redis Cluster

使用Docker搭建Redis Cluster，最重要的环节就是容器间通信的问题，我们来练习使用多个容器完成Redis Cluster环境的搭建，顺便为学习Docker Compose铺铺路。俗话说没有对比就没有伤害，通过对比才能感受到Docker Compose的好处。

关于Redis Cluster集群更多的内容请阅读《最通俗易懂的Redis架构模式详解》。

按照Redis官网：https://redis.io/topics/cluster-tutorial的提示，为了使Docker与Redis Cluster兼容，您需要使用Docker的host网络模式。

host网络模式需要在创建容器时通过参数--net host或者--network host指定，host网络模式可以让容器共享宿主机网络栈，容器将不会虚拟出自己的网卡，配置自己的IP等，而是使用宿主机的IP的端口。

```markdown
Redis Cluster and Docker
Currently Redis Cluster does not support NATted environments and in general environments where IP addresses or TCP ports are remapped.
Docker uses a technique called port mapping: programs running inside Docker containers may be exposed with a different port compared to the one the program believes to be using. This is useful in order to run multiple containers using the same ports, at the same time, in the same server.
In order to make Docker compatible with Redis Cluster **you need to use the host networking mode of Docker**. Please check the --net=host option in the Docker documentation for more information.
```



### 7.1 环境

为了让环境更加真实，本文使用多机环境

```markdown
192.168.10.10
192.168.10.11
```

每台机器所使用的的基础设置环境如下

```markdown
Centos 7.8.2003
Docker version 19.03.12
```



### 7.2 搭建

整体搭建步骤主要分为以下几步：

1. 下载Redis镜像
2. 编写Redis配置文件
3. 创建Redis容器
4. 创建Redis Cluster

#### 7.2.1 创建目录及文件

分别在192.168.10.10和192.168.10.11两台机器上执行以下操作

```shell
#创建目录
mkdir -p /usr/local/docker-redis/redis-cluster
#切换至指定目录
cd /usr/local/docker-redis/redis-cluster/
#编写redis-cluster.tmpl文件
vi redis-cluster.tmpl
```

#### 7.2.2 编写配置文件

192.168.10.10机器的redis-cluster.tmpl文件内容如下：

```bash
port ${PORT}					#端口
requirepass 1234			#密码		requirepass和masterauth要么都不加，要么都加
masterauth 1234				#主从密码
protected-mode no			#保护模式关闭
daemonize no					#是不是后台进程，容器要有前台进程
appendonly yes				#AOF文件是否开启
cluster-enable yes		#开启集群模式
cluster-config-file nodes.conf				#集群配置
cluster-node-timeout 15000						#超时时间
cluster-announce-ip 192.168.10.10			#host模式宿主机的
cluster-announce-port ${PORT}					#cluster端口
cluster-announce-bus-port 1${PORT}		#cluster消息总线的端口
```

192.168.10.11机器的redis-cluster.tmpl文件内容如下：

```bash
port ${PORT}
requirepass 1234
masterauth 1234
protected-mode no
daemonize no
appendonly yes
cluster-enable yes
cluster-config-file nodes.conf
cluster-node-timeout 15000
cluster-announce-ip 192.168.10.11
cluster-announce-port ${PORT}
cluster-announce-bus-port 1${PORT}
```

```markdown
port：节点端口
requirepass：添加访问认证
masterauth：如果主节点开启了访问认证，从节点访问主节点也需要认证
protect-mode：保护模式，默认为yes，即开启。开启保护模式以后，需要配置bind ip或者设置访问密码；关闭保护模式，外部网络可以直接访问
daemonize：是否已守护线程的方式启动（后台启动），默认no
appendonly：是否开启AOF持久化模式，默认no
cluster-enable：是否开启集群模式，默认no
cluster-config-file：集群节点信息文件
cluster-node-timeout：集群节点连接超时时间
cluster-announce-ip：集群节点IP，填写宿主机的IP
cluster-announce-port：集群节点映射端口
cluster-announce-bus-port：集群节点总线端口
```

```markdown
每个Redis集群节点都需要打开两个TCP连接。一个用于为客户端提供服务的正常Redis TCP端口，例如：6379.还有一个基于6379端口加10000的端口，比如：16379。
第二个端口用于集群总线，这是一个使用二进制协议的节点到节点通信信道。节点使用集群总线进行故障检测、配置更新、故障转移授权等等。客户端永远不要尝试与几圈总线端口通信，与正常的Redis命令端口通信即可，但是请确保防火墙中的这连个端口都已经打开，否则Redis集群节点将无法通信。
```

在192.168.10.10机器的redis-cluster目录下执行以下命令：

```shell
for port in `seq 6371 6373`; do \
mkdir -p ${port}/conf \
&& PORT=${port} envsubst < redis-cluster.tmpl > ${port}/conf/redis/conf \
&& mkdir -p ${port}/data; \
done
```

在192.168.10.11机器的redis-cluster目录下执行以下命令：

```shell
for port `seq 6374 6376`; do \
mkdir -p ${port}/conf \
&& PORT=${port} envsubst < redis-cluster.tmpl > ${port}/conf/redis.conf \
&& mkdir -p ${port}/data; \
done
```

上面两端shell for语句，意思是循环创建6371~6376相关的目录及文件

执行完之后，可以通过命令查看配置文件信息

```shell
#192.168.10.10
cat /usr/local/docker-redis/redis-cluster/637{1..3}/conf/redis.conf
#192.168.10.11
cat /usr/local/docker-redis/redis-cluster/637{4..6}/conf/redis.conf
```

#### 7.2.3 创建redis容器

##### 7.2.3.1 创建容器

将宿主机的6371~6376之间的端口与6个redis容器映射，并将宿主机的目录与容器内的目录进行映射(目录挂载)。

记得指定网络模式，使用host网络模式

在192.168.10.10机器执行以下命令

```shell
for port in $(seq 6371 6373);do \
docker run -di --restart always --name redis-${port} --net host \
-v /usr/local/docker-redis/redis-cluster/${port}/conf/redis.conf:/usr/local/etc/redis/redis.conf \
-v /usr/local/docker-redis/redis-cluster/${port}/data:/data \
redis redis-server /usr/local/etc/redis/redis.conf; \
done
```

在192.168.10.11机器执行以下命令

```shell
for port in $(seq 6374 6376); do \
docker run -di --restart always --name redis-${port} --net host \
-v /usr/local/docker-redis/redis-cluster/${port}/conf/redis.conf:/usr/loca/etc/redis/redis.conf \
-v /usr/local/docker-redis/redis-cluster/${port}/data:/data \
redis redis-server /usr/local/etc/redis/redis.conf; \
done
```

--restart always:docker启动了，容器就启动

在192.168.10.10|11机器执行docker ps -n 3查看容器是否创建成功



#### 7.2.4 创建Redis Cluster集群

随便进入一个容器节点，并进入/usr/local/bin目录：

```shell
#进入容器
docker exec -it redis-6379 bash
#切换至指定目录
cd /usr/local/bin
```

接下来我们就可以通过以下命令实现Redis Cluster集群的创建，三主三从的，这个可能会失败

那么先关掉防火墙

```shell
systemctl disable firewalld
systenctl stop firewalld
```



```shell
redis-cli -a 1234 --cluster create 192.168.10.10:6371 192.168.10.10:6372 192.168.10.10:6373 192.168.10.11:6374 192.168.10.11:6375 192.168.10.11:6376 --cluster-replicas 1
```

出现选择提示信息，输入yes



### 7.3 查看集群状态

我们先进入容器，然后通过一些集群常用的命令查看一下集群的状态。

```shell
#进入容器
docker exec -it redis-6371 bash
#切换至指定目录
cd /usr/local/bin/
```

#### 7.3.1 检查集群状态

```shell
redis-cli -a 1234 --cluster 192.168.10.11:6375
```

#### 7.3.2 查看集群信息和节点信息

```shell
#连接至集群的某个节点
redis-cli -c -a 1234 -h 192.168.10.11 -p 6376
#查看集群信息
cluster info
#查看集群节点信息
cluster nodes
```

#### 7.3.3 SET/GET

在6371节点中执行写入和读取，命令如下：

```shell
#进入容器并连接至集群某个节点
docker exec -it redis-6371 /usr/local/bin/redis-cli -c -a 1234 -h 192.168.10.10 -p 6371
或者进入/usr/local/docker-redis/redis-cluster执行redis-cli -c -a 1234 -h 192.168.10.10 -p 6371
#写入数据
set name sennerming
set aaa 111
set bbb 222
#读取数据
get name 
get aaa
get bbb
```



## 8 Compse

Dockerfile文件让用户很方便的定义一个单独的应用容器。然而，在日常工作中，经常会碰到需要多个容器相互配合来完成某项任务的情况，例如之前搭建Redis Cluster，或者开发一个web应用，除了web服务容器本身，还需要数据库服务器容器、缓存容器，甚至还包括负载均衡容器等等。

Docker Compose恰好满足了这样的需求，他是用于定义和运行多容器Docker应用程序的工具，通过Compose，您可以使用YAML文件来配置程序所需要的服务。然后使用一个命令，就额可以通过YAML配置文件创建并启动所有服务。

Docker Compose项目是docker官方的开源项目，来源于之前的Flg项目，使用Python语言编写，负责实现对Docker容器集群的快速编排。项目地址为：https://github.com/docker/compose/releases

Docker Compose使用的三个步骤为：

1. 使用Dockerfile文件定义应用程序的环境；
2. 使用docker-compose.yml文件定义构成应用程序的服务，这样他们可以在隔离环境中一起运行；
3. 最后，执行docker-compose up命令来创建并启动所有服务

比如你有个Web项目，自己的应用、一个tomcat、一个mysql...一套东西，就可以使用Compose一个东西搞定。

### 8.1 下载及安装

官方文档：https://docs.docker.com/compose/install

您可以在macos，windows和linux运行compose，本文基于Linux环境的安装。我们可以使用curl命令从Github下载他的二级制文件来使用，运行一下命令下载Docker Compose的当前稳定版本。或者从网页下载后上传至服务器指定目录/usr/local/bin也行。

```shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-${uname -s}-${uname -m}" -o /usr/local/bin/docker-compose
```

因为Docker Compose存放在Github上，可能不太稳定，也可以通过执行下面的命令，高速安装Compose。该加速通道由DaoCloud提供：http://get.daocloud.io/#install-compose

```shell
sudo curl -L https://get.daocloud.io/docker/compose/releases/download/1.26.2/docker-compose-`uname -s` - `uname -m` > /usr/local/bin/docker-compose
```

您可以通过修改URL中的版本，自定义您所需要的版本文件

MacOS的docker desktop，其实自带了docker-compose

**授权**

安装完成以后，查看指定目录，发现改文件没有可执行权限，进行授权操作

```shell
cd /usr/local/bin/
sudo chmod +x /usr/local/bin/docker-compose
```

**测试**

```shell
docker-compose --version
```

[参考文章](https://blog.csdn.net/xinquanv1/article/details/109035196)

### 8.2 docker-compose.yaml文件详解

#### 8.2.1 概念

官方文档：https://docs.docker.com/compose/compose-file/

Docker Compose允许用户通过docker-compose.yml文件(YAML格式)来定义一组相关联的容器为一个工程(project)。一个工程包含过个服务(service)，每个服务中定义了创建容器时所需的镜像、参数、依赖等。

```markdown
工程名若无特殊指定，即为docker-compose.yml文件所在目录的名称
```

Docker Compose模板文件我们需要关注的顶级配置有version、services、networks、volumes几个部分，除了version之外，其他几个顶级配置下还有很多下级配置，后面也会详细给大家介绍，先来看看这几个顶级配置都什么意思：

1. version：描述Compose文件的版本信息，当前最新版本为3.8，对应的Docker版本为19.03.0+；
2. services：定义服务，可以多个，每个服务中定义了创建容器时所需要的镜像、参数、依赖等等；
3. networks：定义网络，可以多个，根据DNS server让相同网络中的容器可以直接通过容器名称进行通信；
4. volumes：数据卷，用于实现目录挂载

#### 8.2.2 案例

在配置文件中，所有的容器通过services来定义，然后使用docker-compose脚本来启动、停止和重启容器，非常适合多个容器组合使用进行开发的场景。我们先从一个简单的Compose案例学起。

编写docker-compose.yml文件

```shell
#创建目录
mkdir -p /usr/local/docker-nginx
#切换至指定目录
cd /usr/local/docker-nginx/
#编写docker-compose.yml文件
vi docker-compose.yml
```

在文件中添加以下内容

```yaml
#描述Compose文件版本信息
version: "3.8"

#定义服务，可以多个
services: 
	nginx:  #服务名称
		image: nginx #创建容器时所需的镜像
    container_name: mynginx	#容器名称，默认为“工程名称_服务条目名称_序列号”
    ports: #宿主机与容器的端口映射关系
    	-	"80:80" #左边为宿主机端口：右边为容器端口
    networks: #配置容器连接的网络，引用顶级networks下的条目
    	-	nginx-net
#定义网络，可以多个。如果不声明，默认会创建一个网络名称为"工程名称_default"的bridge网络
networks:
	nginx-net: #一个具体网络的条目名称
		name: nginx-net #网络名称，默认为"工程名称_网络条目名称"
		driver: bridge #网络模式，默认为bridge
```

使用docker-compose up 创建并启动所有服务，这个简单的案例中就只有一个Nginx后续我们会做一些复杂的：

```shell
#前台启动
docker-compose up
#后台启动
docker-compose up -d
```

up完之后，访问http://127.0.0.1，就能看到nginx了

可以使用docker-compose down可以停止并删除容器、网络

Docker Compose命令[参考地址](https://www.cnblogs.com/mrhelloworld/p/docker13.html)

### 8.3 Compose搭建Redis Cluster

这次使用Docker Compose再次进行搭建

按照Redis官网：https://redis.io/topics/cluster-tutorial的提示，为了使Docker与Redis Cluster兼容，需要使用Docker的host网络模式

host网络模式需要在创建容器时通过参数--net host或者--network host指定，host网络模式可以让容器共享宿主机网络栈，容器将不会虚拟出自己的网卡，配置自己的IP等，而是使用宿主机的IP和端口

关于Docker网络模式跟过的内容请阅读《Docker网络模式详解及容器间通信》

之前的配置都一样的，这次要编写一个Docker Compose模板文件

在192.68.10.10机器的/usr/local/docker-redis目录下创建docker-compose.yml文件并编辑

```yaml
#描述Compose文件的版本信息
version: "3.8"

#定义服务，可以多个
services:
	redis-6371: #服务名称
		image: redis #创建容器时所需的镜像
		container_name: redis-6371 #容器名称
		restart: always #容器总是重新启动
		network_mode: "host" #host网络模式
		volumes: #数据卷，目录挂载
			- /usr/local/docker-redis/redis-cluster/6371/conf/redis.conf:/usr/local/etc/redis/redis.conf
			- /usr/local/docker-redis/redis-cluster/6371/data:/data
		command: redis-server /usr/local/etc/redis/redis.conf #覆盖容器启动后默认执行的命令
	redis-6372:
		image: redis
		container_name: redis-6372
		network_mode: "host"
		volumes:
			-	/usr/local/docker-redis/redis-cluster/6372/conf/redis.conf:/usr/local/etc/redis/redis.conf
			- /usr/local/docker-redis/redis-cluster/6372/data:/data
		command: redis-server /usr/local/etc/redis/redis.conf
	redis-6373:
		image: redis
		container_name: redis-6373
		network_mode: "host"
		volumes:
		 - /usr/local/docker-redis/redis-cluster/6373/conf/redis.conf:/usr/local/etc/redis/redis.conf
		 - /usr/local/docker-redis/redis-cluster/6373/data:/data
		command: redis-server /usr/local/etc/redis/redis.conf
	
```

192.168.10.11的机器相同目录下/usr/local/docker-redis目录下创建docker-compose.yml文件并编辑

```yaml
#描述Compose文件的版本信息
version: "3.8"

#定义服务，可以多个
services:
	redis-6374:
		image: redis
		container_name: redis-6374
		network_mode: "host"
		volumes:
		 - /usr/local/docker-redis/redis-cluster/6374/conf/redis.conf:/usr/local/etc/redis/redis.conf
		 - /usr/local/docker-redis/redis-cluster/6374/data:/data
		command: redis-server /usr/local/etc/redis/redis.conf
	 redis-6375:
		image: redis
		container_name: redis-6375
		network_mode: "host"
		volumes:
		 - /usr/local/docker-redis/redis-cluster/6375/conf/redis.conf:/usr/local/etc/redis/redis.conf
		 - /usr/local/docker-redis/redis-cluster/6375/data:/data
		command: redis-server /usr/local/etc/redis/redis.conf
	redis-6376:
		image: redis
		container_name: redis-6376
		network_mode: "host"
		volumes:
		 - /usr/local/docker-redis/redis-cluster/6376/conf/redis.conf:/usr/local/etc/redis/redis.conf
		 - /usr/local/docker-redis/redis-cluster/6376/data:/data
		command: redis-server /usr/local/etc/redis/redis.conf
```

分别在docker-compose.yml目录下运行

```shell
docker-compose up -d
```

随便进入一个redis container

```shell
docker exec -it redis-6371 bash

cd /usr/local/bin/

ls #里面就会有 redis-cli、redis-server....
```

关闭

```shell
docker-compose down #就关闭了，很容易
```

## 9 Swarm

Docker swarm是Docker官方退出的容器集群管理工具，基于Go语言实现。代码开源在：https://github.com/docker/swarm使用它可以将多个Docker主机封装为单个大型的虚拟Docker主机快速打造一套容器云平台。

Docker swarm是生产环境中运行Docker应用程序最简单的方法。作为容器集群管理器，Swarm最大的优势之一就是100%支持标准的Docker API。各种基于标准API的工具比如Compose、docker-py、各种管理软件，甚至是Docker本身等都可以很容易的与Swarm进行集成。大大方便了用户将原先基于单节点的系统一直到Swarm上，同时Swarm内置了对Docker网络插件的支持，用户可以很容易的部署跨主机的容器集群服务。

Docker Swarm和Docker Compose一样，都是Docker官方容器编排工具，但不同的是，Docker Compose是一个在单个服务器或主机上擦行间多个容器的工具，而Docker Swarm则可以在多个服务器或主机上创建容器集群服务，对于微服务的部署，显然Docker Swarm会更加合适。



Swarm是Docker公司推出的用来管理docker集群的平台，几乎全部用GO语言来完成的开发的，代码开源在https://github.com/docker/swarm， 它是将一群Docker宿主机变成一个单一的虚拟主机，Swarm使用标准的Docker API接口作为其前端的访问入口，换言之，各种形式的Docker

Client(compose,docker-py等)均可以直接与Swarm通信，甚至Docker本身都可以很容易的与Swarm集成，这大大方便了用户将原本基于单节点的系统移植到Swarm上，同时Swarm内置了对Docker网络插件的支持，用户也很容易的部署跨主机的容器集群服务。

　　Docker Swarm 和 Docker Compose 一样，都是 Docker 官方容器编排项目，但不同的是，Docker Compose 是一个在单个服务器或主机上创建多个容器的工具，而 Docker Swarm 则可以在多个服务器或主机上创建容器集群服务，对于微服务的部署，显然 Docker Swarm 会更加适合。

从 Docker 1.12.0 版本开始，Docker Swarm 已经包含在 Docker 引擎中（docker swarm），并且已经内置了服务发现工具，我们就不需要像之前一样，再配置 Etcd 或者 Consul 来进行服务发现配置了。

　　Swarm deamon只是一个调度器(Scheduler)加路由器(router),Swarm自己不运行容器，它只是接受Docker客户端发来的请求，调度适合的节点来运行容器，这就意味着，即使Swarm由于某些原因挂掉了，集群中的节点也会照常运行，放Swarm重新恢复运行之后，他会收集重建集群信息。

这个工具能实现我们常说的弹性扩容，规模更大了要上K8S了

### 9.1 核心概念

![swarm-architecture](/Users/liuxiangren/docker-learning/swarm-architecture.png)

在结构图可以看出 Docker Client使用Swarm对 集群(Cluster)进行调度使用。

上图可以看出，Swarm是典型的master-slave结构，通过发现服务来选举manager。manager是中心管理节点，各个node上运行agent接受manager的统一管理，集群会自动通过Raft协议分布式选举出manager节点，无需额外的发现服务支持，避免了单点的瓶颈问题，同时也内置了DNS的负载均衡和对外部负载均衡机制的集成支持。



Docker Engine 1.12引入了Swarm模式，一个Swarm由多个Docker主机组成，他们以Swarm集群模式运行。swarm集群由Manager节点(管理者角色，管理成员和委托任务)和Worker节点(工作者角色，运行Swarm服务)组成。这些Docker主机有些是Manager节点，有些是Worker节点，或者同时扮演这两种角色。

Swarm创建服务时，需要指定要使用的镜像、在运行的容器中执行的命令、定义其副本的数量、可用的网络和数据卷、将服务公开给外部的端口等等。与独立容器相比，集群服务的主要优势之一是，你可以修改服务的配置，包括它所连接的网络和数据卷等，而不需要手动重启服务。还有就是，如果一个Worker Node不可用了，Docker会调度不可用Node的Task任务到其他Nodes上。

#### 9.1.1 Swarm的集合关键概念

```markdown
1.Swarm
集群的管理和编排是使用嵌入docker引擎的SwarmKit，可以在docker初始化时启动swarm模式或者加入已存在的swarm
 
2.Node
一个节点是docker引擎集群的一个实例。您还可以将其视为Docker节点。您可以在单个物理计算机或云服务器上运行一个或多个节点，但生产群集部署通常包括分布在多个物理和云计算机上的Docker节点。
要将应用程序部署到swarm，请将服务定义提交给 管理器节点。管理器节点将称为任务的工作单元分派 给工作节点。
Manager节点还执行维护所需群集状态所需的编排和集群管理功能。Manager节点选择单个领导者来执行编排任务。
工作节点接收并执行从管理器节点分派的任务。默认情况下，管理器节点还将服务作为工作节点运行，但您可以将它们配置为仅运行管理器任务并且是仅管理器节点。代理程序在每个工作程序节点上运行，并报告分配给它的任务。工作节点向管理器节点通知其分配的任务的当前状态，以便管理器可以维持每个工作者的期望状态。
 
3.Service
一个服务是任务的定义，管理机或工作节点上执行。它是群体系统的中心结构，是用户与群体交互的主要根源。创建服务时，你需要指定要使用的容器镜像。
 
4.Task
任务是在docekr容器中执行的命令，Manager节点根据指定数量的任务副本分配任务给worker节点
 
------------------------------------------使用方法-------------------------------------
docker swarm：集群管理，子命令有init, join, leave, update。（docker swarm --help查看帮助）
docker service：服务创建，子命令有create, inspect, update, remove, tasks。（docker service--help查看帮助）
docker node：节点管理，子命令有accept, promote, demote, inspect, update, tasks, ls, rm。（docker node --help查看帮助）
   
node是加入到swarm集群中的一个docker引擎实体，可以在一台物理机上运行多个node，node分为：
manager nodes，也就是管理节点
worker nodes，也就是工作节点
 
1）manager node管理节点：执行集群的管理功能，维护集群的状态，选举一个leader节点去执行调度任务。
2）worker node工作节点：接收和执行任务。参与容器集群负载调度，仅用于承载task。
3）service服务：一个服务是工作节点上执行任务的定义。创建一个服务，指定了容器所使用的镜像和容器运行的命令。
   service是运行在worker nodes上的task的描述，service的描述包括使用哪个docker 镜像，以及在使用该镜像的容器中执行什么命令。
4）task任务：一个任务包含了一个容器及其运行的命令。task是service的执行实体，task启动docker容器并在容器中执行任务
```



#### 9.1.2 Nodes

Swarm集群由Manager节点(管理者角色，管理成员和委托任务)和Worker节点(工作者角色，运行Swarm服务)组成。一个节点就是Swarm集群中的一个实例，也就是一个Docker主机。你可以运行一个或者多个节点在单台物理机或者云服务器上，但是生产环境上，经典的部署方式就是：Docker节点交叉分布式部署在多态物理气或者云主机上。节点名称默认为机器的hostname。

- Manager：负责整个集群的管理工作包括集群配置、服务管理、容器编排等所有跟集群有关的工作，他会选举出一个leader来指挥编排任务；
- Worker：工作节点接收和执行从管理节点分派的任务(Tasks)运行在相应的服务(Services)上

![swarm-node](/Users/liuxiangren/docker-learning/swarm-node.png)

[参考文章](https://www.cnblogs.com/zhujingzhi/p/9792432.html)

Raft和Paxos算法

#### 9.1.3 Service

服务是一个抽象的概念，是对要在管理节点或者工作节点上执行的任务的定义。他是集群系统的中心结构，是用户与集群交互的主要手段。Swarm创建服务时，可以为服务定义一下信息：

- 服务名称；
- 使用哪个镜像来创建容器；
- 要运行多个副本；
- 服务的容器要连接到哪个网络上；
- 要映射哪些端口

![swarm-service](/Users/liuxiangren/docker-learning/swarm-service.png)

任务包括一个Docker容器和在容器中运行的命令。任务是一个集群的最小单元，任务与容器是一对一的关系。管理节点根据服务规模中设置的副本数量将任务分配给工作节点。一旦任务被分配到一个节点，便无法移动到另一个几点。它只能在分配的节点上运行或失败。



#### 9.1.4 Scheduler

![swarm-scheduler](/Users/liuxiangren/docker-learning/swarm-scheduler.png)

#### 9.1.5 服务副本与全局服务

Swarm不只是提供了优秀的高可用性，同时也提供了节点的弹性扩容和伸缩的功能，可以通过以下两种类型的Services部署实现：

- Replicated Service：当服务需要动态扩缩容时，只需要通scale参数或者--replicas n 参数指定运行相同任务的数量，即可复制出新的副本，将一些列复制任务分发至各个节点中，这种操作便称为副本服务（Replicate）
- Global Services：我们也可以通过--mode global 承诺书将服务分发至全部节点之上，这种操作我们称之为全局服务（Global）。在每个节点上运行一个相同的任务，不需要预先指定任务的数量，没增加一个几点到Swarm中，协调器就会创建一个任务，然后调度器把任务分配给新的节点。

下图用黄色表示拥有三个副本服务Replicated Service，用灰色表示拥有一个全局服务Global Service。

![swarm-replicate-global-service](/Users/liuxiangren/docker-learning/swarm-replicate-global-service.png)



### 9.2 Overlay网络

Docker Swarm集群模式默认使用是Overlay网络(覆盖网络)，这里简单介绍下什么是Overlay网络。

Overlay网络指构建在另一个网络上的计算机网络，这是一种网络虚拟化技术的形式，近年来云计算虚拟化技术的演进促进了网络虚拟化技术的应用。所以Overlay网络就是建立在另一个计算机网络之上的虚拟网络，他是不能独立出现的，Overlay底层依赖的网络就是Underlay网络。

Underlay网络是专门用来承载用户IP流量的基础架构层，他与Overlay网络之间的关系有点类似物理机和虚拟机。Underlay网络和物理机都是真正存在的实体，他们分别对应着真实存在的网络设备和计算设备，而Overlay网络和虚拟机都是依托在下层实体的基础上，使用软件虚拟出来的层级。

```markdown
Overlay 	--->	虚拟机
Underlay	--->	物理机
```

在Docker版本1.12以后Swarm模式原生已支持覆盖网络（Overlay Network），只要是这个覆盖网络内的容器，**不管在不在同一个宿主机上都能互相通信，即跨主机通信**。不同覆盖网络内的容器之间是相互隔离的（相互ping不同）。

Overlay网络是目前主流的容器跨节点数据传输和路由方案。当然，容器在跨主机进行通信的时候，除了可以使用Overlay网络模式进行通信之外，还可以使用host网络模式，直接使用物理机的IP地址就可以进行通信。

https://docs.docker.com/engine/swarm/













## 命令参考

[Docker 命令中文参考地址](https://www.runoob.com/docker/docker-history-command.html)



### 1.1 -C

```markdown
-c ：建立一个压缩文件的参数指令(create 的意思)，在当前目录下将文件解压缩到其他目录
例如：$ tar -xvf file2.tar -C /home/usr2

首先介绍一个名词“控制台（console）”，它就是我们通常见到的使用字符操作界面的人机接口，例如dos。我们说控制台命令，就是指通过字符界面输入的可以操作系统的命令，例如dos命令就是控制台命令。

我们现在要了解的是基于Linux操作系统的基本控制台命令。有一点一定要注意，和dos命令不同的是，Linux的命令（也包括文件名等等）对大小写是敏感的，也就是说，如果你输入的命令大小写不对的话，系统是不会做出你期望的响应的。
```

**扩展资料**：

-x ：解开一个压缩文件的参数指令！

-t ：查看 tarfile 里面的文件！

特别注意，在参数的下达中， c/x/t 仅能存在一个！不可同时存在！

因为不可能同时压缩与解压缩。

-z ：是否同时具有 gzip 的属性？亦即是否需要用 gzip 压缩？

-j ：是否同时具有 bzip2 的属性？亦即是否需要用 bzip2 压缩？

-v ：压缩的过程中显示文件！这个常用，但不建议用在背景执行过程！



### 1.2 -p

```markdown
mkdir -p参数是能直接创建一个不存在的目录下的子目录

如，创建A目录下的B目录时 正常是使用mkdir A ，然后cd A， mkdir B

如果使用mkdir -p时，可以直接输入，mkdir -p A/B

同样，rm -r 参数是能够直接删除一个非空目录，与mkdir -p 同理是用的递归函数
```



### 1.3 -rf

```markdown
“rm -rf”的意义

　　-r或-R：递归处理，将指定目录下的所有文件与子目录一并处理；

　　-f：强制删除文件或目录；

　　由于rm命令只能删除空的目录，因此当我们需要删除一个非空目录时可以使用“rm -rf”命令，通过递归的方式先将该目录中的文件删除使其成为一个空的目录后再将其删除，从而达到删除一个非空目录的效果。
```



### 1.4 curl

curl 是常用的命令行工具，用来请求 Web 服务器。它的名字就是客户端（client）的 URL 工具的意思。

它的功能非常强大，命令行参数多达几十种。如果熟练的话，完全可以取代 Postman 这一类的图形界面工具。

不带有任何参数时，curl 就是发出 GET 请求。

```shell
$ curl https://www.example.com
上面命令向www.example.com发出 GET 请求，服务器返回的内容会在命令行输出。
```

阮一峰的curl[用法指南](http://www.ruanyifeng.com/blog/2019/09/curl-reference.html)

### 1.5 docker history nginx

**docker history :** 查看指定镜像的创建历史。

**语法**

```
docker history [OPTIONS] IMAGE
```

OPTIONS说明：

- **-H :**以可读的格式打印镜像大小和日期，默认为true；

- **--no-trunc :**显示完整的提交记录；

- **-q :**仅列出提交记录ID。

  

**实例**

查看本地镜像runoob/ubuntu:v3的创建历史。

```shell
root@runoob:~# docker history runoob/ubuntu:v3
IMAGE             CREATED           CREATED BY                                      SIZE      COMMENT
4e3b13c8a266      3 months ago      /bin/sh -c #(nop) CMD ["/bin/bash"]             0 B                 
<missing>         3 months ago      /bin/sh -c sed -i 's/^#\s*\(deb.*universe\)$/   1.863 kB            
<missing>         3 months ago      /bin/sh -c set -xe   && echo '#!/bin/sh' > /u   701 B               
<missing>         3 months ago      /bin/sh -c #(nop) ADD file:43cb048516c6b80f22   136.3 MB
```

### 1.6 docker attach 

**docker attach :**连接到正在运行中的容器。

**语法**

```
docker attach [OPTIONS] CONTAINER
```

要attach上去的容器必须正在运行，可以同时连接上同一个container来共享屏幕（与screen命令的attach类似）。

官方文档中说attach后可以通过CTRL-C来detach，但实际上经过我的测试，如果container当前在运行bash，CTRL-C自然是当前行的输入，没有退出；如果container当前正在前台运行进程，如输出nginx的access.log日志，CTRL-C不仅会导致退出容器，而且还stop了。这不是我们想要的，detach的意思按理应该是脱离容器终端，但容器依然运行。好在attach是可以带上--sig-proxy=false来确保CTRL-D或CTRL-C不会关闭容器。

**实例**

容器mynginx将访问日志指到标准输出，连接到容器查看访问信息。

```shell
runoob@runoob:~$ docker attach --sig-proxy=false mynginx
192.168.239.1 - - [10/Jul/2016:16:54:26 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.93 Safari/537.36" "-"
```



### 1.7 Systemctl

[Systemd 入门教程：命令篇](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)



### 1.8 tree

tree /usr/local/

可以格式化目录结构进行展示



## 国内Docker Hub

[地址](http://hub.daocloud.io)

