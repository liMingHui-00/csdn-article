> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_21197507/article/details/115071715?spm=1001.2014.3001.5506)

### 笔记来源于

1.  Docker [https://www.bilibili.com/video/BV1og4y1q7M4](https://www.bilibili.com/video/BV1og4y1q7M4) 视频整理

### 一.Docker 入门

1. Docker 为什么会出现
----------------

![](https://i-blog.csdnimg.cn/blog_migrate/9a254d75facbbba2f4b38082675819ca.png#pic_center)

![](https://i-blog.csdnimg.cn/blog_migrate/c76811457bcbd282164140f80aca1404.png#pic_center)

2. Docker 的历史
-------------

![](https://i-blog.csdnimg.cn/blog_migrate/042a2a08df33186b8329e77c790d310e.png#pic_center)

3. [Docker 最新超详细版教程通俗易懂](https://www.bilibili.com/video/BV1og4y1q7M4)
---------------------------------------------------------------------

![](https://i-blog.csdnimg.cn/blog_migrate/9c1319c4959998c4640086f8ac169ef0.png)

*   Docker 是基于 Go 语言开发的！开源项目
*   [官网](https://www.docker.com/)
*   [官方文档](https://docs.docker.com/) **Docker 文档是超详细的**
*   [仓库地址](https://hub.docker.com/)

4. 虚拟化技术和容器化技术对比
----------------

### 4.1. 虚拟化技术的缺点

*   资源占用十分多
*   冗余步骤多
*   启动很慢

![](https://i-blog.csdnimg.cn/blog_migrate/7eb113eb1ed9cc907df7315bf90c533f.png)

### 2.2. 容器化技术

![](https://i-blog.csdnimg.cn/blog_migrate/adcfb944932bd422b28408162e221515.png)

*   比较 Docker 和虚拟化技术的不同
    *   传统虚拟机， 虚拟出一条硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件
    *   容器内的应用直接运行在宿主机的内部，容器是没有自己的内核的，也没有虚拟硬件，所以轻便
    *   每个容器间是相互隔离的，每个容器内都有一个属于自己的文件系统，互不影响
*   应用更快速的交互和部署
    *   传统：一堆帮助文档，安装程序
    *   Docker： 打包镜像发布测试，一键运行
*   更便捷的升级和扩缩容
*   更简的系统运维
*   更高效的计算资源利用

### 4.3. DevOps

![](https://i-blog.csdnimg.cn/blog_migrate/7abbd746cbd3a8d298d4c4b95153f188.png#pic_center)

3. 名词解释
-------

*   镜像（image）
    *   Docker 镜像就好比是一个模板，可以通过这个模板来创建容器服务，tomcat 镜像 ===> run ===> tomcat01 容器， 通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中的）
*   容器（container）
    *   Docker 利用容器技术，独立运行一个或者一组应用， 通过镜像来创建的
    *   启动，停止，删除，基本命令！
    *   就目前可以把这个容器理解为一个建议的 linux 系统
*   仓库（repository）
    *   存放镜像的地方
    *   Docker Hub（默认是国外的）
    *   阿里云,,, 都有容器服务（配置镜像加速！）

4. 阿里云镜像加速
----------

1.  登录阿里云服务器，找到`容器镜像服务`
2.  设置 Registry 登录密码
3.  找到镜像加速器
4.  配置使用

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://pi9dpp60.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

4.2 安装 docker
-------------

#### 卸载旧版本

较旧的 Docker 版本称为 docker 或 docker-engine 。如果已安装这些程序，请卸载它们以及相关的依赖项。

$ sudo yum remove docker \  
                  docker-client \  
                  docker-client-latest \  
                  docker-common \  
                  docker-latest \  
                  docker-latest-logrotate \  
                  docker-logrotate \  
                  docker-engine

#### 安装 Docker Engine-Community

#### 使用 Docker 仓库进行安装

在新主机上首次安装 Docker Engine-Community 之前，需要设置 Docker 仓库。之后，您可以从仓库安装和更新 Docker。

**设置仓库**

安装所需的软件包。yum-utils 提供了 yum-config-manager ，并且 device mapper 存储驱动程序需要 device-mapper-persistent-data 和 lvm2。

$ sudo yum install -y yum-utils \  
  device-mapper-persistent-data \  
  lvm2

使用以下命令来设置稳定的仓库。

### 使用官方源地址（比较慢）

$ sudo yum-config-manager \  
    --add-repo \  
    https://download.docker.com/linux/centos/docker-ce.repo

可以选择国内的一些源地址：

### 阿里云

$ sudo yum-config-manager \  
    --add-repo \  
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

### 清华大学源

$ sudo yum-config-manager \  
    --add-repo \  
    https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo

#### 安装 Docker Engine-Community

安装最新版本的 Docker Engine-Community 和 containerd，或者转到下一步安装特定版本：

```
$ sudo yum install docker-ce docker-ce-cli containerd.io

```

Docker 安装完默认未启动。并且已经创建好 docker 用户组，但该用户组下没有用户。

**要安装特定版本的 Docker Engine-Community，请在存储库中列出可用版本，然后选择并安装：**

1、列出并排序您存储库中可用的版本。此示例按版本号（从高到低）对结果进行排序。

$ yum list docker-ce --showduplicates | sort -r  
docker-ce.x86_64  3:18.09.1-3.el7                     docker-ce-stable  
docker-ce.x86_64  3:18.09.0-3.el7                     docker-ce-stable  
docker-ce.x86_64  18.06.1.ce-3.el7                    docker-ce-stable  
docker-ce.x86_64  18.06.0.ce-3.el7                    docker-ce-stable

2、通过其完整的软件包名称安装特定版本，该软件包名称是软件包名称（docker-ce）加上版本字符串（第二列），从第一个冒号（:）一直到第一个连字符，并用连字符（-）分隔。例如：docker-ce-18.09.1。

```
$ sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io

```

启动 Docker。

```
$ sudo systemctl start docker

```

通过运行 hello-world 映像来验证是否正确安装了 Docker Engine-Community 。

```
$ sudo docker run hello-world

```

5. 底层原理
-------

*   HelloWorld 镜像
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/7494e3ffc8a9771bcccb773ffdb169ee.png)
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/46c5f7ff68e809091198f873357bccba.png)
    

![](https://i-blog.csdnimg.cn/blog_migrate/9f41e5013bb8c52c7e3d6991f08459c2.png)

*   底层原理
    
    Docker Engine 是一个客户端 - 服务器应用程序，具有以下主要组件:
    
    *   一个服务器，它是一种长期运行的程序，称为守护进程 (dockerd 命令)
    *   一个 REST API，它指定程序可以用来与守护进程对话并指示它做什么的接口。
    
    Docker 是一个 **Client Server 结构的系统**，Docker 守护进程运行在主机上，然后通过 Socket 连接从客户 端访问，守护进程从客户端接受命令并管理运行在主机上的容器。**容器，是一个运行时环境就是我们所说的集装箱。**
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/047bae011dff536758195bd9e85eb87f.png)
    

*   为什么 Docker 比 Vm 快
    
    *   docker 有着比虚拟机更少的抽象层。__由于 docker 不需要 Hypervisor 实现硬件资源虚拟化,*_ 运行在 docker 容器上的程序直接使用的都是实际物理机的硬件资源 *_。因此在 CPU、内存利用率上 docker 将会在效率上有明显优势。**
    *   **docker 利用的是宿主机的内核, 而不需要 Guest OS**。因此, 当新建一个 容器时, docker 不需要和虚拟机一样重新加载一个操作系统内核。仍而避免引寻、加载操作系统内核返个比较费时费资源的过程, 当新建一个虚拟机时, 虚拟机软件需要加载 GuestOS, 返个新建过程是分钟级别的。**而 docker 由于直接利用宿主机的操作系统, 则省略了返个过程, 因此新建一个 docker 容器只需要几秒钟。**
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/322632f6e7caa921a72cc54ea7301857.png)
    

### 二，Docker 基本命令

1. Docker 的常用命令
---------------

![](https://i-blog.csdnimg.cn/blog_migrate/52575e34aa6632aa6e70eeca27b4785f.png)

### 帮助命令

```
docker version  # docker版本信息
docker info     # 系统级别的信息，包括镜像和容器的数量
docker 命令 --help 
```

*   [**帮助文档**](https://docs.docker.com/engine/reference/commandline/docker/)

### 镜像命令

docker images 查看所有本地主机上的镜像

```
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              bf756fb1ae65        7 months ago        13.3kB
 
# 解释
REPOSITORY      # 镜像的仓库
TAG             # 镜像的标签
IMAGE ID        # 镜像的ID
CREATED         # 镜像的创建时间
SIZE            # 镜像的大小
 
# 可选项
--all , -a      # 列出所有镜像
--quiet , -q    # 只显示镜像的id
```

docker search 查找镜像

```
NAME                              DESCRIPTION                                     STARS               OFFICIAL         AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   9822                [OK]                
mariadb                           MariaDB is a community-developed fork of MyS…   3586                [OK]                
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   719                                     [OK]
 
# 可选项
--filter=STARS=3000     # 搜素出来的镜像就是STARS大于3000的
 
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker search mysql --filter=STARS=3000
NAME                DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql               MySQL is a widely used, open-source relation…   9822                [OK]                
mariadb             MariaDB is a community-developed fork of MyS…   3586                [OK]     
```

docker pull 下拉镜像

```
# 下载镜像，docker pull 镜像名[:tag]
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker pull mysql
Using default tag: latest           # 如果不写tag，默认就是latest
latest: Pulling from library/mysql
bf5952930446: Pull complete         # 分层下载，dockerimages的核心，联合文件系统
8254623a9871: Pull complete 
938e3e06dac4: Pull complete 
ea28ebf28884: Pull complete 
f3cef38785c2: Pull complete 
894f9792565a: Pull complete 
1d8a57523420: Pull complete 
6c676912929f: Pull complete 
ff39fdb566b4: Pull complete 
fff872988aba: Pull complete 
4d34e365ae68: Pull complete 
7886ee20621e: Pull complete 
Digest: sha256:c358e72e100ab493a0304bda35e6f239db2ec8c9bb836d8a427ac34307d074ed     # 签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest      # 真实地址
 
# 等价于
docker pull mysql
docker pull docker.io/library/mysql:latest
 
# 指定版本下载
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker pull mysql:5.7
5.7: Pulling from library/mysql
bf5952930446: Already exists 
8254623a9871: Already exists 
938e3e06dac4: Already exists 
ea28ebf28884: Already exists 
f3cef38785c2: Already exists 
894f9792565a: Already exists 
1d8a57523420: Already exists 
5f09bf1d31c1: Pull complete 
1b6ff254abe7: Pull complete 
74310a0bf42d: Pull complete 
d398726627fd: Pull complete 
Digest: sha256:da58f943b94721d46e87d5de208dc07302a8b13e638cd1d24285d222376d6d84
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
 
# 查看本地镜像
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mysql               5.7                 718a6da099d8        6 days ago          448MB
mysql               latest              0d64f46acfd1        6 days ago          544MB
hello-world         latest              bf756fb1ae65        7 months ago        13.3kB
```

docker rmi 删除镜像

```
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker rmi -f IMAGE ID                        # 删除指定镜像
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker rmi -f IMAGE ID1 IMAGE ID2 IMAGE ID3   # 删除多个镜像
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]#  docker rmi -f $(docker images -aq)           # 删除所有镜像
```

### 容器命令

**说明： 我们有了镜像才可创建容器，linux，下载一个 centos 镜像来测试学习**

```
docker pull centos


```

**新建容器并启动**

```
docker run [可选参数] image
 
# 参数说明
--name=“Name”   容器名字    tomcat01    tomcat02    用来区分容器
-d      后台方式运行
-it     使用交互方式运行，进入容器查看内容
-p      指定容器的端口     -p 8080:8080
    -p  ip:主机端口：容器端口
    -p  主机端口：容器端口（常用）
    -p  容器端口
    容器端口
-p      随机指定端口
 
 
# 测试，启动并进入容器
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker run -it centos /bin/bash
[root@74e82b7980e7 /]# ls   # 查看容器内的centos，基础版本，很多命令是不完善的
bin  etc   lib    lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
 
# 从容器中退回主机
[root@77969f5dcbf9 /]# exit
exit
[root@iZ2zeg4ytp0whqtmxbsqiiZ /]# ls
bin   dev  fanfan  lib    lost+found  mnt  proc  run   srv  tmp  var
boot  etc  home    lib64  media       opt  root  sbin  sys  usr
```

**列出所有的运行的容器**

```
# docker ps 命令
        # 列出当前正在运行的容器
-a      # 列出正在运行的容器包括历史容器
-n=?    # 显示最近创建的容器
-q      # 只显示当前容器的编号
 
[root@iZ2zeg4ytp0whqtmxbsqiiZ /]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@iZ2zeg4ytp0whqtmxbsqiiZ /]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
77969f5dcbf9        centos              "/bin/bash"         5 minutes ago       Exited (0) 5 minutes ago                       xenodochial_bose
74e82b7980e7        centos              "/bin/bash"         16 minutes ago      Exited (0) 6 minutes ago                       silly_cori
a57250395804        bf756fb1ae65        "/hello"            7 hours ago         Exited (0) 7 hours ago                         elated_nash
392d674f4f18        bf756fb1ae65        "/hello"            8 hours ago         Exited (0) 8 hours ago                         distracted_mcnulty
571d1bc0e8e8        bf756fb1ae65        "/hello"            23 hours ago        Exited (0) 23 hours ago                        magical_burnell
 
[root@iZ2zeg4ytp0whqtmxbsqiiZ /]# docker ps -qa
77969f5dcbf9
74e82b7980e7
a57250395804
392d674f4f18
571d1bc0e8e8
```

**退出容器**

```
exit            # 直接退出容器并关闭
Ctrl + P + Q    # 容器不关闭退出
```

**删除容器**

```
docker rm -f 容器id                       # 删除指定容器
docker rm -f $(docker ps -aq)       # 删除所有容器
docker ps -a -q|xargs docker rm -f  # 删除所有的容器
```

**启动和停止容器的操作**

```
docker start 容器id           # 启动容器
docker restart 容器id         # 重启容器
docker stop 容器id            # 停止当前正在运行的容器
docker kill 容器id            # 强制停止当前的容器
```

### 常用的其他命令

**后台启动容器**

```
# 命令 docker run -d 镜像名
[root@iZ2zeg4ytp0whqtmxbsqiiZ /]# docker run -d centos
 
# 问题 docker ps， 发现centos停止了
 
# 常见的坑， docker 容器使用后台运行， 就必须要有一个前台进程，docker发现没有应用，就会自动停止
# nginx， 容器启动后，发现自己没有提供服务，就会立即停止，就是没有程序了
```

**查看日志**

```
docker logs -tf --tail number 容器id
 
[root@iZ2zeg4ytp0whqtmxbsqiiZ /]# docker logs -tf --tail 1 8d1621e09bff
2020-08-11T10:53:15.987702897Z [root@8d1621e09bff /]# exit      # 日志输出
 
# 自己编写一段shell脚本
[root@iZ2zeg4ytp0whqtmxbsqiiZ /]# docker run -d centos /bin/sh -c "while true;do echo xiaofan;sleep 1;done"
a0d580a21251da97bc050763cf2d5692a455c228fa2a711c3609872008e654c2
 
[root@iZ2zeg4ytp0whqtmxbsqiiZ /]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
a0d580a21251        centos              "/bin/sh -c 'while t…"   3 seconds ago       Up 1 second                             lucid_black
 
# 显示日志
-tf                 # 显示日志
--tail number       # 显示日志条数
[root@iZ2zeg4ytp0whqtmxbsqiiZ /]# docker logs -tf --tail 10 a0d580a21251
```

**查看容器中进程信息 ps**

```
# 命令 docker top 容器id
[root@iZ2zeg4ytp0whqtmxbsqiiZ /]# docker top df358bc06b17
UID                 PID                 PPID                C                   STIME               TTY     
root                28498               28482               0                   19:38               ?      
```

**查看镜像的元数据**

```
# 命令
docker inspect 容器id
 
[root@iZ2zeg4ytp0whqtmxbsqiiZ /]# docker inspect df358bc06b17
[
    {
        "Id": "df358bc06b17ef44f215d35d9f46336b28981853069a3739edfc6bd400f99bf3",
        "Created": "2020-08-11T11:38:34.935048603Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 28498,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-08-11T11:38:35.216616071Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:0d120b6ccaa8c5e149176798b3501d4dd1885f961922497cd0abef155c869566",
        "ResolvConfPath": "/var/lib/docker/containers/df358bc06b17ef44f215d35d9f46336b28981853069a3739edfc6bd400f99bf3/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/df358bc06b17ef44f215d35d9f46336b28981853069a3739edfc6bd400f99bf3/hostname",
        "HostsPath": "/var/lib/docker/containers/df358bc06b17ef44f215d35d9f46336b28981853069a3739edfc6bd400f99bf3/hosts",
        "LogPath": "/var/lib/docker/containers/df358bc06b17ef44f215d35d9f46336b28981853069a3739edfc6bd400f99bf3/df358bc06b17ef44f215d35d9f46336b28981853069a3739edfc6bd400f99bf3-json.log",
        "Name": "/hungry_heisenberg",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/5af8a2aadbdba9e1e066331ff4bce56398617710a22ef906f9ce4d58bde2d360-init/diff:/var/lib/docker/overlay2/62926d498bd9d1a6684bb2f9920fb77a2f88896098e66ef93c4b74fcb19f29b6/diff",
                "MergedDir": "/var/lib/docker/overlay2/5af8a2aadbdba9e1e066331ff4bce56398617710a22ef906f9ce4d58bde2d360/merged",
                "UpperDir": "/var/lib/docker/overlay2/5af8a2aadbdba9e1e066331ff4bce56398617710a22ef906f9ce4d58bde2d360/diff",
                "WorkDir": "/var/lib/docker/overlay2/5af8a2aadbdba9e1e066331ff4bce56398617710a22ef906f9ce4d58bde2d360/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "df358bc06b17",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20200809",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "4822f9ac2058e8415ebefbfa73f05424fe20cc8280a5720ad3708fa6e80cdb08",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/4822f9ac2058",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "5fd269c0a28227241e40cd30658e3ffe8ad6cc3e6514917c867d89d36a31d605",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "30d6017888627cb565618b1639fecf8fc97e1ae4df5a9fd5ddb046d8fb02b565",
                    "EndpointID": "5fd269c0a28227241e40cd30658e3ffe8ad6cc3e6514917c867d89d36a31d605",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
[root@iZ2zeg4ytp0whqtmxbsqiiZ /]# 
 
```

**进入当前正在运行的容器**

```
# 我们通常容器使用后台方式运行的， 需要进入容器，修改一些配置
 
# 命令
docker exec -it 容器id /bin/bash
 
# 测试
[root@iZ2zeg4ytp0whqtmxbsqiiZ /]# docker exec -it df358bc06b17 /bin/bash
[root@df358bc06b17 /]# ls       
bin  etc   lib    lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
[root@df358bc06b17 /]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 Aug11 pts/0    00:00:00 /bin/bash
root        29     0  0 01:06 pts/1    00:00:00 /bin/bash
root        43    29  0 01:06 pts/1    00:00:00 ps -ef
 
# 方式二
docker attach 容器id
 
# docker exec       # 进入容器后开启一个新的终端，可以在里面操作
# docker attach     # 进入容器正在执行的终端，不会启动新的进程
```

**从容器中拷贝文件到主机**

```
docker cp 容器id：容器内路径    目的地主机路径
 
[root@iZ2zeg4ytp0whqtmxbsqiiZ /]# docker cp 7af535f807e0:/home/Test.java /home
```

### 三，Docker 部署软件实战

1.Docker 部署软件实战
---------------

### Docker 安装 Nginx

```
# 1. 搜索镜像 search 建议去docker hub搜索，可以看到帮助文档
# 2. 下载镜像 pull
# 3. 运行测试
[root@iZ2zeg4ytp0whqtmxbsqiiZ home]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos              latest              0d120b6ccaa8        32 hours ago        215MB
nginx               latest              08393e824c32        7 days ago          132MB
 
# -d 后台运行
# -name 给容器命名
# -p 宿主机端口：容器内部端口
[root@iZ2zeg4ytp0whqtmxbsqiiZ home]# docker run -d --name nginx01 -p 3344:80 nginx  # 后台方式启动启动镜像
fe9dc33a83294b1b240b1ebb0db9cb16bda880737db2c8a5c0a512fc819850e0
[root@iZ2zeg4ytp0whqtmxbsqiiZ home]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
fe9dc33a8329        nginx               "/docker-entrypoint.…"   4 seconds ago       Up 4 seconds        0.0.0.0:3344->80/tcp   nginx01
[root@iZ2zeg4ytp0whqtmxbsqiiZ home]# curl localhost:3344    # 本地访问测试
 
# 进入容器
[root@iZ2zeg4ytp0whqtmxbsqiiZ home]# docker exec -it nginx01 /bin/bash
root@fe9dc33a8329:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
root@fe9dc33a8329:/# cd /etc/nginx/
root@fe9dc33a8329:/etc/nginx# ls
conf.d      koi-utf  mime.types  nginx.conf   uwsgi_params
fastcgi_params  koi-win  modules     scgi_params  win-utf
 
```

**端口暴露概念**

![](https://i-blog.csdnimg.cn/blog_migrate/1e251ed4d517c1aca6af58c3d2586553.png)

2. Docker 安装 Tomcat
-------------------

```
# 官方的使用
docker run -it --rm tomcat:9.0
 
# 我们之前的启动都是后台的，停止了容器之后， 容器还是可以查到，docker run -it --rm 一般用来测试，用完就删
 
# 下载再启动
docker pull tomcat
 
# 启动运行
docker run -d -p 3344:8080 --name tomcat01 tomcat
 
# 测试访问没有问题
 
# 进入容器
docker exec -it tomcat01 /bin/bash
 
# 发现问题：1.linux命令少了， 2. webapps下内容为空，阿里云净吸纳过默认是最小的镜像，所有不必要的都剔除了，保证最小可运行环境即可
```

3. Docker 部署 es + kibana
------------------------

```
# es 暴露的端口很多
# es 十分的耗内存
# es 的数据一般需要放置到安全目录！ 挂载
# --net somenetwork 网络配置
 
# 启动elasticsearch
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2
 
[root@iZ2zeg4ytp0whqtmxbsqiiZ home]# docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2
a920894a940b354d3c867079efada13d96cf9138712c76c8dea58fabd9c7e96f
 
# 启动了linux就卡主了，docker stats 查看cpu状态
 
# 测试一下es成功了
[root@iZ2zeg4ytp0whqtmxbsqiiZ home]# curl localhost:9200
{
  "name" : "a920894a940b",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "bxE1TJMEThKgwmk7Aa3fHQ",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
 
 
# 增加内存限制，修改配置文件 -e 环境配置修改
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
```

### 可视化

*   portainer（先用这个）

```
docker run -d -p 8088:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
 
# 测试
[root@iZ2zeg4ytp0whqtmxbsqiiZ home]# curl localhost:8088
<!DOCTYPE html
><html lang="en" ng-app="portainer">
 
# 外网访问 http://ip:8088
 
```

![](https://i-blog.csdnimg.cn/blog_migrate/93869db1b6f73f471aa35a49c18fac1c.png)

*   Rancher(CI/CD 再用)

### 四. Docker 原理

> 特点

Docker 奖项都是只读的，当容器启动时， 一个新的可写层被加载到镜像的顶部！

这一层就是我们通常说的容器层， 容器之下的都叫做镜像层

![](https://i-blog.csdnimg.cn/blog_migrate/1b08e945b927521e0b1407fb25f91338.png)

*   commit 镜像

```
docker commit 提交容器成为一个新的版本
 
# 命令和git 原理类似
docker commit -m="提交的描述信息" -a="作者" 容器id 目标镜像名：[TAG]
 
docker commit -a="xiaofan" -m="add webapps app" d798a5946c1f tomcat007:1.0
 
```

实战测试

```
# 1. 启动一个默认的tomcat
# 2. 发现这个默认的tomcat是没有webapps应用， 镜像的原因，官方镜像默认webapps下面是没有内容的
# 3. 我自己拷贝进去了基本的文件
# 4. 将我们操作过的容器通过commit提价为一个镜镜像！我们以后就使用我们自己制作的镜像了
```

五，容器数据卷
-------

1. 容器数据卷
--------

### 1.1. docker 的理解回顾

将应用和环境打包成一个镜像！

数据？如果数据都在容器中，那么我们容器删除，数据就会丢失！`需求：数据可以持久化`

MySQL，容器删了，删库跑路！`需求：MySQL数据可以存储在本地！`

容器之间可以有一个数据共享技术！Docker 容器中产生的数据，同步到本地！

这就是卷技术，目录的挂载，将我们容器内的目录挂载到 linux 目录上面！

** 总结： ** 容器的持久化和同步操作！容器间数据也是可以共享的！

### 1.2. 使用数据卷

> 方式一：直接使用命令来挂载 -v

```
docker run -it -v 主机目录：容器目录
 
[root@iZ2zeg4ytp0whqtmxbsqiiZ home]# docker run -it -v /home/ceshi:/home centos /bin/bash
```

![](https://i-blog.csdnimg.cn/blog_migrate/a0819ff72f5436a9c4bc41e35ac22731.png#pic_center)

**测试文件的同步**（在主机上改动，观察容器变化）

![](https://i-blog.csdnimg.cn/blog_migrate/b1bfa9f78d6d79a111a2f0df524fc7ee.png#pic_center)

**再来测试**（测试通过）

1.  停止容器
    
2.  主机上修改文件
    
3.  启动容器
    
4.  容器内的数据依旧是同步的！
    

### 1.3. 实战：安装 MySQL

思考：MySQL 的数据持久化的问题！

```
# 获取镜像
[root@iZ2zeg4ytp0whqtmxbsqiiZ home]# docker pull mysql:5.7
 
# 运行容器， 需要做数据挂载！ # 安装启动mysql，需要配置密码（注意）
# 官方测试， docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
 
# 启动我们的
-d      # 后台运行
-p      # 端口隐射
-v      # 卷挂载
-e      # 环境配置
--name  # 容器的名字
[root@iZ2zeg4ytp0whqtmxbsqiiZ home]# docker run -d -p 3344:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
9552bf4eb2b69a2ccd344b5ba5965da4d97b19f2e1a78626ac1f2f8d276fc2ba
 
# 启动成功之后，我们在本地使用navicat链接测试一下
# navicat链接到服务器的3344 --- 3344 和 容器的3306映射，这个时候我们就可以连接上mysql喽！
 
# 在本地测试创建一个数据库，查看下我们的路径是否ok！
```

![](https://i-blog.csdnimg.cn/blog_migrate/9f9c332ae049af404b178e7f38acde9d.png#pic_center)

### 1.4. 匿名和具名挂载

```
# 匿名挂载
-v 容器内路径
docker run -d -P --name nginx01 -v /etc/nginx nginx     # -P 随机指定端口
 
# 查看所有volume的情况
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker volume ls
DRIVER              VOLUME NAME
local               561b81a03506f31d45ada3f9fb7bd8d7c9b5e0f826c877221a17e45d4c80e096
local               36083fb6ca083005094cbd49572a0bffeec6daadfbc5ce772909bb00be760882
 
# 这里发现，这种情况就是匿名挂载，我们在-v 后面只写了容器内的路径，没有写容器外的路径！
 
# 具名挂载
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx
26da1ec7d4994c76e80134d24d82403a254a4e1d84ec65d5f286000105c3da17
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
26da1ec7d499        nginx               "/docker-entrypoint.…"   3 seconds ago       Up 2 seconds        0.0.0.0:32769->80/tcp   nginx02
486de1da03cb        nginx               "/docker-entrypoint.…"   3 minutes ago       Up 3 minutes        0.0.0.0:32768->80/tcp   nginx01
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker volume ls
DRIVER              VOLUME NAME
local               561b81a03506f31d45ada3f9fb7bd8d7c9b5e0f826c877221a17e45d4c80e096
local               36083fb6ca083005094cbd49572a0bffeec6daadfbc5ce772909bb00be760882
local               juming-nginx
 
# 通过-v 卷名：容器内的路径
# 查看一下这个卷
# docker volume inspect juming-nginx
 
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker volume inspect juming-nginx
[
  {
      "CreatedAt": "2020-08-12T18:15:21+08:00",
      "Driver": "local",
      "Labels": null,
      "Mountpoint": "/var/lib/docker/volumes/juming-nginx/_data",
      "Name": "juming-nginx",
      "Options": null,
      "Scope": "local"
  }
]
```

所有 docker 容器内的卷，没有指定目录的情况下都是在`/var/lib/docker/volumes/xxxxx/_data`

我们通过具名挂载可以方便的找到我们的一个卷，大多数情况下使用的是`具名挂载`

```
# 如何确定是具名挂载还是匿名挂载，还是指定路径挂载！
-v  容器内路径                   # 匿名挂载
-v  卷名:容器内路径               # 具名挂载
-v /主机路径:容器内路径            # 指定路径挂载
```

拓展

```
# 通过 -v 容器内容路径 ro rw 改变读写权限
ro  readonly    # 只读
rw  readwrite   # 可读可写
 
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:ro nginx
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:rw nginx
 
# ro 只要看到ro就说明这个路径只能通过宿主机来操作，容器内容无法操作
```

### 六，DockerFile

### 初始 DockerFile

DockerFile 就是用来狗之间 docker 镜像的构建文件！命令脚本！先体验一下！

通过这个脚本可以生成镜像，镜像是一层一层的，脚本一个个的命令，每个命令都是一层！

```
# 创建一个dockerfile文件， 名字可以随机
# 文件的内容 指定（大写） 参数
 
FROM centos
 
VOLUME ["volume01", "volume02"]
 
CMD echo "----end----"
CMD /bin/bash
 
# 这里的每一个命令都是镜像的一层！
```

![](https://i-blog.csdnimg.cn/blog_migrate/b52cf040f878e8107b8d25dbf43bdd18.png#pic_center)

```
# 启动自己的容器


```

![](https://i-blog.csdnimg.cn/blog_migrate/ed4c8765fb9dbb4b2f50f4fd016cd2f1.png#pic_center)

这个卷和外部一定有一个同步的目录！

![](https://i-blog.csdnimg.cn/blog_migrate/e57623789a9a7abefb86bd625bf701ef.png#pic_center)

```
docker inspect 容器id


```

![](https://i-blog.csdnimg.cn/blog_migrate/2f5be7f5f26ca38418e173657edd671d.png#pic_center)

测试一下刚才的文件是否同步到主机上了！

这种方式我们未来使用的十分多， 因为我们通常会构建自己的镜像！

假设构建镜像时候没有挂载卷，要手动镜像挂载 -v 卷名: 容器内路径！

### 数据卷容器

多个 mysql 同步数据！

![](https://i-blog.csdnimg.cn/blog_migrate/16400dcc902bf2840a822d82142e474f.png#pic_center)

```
# 启动3个容器，通过我们刚才自己写的镜像启动


```

![](https://i-blog.csdnimg.cn/blog_migrate/71be83d9cf07c33be4af6cc0cdd1422b.png#pic_center)

![](https://i-blog.csdnimg.cn/blog_migrate/a1631099701847e75aeca24587e97cfe.png#pic_center)

多个 mysql 实现数据共享

```
[root@iZ2zeg4ytp0whqtmxbsqiiZ home]# docker run -d -p 3344:3306 -v /etc/mysql/conf.d -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
 
[root@iZ2zeg4ytp0whqtmxbsqiiZ home]# docker run -d -p 3344:3306 -v /etc/mysql/conf.d -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql02 --volumes-from mysql01 mysql:5.7
```

**结论**

容器之间配置信息的传递， 数据卷容器的声明周期一直持续到没有容器使用为止。

但是一旦你持久化到了本地，这个时候，本地的数据是不会删除的！

### DockerFile

dockerFile 是用来构建 docker 镜像的文件！命令参数脚本！

`构建步骤`

`1. 编写一个dockerFile文件`

`2.docker build 构建成为一个镜像`

`3. docker run 运行镜像`

`4. docker push 发布镜像（DockerHub、阿里云镜像）`

查看婴喜爱官方是怎么做的？

![](https://i-blog.csdnimg.cn/blog_migrate/6d695fb81f25ffd4b86f99e3b11320ca.png#pic_center)

![](https://i-blog.csdnimg.cn/blog_migrate/35cbc225bed453ad8af3cb3c965297a9.png#pic_center)

很多官方镜像都像是基础包，很多功能都不具备，我们通常会自己搭建自己的镜像！

官方既然可以制作镜像，能我们一样可以！

### DockerFile 的构建过程

**基础知识：**

1.  每个保留关键字（指令）都是必须大写字母
2.  执行从上到下顺序执行
3.  `#` 表示注释
4.  每个指令都会创建提交一个新的镜像层，并提交！

![](https://i-blog.csdnimg.cn/blog_migrate/126043b38ed9817b306c87c58e8b8a43.png#pic_center)

dockerFile 是面向开发的， 我们以后要发布项目， 做镜像， 就需要编写 dockefile 文件， 这个文件十分简单！

Docker 镜像逐渐成为企业的交互标准，必须要掌握！

步骤：开发，部署， 运维..... 缺一不可！

DockerFile： 构建文件， 定义了一切的步骤，源代码

DockerImages： 通过 DockerFile 构建生成的镜像， 最终发布和运行的产品！

Docker 容器：容器就是镜像运行起来提供服务器

### DockerFile 指令说明

![](https://i-blog.csdnimg.cn/blog_migrate/f34d33f60d820615a1fce410b9080e2d.png#pic_center)

```
FROM            # 基础镜像，一切从这里开始构建
MAINTAINER      # 镜像是谁写的， 姓名+邮箱
RUN             # 镜像构建的时候需要运行的命令
ADD             # 步骤， tomcat镜像， 这个tomcat压缩包！添加内容
WORKDIR         # 镜像的工作目录
VOLUME          # 挂载的目录
EXPOSE          # 保留端口配置
CMD             # 指定这个容器启动的时候要运行的命令，只有最后一个会生效可被替代
ENTRYPOINT      # 指定这个容器启动的时候要运行的命令， 可以追加命令
ONBUILD         # 当构建一个被继承DockerFile 这个时候就会运行 ONBUILD 的指令，触发指令
COPY            # 类似ADD, 将我们文件拷贝到镜像中
ENV             # 构建的时候设置环境变量！
```

### 创建一个自己的 centos

```
# 1. 编写Dockerfile的文件
[root@iZ2zeg4ytp0whqtmxbsqiiZ dockerfile]# cat mydockerfile-centos 
FROM centos
MAINTAINER xiaofan<594042358@qq.com>
 
ENV MYPATH /usr/local
WORKDIR $MYPATH     # 镜像的工作目录
 
RUN yum -y install vim
RUN yum -y install net-tools
 
EXPOSE 80
 
CMD echo $MYPATH
CMD echo "---end---"
CMD /bin/bash
 
# 2. 通过这个文件构建镜像
# 命令 docker build -f dockerfile文件路径 -t 镜像名:[tag] .
 
[root@iZ2zeg4ytp0whqtmxbsqiiZ dockerfile]# docker build -f mydockerfile-centos -t mycentos:0.1 .
 
Successfully built d2d9f0ea8cb2
Successfully tagged mycentos:0.1
```

![](https://i-blog.csdnimg.cn/blog_migrate/bbdffbb615218b0d01ab1349210ba0a2.png#pic_center)

我们可以列出本地进行的变更历史

![](https://i-blog.csdnimg.cn/blog_migrate/33e7ac22c6051c08d445754ed23a4cb7.png#pic_center)

> CMD 和 ENTRYPOINT 区别

```
CMD         # 指定这个容器启动的时候要运行的命令，只有最后一个会生效可被替代
ENTRYPOINT      # 指定这个容器启动的时候要运行的命令， 可以追加命令
```

测试 CMD

```
# 1. 编写dockerfile文件
[root@iZ2zeg4ytp0whqtmxbsqiiZ dockerfile]# vim dockerfile-cmd-test 
FROM centos
CMD ["ls", "-a"]
 
# 2. 构建镜像
[root@iZ2zeg4ytp0whqtmxbsqiiZ dockerfile]# docker build -f dockerfile-cmd-test -t cmdtest .
 
# 3. run运行， 发现我们的ls -a 命令生效
[root@iZ2zeg4ytp0whqtmxbsqiiZ dockerfile]# docker run ebe6a52bb125
.
..
.dockerenv
bin
dev
etc
home
lib
lib64
 
# 想追加一个命令 -l 变成 ls -al
[root@iZ2zeg4ytp0whqtmxbsqiiZ dockerfile]# docker run ebe6a52bb125 -l
docker: Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "exec: \"-l\": executable file not found in $PATH": unknown.
[root@iZ2zeg4ytp0whqtmxbsqiiZ dockerfile]# docker run ebe6a52bb125 ls -l
 
# cmd的情况下 -l替换了CMD["ls", "-a"]命令， -l不是命令，所以报错了
```

测试 ENTRYPOINT

```
# 1. 编写dockerfile文件
[root@iZ2zeg4ytp0whqtmxbsqiiZ dockerfile]# vim dockerfile-entrypoint-test 
FROM centos
ENTRYPOINT ["ls", "-a"]
 
# 2. 构建文件
[root@iZ2zeg4ytp0whqtmxbsqiiZ dockerfile]# docker build -f dockerfile-entrypoint-test -t entrypoint-test .
 
# 3. run运行 发现我们的ls -a 命令同样生效
[root@iZ2zeg4ytp0whqtmxbsqiiZ dockerfile]# docker run entrypoint-test
.
..
.dockerenv
bin
dev
etc
home
lib
 
# 4. 我们的追加命令， 是直接拼接到ENTRYPOINT命令的后面的！
[root@iZ2zeg4ytp0whqtmxbsqiiZ dockerfile]# docker run entrypoint-test -l
total 56
drwxr-xr-x  1 root root 4096 Aug 13 07:52 .
drwxr-xr-x  1 root root 4096 Aug 13 07:52 ..
-rwxr-xr-x  1 root root    0 Aug 13 07:52 .dockerenv
lrwxrwxrwx  1 root root    7 May 11  2019 bin -> usr/bin
drwxr-xr-x  5 root root  340 Aug 13 07:52 dev
drwxr-xr-x  1 root root 4096 Aug 13 07:52 etc
drwxr-xr-x  2 root root 4096 May 11  2019 home
lrwxrwxrwx  1 root root    7 May 11  2019 lib -> usr/lib
lrwxrwxrwx  1 root root    9 May 11  2019 lib64 -> usr/lib64
drwx------  2 root root 4096 Aug  9 21:40 lost+found
 
```

### 七，Dockerfile 制作 tomcat 镜像

### Dockerfile 制作 tomcat 镜像

1.  准备镜像文件 tomcat 压缩包，jdk 的压缩包！

![](https://i-blog.csdnimg.cn/blog_migrate/5b4a856ebcc089d194f612af9e31c107.png#pic_center)

1.  编写 Dockerfile 文件，官方命名`Dockerfile`, build 会自动寻找这个文件，就不需要 - f 指定了！

```
[root@iZ2zeg4ytp0whqtmxbsqiiZ tomcat]# cat Dockerfile 
FROM centos
MAINTAINER xiaofan<594042358@qq.com>
 
COPY readme.txt /usr/local/readme.txt
 
ADD jdk-8u73-linux-x64.tar.gz /usr/local/
ADD apache-tomcat-9.0.37.tar.gz /usr/local/
 
RUN yum -y install vim
 
ENV MYPATH /usr/local
WORKDIR $MYPATH
 
ENV JAVA_HOME /usr/local/jdk1.8.0_73
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.37
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.37
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
 
EXPOSE 8080
 
CMD /usr/local/apache-tomcat-9.0.37/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.37/bin/logs/catalina.out
 
```

1.  构建镜像

```
# docker build -t diytomcat .


```

1.  启动镜像

```
#  docker run -d -p 3344:8080 --name xiaofantomcat1 -v /home/xiaofan/build/tomcat/test:/usr/local/apache-tomcat-9.0.37/webapps/test -v /home/xiaofan/build/tomcat/tomcatlogs/:/usr/local/apache-tomcat-9.0.37/logs diytomcat


```

1.  访问测试
    
2.  发布项目（由于做了卷挂载， 我们直接在本地编写项目就可以发布了）
    

**在本地编写 web.xml 和 index.jsp 进行测试**

![](https://i-blog.csdnimg.cn/blog_migrate/7cdb99370444eba9cbc60cd5a8a96ead.png#pic_center)

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.4" 
    xmlns="http://java.sun.com/xml/ns/j2ee" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
        http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
        
</web-app>
```

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>hello. xiaofan</title>
</head>
<body>
Hello World!<br/>
<%
System.out.println("-----my test web logs------");
%>
</body>
</html>
```

发现：项目部署成功， 可以直接访问 ok！

我们以后开发的步骤：需要掌握 Dockerfile 的编写！ 我们之后的一切都是使用 docker 进行来发布运行的！

![](https://i-blog.csdnimg.cn/blog_migrate/7e7eb10f133d5066ea4a1f7ca965d703.png#pic_center)

### 发布自己的镜像到 Docker Hub

> Docker Hub

1.  [地址](https://hub.docker.com/) 注册自己的账号！
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/a9f0da6c4bc6cc5cc016c8205e593cc4.png#pic_center)
    
2.  确定这个账号可以登录
    

![](https://i-blog.csdnimg.cn/blog_migrate/2ef3aee2eeef18431be327321906d6fb.png#pic_center)

1.  在我们的服务器上提交自己的镜像

```
# push到我们的服务器上
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker push diytomcat
The push refers to repository [docker.io/library/diytomcat]
2eaca873a720: Preparing 
1b38cc4085a8: Preparing 
088ebb58d264: Preparing 
c06785a2723d: Preparing 
291f6e44771a: Preparing 
denied: requested access to the resource is denied  # 拒绝
 
# push镜像的问题？
The push refers to repository [docker.io/1314520007/diytomcat]
An image does not exist locally with the tag: 1314520007/diytomcat
 
# 解决，增加一个tag
docker tag diytomcat 1314520007/tomcat:1.0
```

![](https://i-blog.csdnimg.cn/blog_migrate/8c061edc87925c3ca6d8e2332ae514a7.png#pic_center)

### 发布到阿里云镜像服务上

1.  登录阿里云
2.  找到容器镜像服务
3.  创建命名空间

![](https://i-blog.csdnimg.cn/blog_migrate/82bd6f001dc8b9cae529f2f436c653e6.png#pic_center)

1.  创建容器镜像

![](https://i-blog.csdnimg.cn/blog_migrate/e13f909852762228de2ff38eec52027b.png#pic_center)

1.  点击仓库名称，参考官方文档即可

![](https://i-blog.csdnimg.cn/blog_migrate/26d2f0d4e144de88f430dbc635106282.png#pic_center)

### 总结

![](https://i-blog.csdnimg.cn/blog_migrate/cc6561fc8b4de802b65b4140a5228335.png#pic_center)

![](https://i-blog.csdnimg.cn/blog_migrate/fa8a5f4aa8aa798c3d90abd9322f6da7.png#pic_center)

### 八，Docker 网络

1. Docker 网络
------------

### 链接 Docker0

> 测试

![](https://i-blog.csdnimg.cn/blog_migrate/4160f26a2529495900885a1bc276a703.png#pic_center)

三个网络

```
# 问题： docker是如何处理容器网络访问的？
 
# [root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker run -d -P --name tomcat01 tomcat
 
# 查看容器内部的网络地址 ip addr
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker exec -it tomcat01 ip addr， 发现容器启动的时候得到一个eth0@if115 ip地址，docker分配的！
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
114: eth0@if115: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
 
# 思考： linux 能不能ping通容器？
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.077 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.069 ms
64 bytes from 172.17.0.2: icmp_seq=3 ttl=64 time=0.075 ms
 
# linux 可以 ping 通docker容器内部！
```

> 原理

1.  我们没启动一个 docker 容器， docker 就会给 docker 容器分配一个 ip， 我们只要安装了 docker，就会有一个网卡 docker0 桥接模式，使用的技术是 veth-pair 技术！

再次测试 ip addr

![](https://i-blog.csdnimg.cn/blog_migrate/3a0fff4337d022787ada9dd672e68ce4.png#pic_center)

1.  再启动一个容器测试， 发现又多了一对网卡

![](https://i-blog.csdnimg.cn/blog_migrate/c6448f911cb3b745ad026fcffaa3baed.png#pic_center)

```
# 我们发现这个容器带来网卡，都是一对对的
# veth-pair 就是一对的虚拟设备接口，他们都是成对出现的，一端连着协议，一端彼此相连
# 正因为有这个特性，veth-pair充当一个桥梁， 连接各种虚拟网络设备
# OpenStac， Docker容器之间的链接，OVS的链接， 都是使用veth-pair技术
```

1.  我们测试一下 tomcat01 和 tomcat02 之间是否可以 ping 通！
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/d682432ee26f461cdf3e62bccf51c7c1.png#pic_center)
    

结论：容器与容器之间是可以相互 ping 通的！

绘制一个网络模型图

![](https://i-blog.csdnimg.cn/blog_migrate/31db8e04af5743cedd00d058e13f6c6a.png#pic_center)

结论：tomcat01 和 tomcat02 是共用的一个路由器，docker0

所有容器不指定网络的情况下，都是 docker0 路由的，doucker 会给我们的容器分配一个默认的可用 IP

> 小结

Docker 使用的是 Linux 的桥接，宿主机中是一个 Docker 容器的网桥 docker0.

![](https://i-blog.csdnimg.cn/blog_migrate/e777061214f6e155202375777c0c133d.png#pic_center)

Docker 中的所有的网络接口都是虚拟的，虚拟的转发效率高！（内网传递文件！）

只要容器删除，对应的网桥一对就没有了！

![](https://i-blog.csdnimg.cn/blog_migrate/8c067607241c64adde4c507572d0c3e9.png#pic_center)

### -- link

> 思考一个场景，我们编写了一个微服务，database url =ip； 项目不重启，数据 ip 换掉了，我们希望可以处理这个问题，可以按名字来进行访问容器

```
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker exec -it tomcat02 ping tomcat01
ping: tomcat01: Name or service not known
 
# 如何可以解决呢？
# 通过--link既可以解决网络连通问题
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker run -d -P  --name tomcat03 --link tomcat02 tomcat
3a2bcaba804c5980d94d168457c436fbd139820be2ee77246888f1744e6bb473
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                     NAMES
3a2bcaba804c        tomcat              "catalina.sh run"   4 seconds ago       Up 3 seconds        0.0.0.0:32772->8080/tcp   tomcat03
f22ed47ed1be        tomcat              "catalina.sh run"   57 minutes ago      Up 57 minutes       0.0.0.0:32771->8080/tcp   tomcat02
9d97f93401a0        tomcat              "catalina.sh run"   About an hour ago   Up About an hour    0.0.0.0:32770->8080/tcp   tomcat01
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker exec -it tomcat03 ping tomcat02
PING tomcat02 (172.17.0.3) 56(84) bytes of data.
64 bytes from tomcat02 (172.17.0.3): icmp_seq=1 ttl=64 time=0.129 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=2 ttl=64 time=0.100 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=3 ttl=64 time=0.110 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=4 ttl=64 time=0.107 ms
 
# 反向可以ping通吗？
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker exec -it tomcat02 ping tomcat03
ping: tomcat03: Name or service not known
 
```

探究：inspect！

![](https://i-blog.csdnimg.cn/blog_migrate/cea6c78f5544c09c443b16804e0f751e.png#pic_center)

其实这个 tomcat03 就是在本地配置了 tomcat02 的配置？

```
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker exec -it tomcat03 cat /etc/hosts
127.0.0.1   localhost
::1 localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
172.17.0.3  tomcat02 f22ed47ed1be
172.17.0.4  3a2bcaba804c
```

本质探究：--link 就是我们在 hosts 配置中增加了一个 172.17.0.3 tomcat02 f22ed47ed1be

我们现在玩 Docker 已经不建议使用 --link 了！

自定义网络！不使用 Docker0！

Docker0 的问题：它不支持容器名链接访问！

### 自定义网络

> 查看所有的 docker 网络

![](https://i-blog.csdnimg.cn/blog_migrate/4dadae6d45748f81abf1342e1f1c7fad.png#pic_center)

**网络模式**

bridge： 桥接模式，桥接 docker 默认，自己创建的也是用 brdge 模式

none： 不配置网络

host： 和宿主机共享网络

container：容器网络连通！（用的少， 局限很大）

**测试**

```
# 我们直接启动的命令默认有一个 --net bridge，而这个就是我们的docker0
docker run -d -P --name tomcat01 tomcat
docker run -d -P --name tomcat01 --net bridge tomcat
 
# docker0特点，默认，容器名不能访问， --link可以打通连接！
# 我们可以自定义一个网络！
# --driver bridge
# --subnet 192.168.0.0/16 可以支持255*255个网络 192.168.0.2 ~ 192.168.255.254
# --gateway 192.168.0.1
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
26a5afdf4805d7ee0a660b82244929a4226470d99a179355558dca35a2b983ec
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
30d601788862        bridge              bridge              local
226019b14d91        host                host                local
26a5afdf4805        mynet               bridge              local
7496c014f74b        none                null                local
```

我们自己创建的网络就 ok 了！

![](https://i-blog.csdnimg.cn/blog_migrate/6d0992ac3cae8b83f52db7057f77f4c8.png#pic_center)

在自己创建的网络里面启动两个容器

```
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker run -d -P --name tomcat-net-01 --net mynet tomcat
0e85ebe6279fd23379d39b27b5f47c1e18f23ba7838637802973bf6449e22f5c
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker run -d -P --name tomcat-net-02 --net mynet tomcat
c6e462809ccdcebb51a4078b1ac8fdec33f1112e9e416406b606d0c9fb6f21b5
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker network inspect mynet
[
    {
        "Name": "mynet",
        "Id": "26a5afdf4805d7ee0a660b82244929a4226470d99a179355558dca35a2b983ec",
        "Created": "2020-08-14T11:12:40.553433163+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "0e85ebe6279fd23379d39b27b5f47c1e18f23ba7838637802973bf6449e22f5c": {
                "Name": "tomcat-net-01",
                "EndpointID": "576ce5c0f5860a5aab5e487a805da9d72f41a409c460f983c0bd341dd75d83ac",
                "MacAddress": "02:42:c0:a8:00:02",
                "IPv4Address": "192.168.0.2/16",
                "IPv6Address": ""
            },
            "c6e462809ccdcebb51a4078b1ac8fdec33f1112e9e416406b606d0c9fb6f21b5": {
                "Name": "tomcat-net-02",
                "EndpointID": "81ecbc4fe26e49855fe374f2d7c00d517b11107cc91a174d383ff6be37d25a30",
                "MacAddress": "02:42:c0:a8:00:03",
                "IPv4Address": "192.168.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
 
# 再次拼连接
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker exec -it tomcat-net-01 ping 192.168.0.3
PING 192.168.0.3 (192.168.0.3) 56(84) bytes of data.
64 bytes from 192.168.0.3: icmp_seq=1 ttl=64 time=0.113 ms
64 bytes from 192.168.0.3: icmp_seq=2 ttl=64 time=0.093 ms
^C
--- 192.168.0.3 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 999ms
rtt min/avg/max/mdev = 0.093/0.103/0.113/0.010 ms
# 现在不使用 --link也可以ping名字了！
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker exec -it tomcat-net-01 ping tomcat-net-02
PING tomcat-net-02 (192.168.0.3) 56(84) bytes of data.
64 bytes from tomcat-net-02.mynet (192.168.0.3): icmp_seq=1 ttl=64 time=0.068 ms
64 bytes from tomcat-net-02.mynet (192.168.0.3): icmp_seq=2 ttl=64 time=0.096 ms
64 bytes from tomcat-net-02.mynet (192.168.0.3): icmp_seq=3 ttl=64 time=0.094 ms
 
```

我们自定义的网络 docker 都已经帮我们维护好了对应的关系，推荐我们平时这样使用网络

好处：

redis - 不同的集群使用不同的网络，保证集群时安全和健康的

mysql - 不同的集群使用不同的网络，保证集群时安全和健康的

### 网络连通

![](https://i-blog.csdnimg.cn/blog_migrate/6849ec4b1fe64fdd7b5b0b93f4ffd359.png#pic_center)

测试打通 tomcat01 和 mynet

![](https://i-blog.csdnimg.cn/blog_migrate/2249d7b8d87338300dbe8f9a8de0a37d.png#pic_center)

```
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker network connect  mynet tomcat01
 
# 连通之后就是讲tomcat01 放到了mynet网路下
# 一个容器两个ip地址：
# 阿里云服务器，公网ip，私网ip
```

![](https://i-blog.csdnimg.cn/blog_migrate/9fadae8ec418a0eb72f6f16cdfeb8455.png#pic_center)

![](https://i-blog.csdnimg.cn/blog_migrate/637cc0f3c3053dbc68cb7dab0c4f30d0.png#pic_center)

```
# 连通ok
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker exec -it tomcat01 ping tomcat-net-01
PING tomcat-net-01 (192.168.0.2) 56(84) bytes of data.
64 bytes from tomcat-net-01.mynet (192.168.0.2): icmp_seq=1 ttl=64 time=0.100 ms
64 bytes from tomcat-net-01.mynet (192.168.0.2): icmp_seq=2 ttl=64 time=0.085 ms
^C
--- tomcat-net-01 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1000ms
rtt min/avg/max/mdev = 0.085/0.092/0.100/0.012 ms
# 依旧无法连通，没有connect
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker exec -it tomcat02 ping tomcat-net-01
ping: tomcat-net-01: Name or service not known
 
```

结论：假设要跨网络 操作别人，就要使用 docker network connect 连通.....!

### 实战：部署 redis

![](https://i-blog.csdnimg.cn/blog_migrate/907af86f80f1aaf2f6f8e1a6cb7431d4.png#pic_center)

```
# 创建网卡
docker network create redis --subnet 172.38.0.0/16
 
# 通过脚本创建六个redis配置
for port in $(seq 1 6); \
do \
mkdir -p /mydata/redis/node-${port}/conf
touch /mydata/redis/node-${port}/conf/redis.conf
cat << EOF >/mydata/redis/node-${port}/conf/redis.conf
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
done
# 创建结点1
docker run -p 6371:6379 -p 16371:16379 --name redis-1 \
-v /mydata/redis/node-1/data:/data \
-v /mydata/redis/node-1/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.11 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
 
#创建结点2
docker run -p 6372:6379 -p 16372:16379 --name redis-2 \
-v /mydata/redis/node-2/data:/data \
-v /mydata/redis/node-2/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.12 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
#创建结点3
docker run -p 6373:6379 -p 16373:16379 --name redis-3 \
-v /mydata/redis/node-3/data:/data \
-v /mydata/redis/node-3/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.13 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
#创建结点4
docker run -p 6374:6379 -p 16374:16379 --name redis-4 \
-v /mydata/redis/node-4/data:/data \
-v /mydata/redis/node-4/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.14 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
#创建结点5
docker run -p 6375:6379 -p 16375:16379 --name redis-5 \
-v /mydata/redis/node-5/data:/data \
-v /mydata/redis/node-5/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.15 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
#创建结点6
docker run -p 6376:6379 -p 16376:16379 --name redis-6 \
-v /mydata/redis/node-6/data:/data \
-v /mydata/redis/node-6/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.16 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
 
# 创建集群
[root@iZ2zeg4ytp0whqtmxbsqiiZ ~]# docker exec -it redis-1 /bin/sh
/data # ls
appendonly.aof  nodes.conf
/data # redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 172.38.0.15:6379 to 172.38.0.11:6379
Adding replica 172.38.0.16:6379 to 172.38.0.12:6379
Adding replica 172.38.0.14:6379 to 172.38.0.13:6379
M: 541b7d237b641ac2ffc94d17c6ab96b18b26a638 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
M: a89c1f1245b264e4a402a3cf99766bcb6138dbca 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
M: 259e804d6df74e67a72e4206d7db691a300c775e 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
S: 9b19170eea3ea1b92c58ad18c0b5522633a9e271 172.38.0.14:6379
   replicates 259e804d6df74e67a72e4206d7db691a300c775e
S: 061a9d38f22910aaf0ba1dbd21bf1d8f57bcb7d5 172.38.0.15:6379
   replicates 541b7d237b641ac2ffc94d17c6ab96b18b26a638
S: 7a16b9bbb0615ec95fc978fa62fc054df60536f0 172.38.0.16:6379
   replicates a89c1f1245b264e4a402a3cf99766bcb6138dbca
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
...
>>> Performing Cluster Check (using node 172.38.0.11:6379)
M: 541b7d237b641ac2ffc94d17c6ab96b18b26a638 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
M: a89c1f1245b264e4a402a3cf99766bcb6138dbca 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
S: 7a16b9bbb0615ec95fc978fa62fc054df60536f0 172.38.0.16:6379
   slots: (0 slots) slave
   replicates a89c1f1245b264e4a402a3cf99766bcb6138dbca
S: 061a9d38f22910aaf0ba1dbd21bf1d8f57bcb7d5 172.38.0.15:6379
   slots: (0 slots) slave
   replicates 541b7d237b641ac2ffc94d17c6ab96b18b26a638
M: 259e804d6df74e67a72e4206d7db691a300c775e 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
S: 9b19170eea3ea1b92c58ad18c0b5522633a9e271 172.38.0.14:6379
   slots: (0 slots) slave
   replicates 259e804d6df74e67a72e4206d7db691a300c775e
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
 
 
```

docker 搭建 redis 集群完成！

![](https://i-blog.csdnimg.cn/blog_migrate/e11793945c494e8c9a220f2588ae73e6.png#pic_center)

### SpringBoot 微服务打包 Docker 镜像

1.  构建 springboot 项目

? [IDEA2020 Ultimate 版本激活方案](https://tech.souyunku.com/?p=11599) `亲测有效`

1.  打包应用
2.  编写 Dockerfile

```
FROM java:8
 
COPY *.jar /app.jar
CMD ["--server.port=8080"]
 
EXPOSE 8080
 
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

1.  构建镜像
    
2.  发布运行！
    

```
# 把打好的jar包和Dockerfile上传到linux
[root@iZ2zeg4ytp0whqtmxbsqiiZ idea]# ll
total 16140
-rw-r--r-- 1 root root 16519871 Aug 14 17:38 demo-0.0.1-SNAPSHOT.jar
-rw-r--r-- 1 root root      122 Aug 14 17:38 Dockerfile
 
# 构建镜像，不要忘了最后有一个点
[root@iZ2zeg4ytp0whqtmxbsqiiZ idea]# docker build -t xiaofan666 .
Sending build context to Docker daemon  16.52MB
Step 1/5 : FROM java:8
8: Pulling from library/java
5040bd298390: Pull complete 
fce5728aad85: Pull complete 
76610ec20bf5: Pull complete 
60170fec2151: Pull complete 
e98f73de8f0d: Pull complete 
11f7af24ed9c: Pull complete 
49e2d6393f32: Pull complete 
bb9cdec9c7f3: Pull complete 
Digest: sha256:c1ff613e8ba25833d2e1940da0940c3824f03f802c449f3d1815a66b7f8c0e9d
Status: Downloaded newer image for java:8
 ---> d23bdf5b1b1b
Step 2/5 : COPY *.jar /app.jar
 ---> d4de8837ebf9
Step 3/5 : CMD ["--server.port=8080"]
 ---> Running in e3abc66303f0
Removing intermediate container e3abc66303f0
 ---> 131bb3917fea
Step 4/5 : EXPOSE 8080
 ---> Running in fa2f25977db7
Removing intermediate container fa2f25977db7
 ---> d98147377951
Step 5/5 : ENTRYPOINT ["java", "-jar", "/app.jar"]
 ---> Running in e1885e23773b
Removing intermediate container e1885e23773b
 ---> afb6b5f28a32
Successfully built afb6b5f28a32
Successfully tagged xiaofan666:latest
 
# 查看镜像
[root@iZ2zeg4ytp0whqtmxbsqiiZ idea]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
xiaofan666          latest              afb6b5f28a32        14 seconds ago      660MB
tomcat              latest              2ae23eb477aa        8 days ago          647MB
redis               5.0.9-alpine3.11    3661c84ee9d0        3 months ago        29.8MB
java                8                   d23bdf5b1b1b        3 years ago         643MB
 
# 运行容器
[root@iZ2zeg4ytp0whqtmxbsqiiZ idea]# docker run -d -P --name xiaofan-springboot-web xiaofan666
fd9a353a80bfd61f6930c16cd92204532bfd734e003f3f9983b5128a27b0375e
# 查看运行起来的容器端口（因为我们启动的时候没有指定）
[root@iZ2zeg4ytp0whqtmxbsqiiZ idea]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                     NAMES
fd9a353a80bf        xiaofan666          "java -jar /app.jar …"   9 seconds ago       Up 8 seconds        0.0.0.0:32779->8080/tcp   xiaofan-springboot-web
# 本地访问1
[root@iZ2zeg4ytp0whqtmxbsqiiZ idea]# curl localhost:32779
{"timestamp":"2020-08-14T09:42:57.371+00:00","status":404,"error":"Not Found","message":"","path":"/"}
# 本地访问2
[root@iZ2zeg4ytp0whqtmxbsqiiZ idea]# [root@iZ2zeg4ytp0whqtmxbsqiiZ idea]# curl localhost:32779/hello
hello, xiaofan
# 远程访问（开启阿里云上的安全组哦）
```

![](https://i-blog.csdnimg.cn/blog_migrate/99e19c93fc73d050c8bb81d50ddf801a.png#pic_center)

以后我们使用了 Docker 之后，给别人交互的就是一个镜像即可！

### 九，Docker Compose

Docker Compose
--------------

### 简介

Docker

Dockerfile build run 手动操作，单个容器！

微服务，100 个微服务，依赖关系。

Docker Compose 来轻松高效的管理容器，定义运行多个容器。

> 官方介绍

1.  定义运行多个容器
2.  YAML file 配置文件
3.  single command。命令有哪些？

Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration. To learn more about all the features of Compose, see [the list of features](https://docs.docker.com/compose/#features).

1.  所有的环境都可以使用 compose。

Compose works in all environments: production, staging, development, testing, as well as CI workflows. You can learn more about each case in [Common Use Cases](https://docs.docker.com/compose/#common-use-cases).

**三步骤：**

Using Compose is basically a three-step process:

1.  Define your app’s environment with a `Dockerfile` so it can be reproduced anywhere.
    *   Dockerfile 保证我们的项目再任何地方可以运行
2.  Define the services that make up your app in `docker-compose.yml` so they can be run together in an isolated environment.
    *   services 什么是服务。
3.  Run `docker-compose up` and Compose starts and runs your entire app.
    *   启动项目

**作用：批量容器编排**

> 我自己的理解

Compose 是 Docker 官方的开源项目，需要安装！

`Dockerfile`让程序在任何地方运行。web 服务、redis、mysql、nginx... 多个容器。 run

Compose

```
version: '2.0'
services:
  web:
    build: .
    ports:
    - "5000:5000"
    volumes:
    - .:/code
    - logvolume01:/var/log
    links:
    - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```

docker-compose up 100 个服务

Compose：重要概念

*   服务 services， 容器、应用（web、redis、mysql...）
*   项目 project。 一组关联的容器

### 安装

1.  下载

```
# 官网提供 （没有下载成功）
curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
 
# 国内地址
curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.5/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

1.  授权

```
chmod +x /usr/local/bin/docker-compose


```

![](https://i-blog.csdnimg.cn/blog_migrate/51990a4015182393d392805709710a06.png#pic_center)

### 体验 (没有测试通过)

地址：https://docs.docker.com/compose/gettingstarted/

python 应用。 计数器。redis！

1.  应用 app.py

1.  Dockerfile 应用打包为镜像
    
    ```
    FROM python:3.6-alpine
    ADD . /code
    WORKDIR /code
    RUN pip install -r requirements.txt
    CMD ["python", "app.py"]
     
    # 官网的用来flask框架，我们这里不用它
    # 这告诉Docker
    # 从python3.7开始构建镜像
    # 将当前目录添加到/code印像中的路径中
    # 将工作目录设置为/code
    # 安装Python依赖项
    # 将容器的默认命令设置为python app.py
    ```
    

1.  Docker-compose yaml 文件（定义整个服务，需要的环境 web、redis） 完整的上线服务！
    
    ```
    version: '3.8'
    services:
      web:
        build: .
        ports:
          - "5000:5000"
        volumes:
          - .:/code
      redis:
        image: "redis:alpine"
     
    ```
    
2.  启动 compose 项目 （docker-compose up）
    

流程：

1.  创建网络
2.  执行 Docker-compose.yaml
3.  启动服务

### yaml 规则

docker-compose.yaml 核心！

https://docs.docker.com/compose/compose-file/#compose-file-structure-and-examples

### 开源项目：博客

https://docs.docker.com/compose/wordpress/

下载程序、安装数据库、配置....

compose 应用 => 一键启动

1.  下载项目（docker-compse.yaml）
2.  如果需要文件。Dockerfile
3.  文件准备齐全，一键启动项目即可

![](https://i-blog.csdnimg.cn/blog_migrate/90ea6a6419425d22d6835a886330bb36.png#pic_center)

### 实战：自己编写微服务上线

1.  编写项目微服务
    
2.  Dockerfile 构建镜像
    
    ```
    FROM java:8
     
    COPY *.jar /app.jar
    CMD ["--server.port=8080"]
     
    EXPOSE 8080
     
    ENTRYPOINT ["java", "-jar", "/app.jar"]
    ```
    

1.  docker-compose.yml 编排项目
    
    ```
    version '3.8'
    services:
      xiaofanapp:
        build: .
        image: xiaofanapp
        depends_on:
          - redis
        ports:
          - "8080:8080"
     
      redis:
        image: "library/redis:alpine"
    ```
    

1.  丢到服务器运行 docker-compose up

```
docker-compose down         # 关闭容器
docker-compose up --build   # 重新构建
```

![](https://i-blog.csdnimg.cn/blog_migrate/6dbb401a5484ee184e9445674d9a502d.png#pic_center)

![](https://i-blog.csdnimg.cn/blog_migrate/de065f2f560ef20233ec11da6482d53f.png#pic_center)

**总结：**

**工程、服务、容器**

项目 compose： 三层

*   工程 Project
*   服务
*   容器 运行实例！ docker k8s 容器

### 十，Docker Swarm

Docker Swarm
------------

集群

### 购买服务器

1.  登录阿里云账号，进入控制台，创建实例
    
    ```
    4台服务器2G
    
    
    ```
    

![](https://i-blog.csdnimg.cn/blog_migrate/b2c663f5260c886c62b9eb063312413c.png#pic_center)

![](https://i-blog.csdnimg.cn/blog_migrate/cc1f9f16afe829399c3cabcff12729d6.png#pic_center)

![](https://i-blog.csdnimg.cn/blog_migrate/8f5ccdab12bcc9b52ba640487d9aeab1.png#pic_center)

![](https://i-blog.csdnimg.cn/blog_migrate/dbc1d60a555f7f22fe769cf585de10f3.png#pic_center)

![](https://i-blog.csdnimg.cn/blog_migrate/a16858a49b2331c63e7af8b944bab750.png#pic_center)

![](https://i-blog.csdnimg.cn/blog_migrate/85fd7fa00e720143dd0c87df66334c2b.png#pic_center)

到此，我们的服务器购买成功！

### 四台机器安装 docker

和我们单机安装一样

技巧： xshell 直接同步操作，省时间！

*   [Docker 的安装](https://blog.csdn.net/fanjianhai/article/details/107860159)

### Swarm 集群搭建

*   [工作机制](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/)
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/ee712765bc001f47b09fc9067f6a144c.png#pic_center)
    

```
docker swarm init --help
 
ip addr # 获取自己的ip（用内网的不要流量）
 
[root@iZ2ze58v8acnlxsnjoulk5Z ~]# docker swarm init --advertise-addr 172.16.250.97
Swarm initialized: current node (otdyxbk2ffbogdqq1kigysj1d) is now a manager.
 
To add a worker to this swarm, run the following command:
 
    docker swarm join --token SWMTKN-1-3vovnwb5pkkno2i3u2a42yrxc1dk51zxvto5hrm4asgn37syfn-0xkrprkuyyhrx7cidg381pdir 172.16.250.97:2377
 
To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
 
```

初始化结点`docker swarm init`

docker swarm join 加入一个结点！

```
# 获取令牌
docker swarm join-token manager
docker swarm join-token worker
```

```
[root@iZ2ze58v8acnlxsnjoulk6Z ~]# docker swarm join --token SWMTKN-1-3vovnwb5pkkno2i3u2a42yrxc1dk51zxvto5hrm4asgn37syfn-0xkrprkuyyhrx7cidg381pdir 172.16.250.97:2377
This node joined a swarm as a worker.
 
```

把后面的结点都搭建进去

![](https://i-blog.csdnimg.cn/blog_migrate/dc8f75c7fa4adf892a555837c833aa24.png#pic_center)

100 台！

1.  生成主节点 init
2.  加入（管理者，worker）

### Raft 协议

双主双从：假设一个结点挂了！其他结点是否可以用！

Raft 协议：保证大多数结点存活才可以使用，只要 > 1, 集群至少大于 3 台！

实验：

1、将 docker1 机器停止。宕机！双主，另外一个结点也不能使用了！

![](https://i-blog.csdnimg.cn/blog_migrate/4b6521c76d1c0e7b4d935bc221dadbf5.png#pic_center)

1.  可以将其他结点离开`docker swarm leave`

![](https://i-blog.csdnimg.cn/blog_migrate/32137eae0fa1fad15b8b66173265b139.png#pic_center)

1.  worker 就是工作的，管理结点操作！ 3 台结点设置为了管理结点。
2.  [Docker swarm 集群增加节点](https://www.cnblogs.com/zoujiaojiao/p/10886262.html)

十分简单：集群，可用！ 3 个主节点。 > 1 台管理结点存活！

Raft 协议：保证大多数结点存活，才可以使用，高可用！

### 体会

弹性、扩缩容！集群！

以后告别 docker run！

docker-compose up！启动一个项目。单机！

集群： swarm `docker-service`

k8s service

容器 => 服务！

容器 => 服务！ => 副本！

redis => 10 个副本！（同时开启 10 个 redis 容器）

体验：创建服务、动态扩容服务、动态更新服务

![](https://i-blog.csdnimg.cn/blog_migrate/74ee5637064f5401b27d0d7c1ace628c.png#pic_center)

*   灰度发布（金丝雀发布）
    
    *   [编程浪子的博客](https://www.cnblogs.com/apanly/)
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/bb499f6cfe7e0897df36041f0a962cf8.png#pic_center)
        

```
docker run 容器启动！ 不具有扩缩容器
docker service 服务！ 具有扩缩容器，滚动更新！
```

查看服务

![](https://i-blog.csdnimg.cn/blog_migrate/0238dd4d2a45ae44bda5cf74bccd7e97.png#pic_center)

动态扩缩容

```
[root@iZ2ze58v8acnlxsnjoulk5Z ~]# docker service update --replicas 3 my-nginx
1/3: running   [==================================================>] 
1/3: running   [==================================================>] 
2/3: running   [==================================================>] 
3/3: running   [==================================================>] 
verify: Service converged 
 
 
[root@iZ2ze58v8acnlxsnjoulk5Z ~]# docker service scale my-nginx=5
my-nginx scaled to 5
overall progress: 3 out of 5 tasks 
overall progress: 3 out of 5 tasks 
overall progress: 3 out of 5 tasks 
overall progress: 5 out of 5 tasks 
1/5: running   [==================================================>] 
2/5: running   [==================================================>] 
3/5: running   [==================================================>] 
4/5: running   [==================================================>] 
5/5: running   [==================================================>] 
verify: Service converged 
 
 
[root@iZ2ze58v8acnlxsnjoulk5Z ~]# docker service scale my-nginx=1
my-nginx scaled to 1
overall progress: 1 out of 1 tasks 
1/1: running   [==================================================>] 
verify: Service converged 
 
```

移出！`docker service rm`

docker swarm 其实并不难

只要会搭建集群、会启动服务、动态管理容器就可以了！

### 概念的总结

**swarm**

集群的管理和编号，docker 可以初始化一个 swarm 集群，其他结点可以加入。（管理，工作者）

**Node**

就是一个 docker 结点，多个结点就组成了一个网络集群（管理、工作者）

**Service**

任务，可以在管理结点或者工作结点来运行。核心，用户访问。

**Task**

容器内的命令、细节任务！

![](https://i-blog.csdnimg.cn/blog_migrate/b024a9091dd60b6b9ce05de45ef17ee1.png#pic_center)

> service

![](https://i-blog.csdnimg.cn/blog_migrate/233bc55d018e57f858f6a4b127432f5e.png#pic_center)

命令 -> 管理 -> api -> 调度 -> 工作结点（创建 Task 容器维护创建！）

> 服务副本和全局服务

![](https://i-blog.csdnimg.cn/blog_migrate/df6e38353504bcc7a3731a7ecdb33fac.png#pic_center)

调整 service 以什么方式运行

```
--mode string                        
Service mode (replicated or global) (default "replicated")
 
docker service create --mode replicated --name mytom tomcat:7 默认的
docker service create --mode global  --name haha alpine ping www.baidu.com
```

拓展： 网络模式 "PublishMode":"ingress"

Swarm:

Overlay:

ingress: 特殊的 Overlay 网络！负载均衡的功能！ipvs vip！

```
[root@iZ2ze58v8acnlxsnjoulk5Z ~]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
74cecd37149f        bridge              bridge              local
168d35c86dd5        docker_gwbridge     bridge              local
2b8f4eb9c2e5        host                host                local
dmddfc14n7r3        ingress             overlay             swarm
8e0f5f648e69        none                null                local
 
 
[root@iZ2ze58v8acnlxsnjoulk5Z ~]# docker network inspect ingress
[
    {
        "Name": "ingress",
        "Id": "dmddfc14n7r3vms5vgw0k5eay",
        "Created": "2020-08-17T10:29:07.002315919+08:00",
        "Scope": "swarm",
        "Driver": "overlay",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "10.0.0.0/24",
                    "Gateway": "10.0.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": true,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "ingress-sbox": {
                "Name": "ingress-endpoint",
                "EndpointID": "9d6ec47ec8309eb209f4ff714fbe728abe9d88f9f1cc7e96e9da5ebd95adb1c4",
                "MacAddress": "02:42:0a:00:00:02",
                "IPv4Address": "10.0.0.2/24",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.driver.overlay.vxlanid_list": "4096"
        },
        "Labels": {},
        "Peers": [
            {
                "Name": "cea454a89163",
                "IP": "172.16.250.96"
            },
            {
                "Name": "899a05b64e09",
                "IP": "172.16.250.99"
            },
            {
                "Name": "81d65a0e8c03",
                "IP": "172.16.250.97"
            },
            {
                "Name": "36b31096f7e2",
                "IP": "172.16.250.98"
            }
        ]
    }
]
 
```

### 其他命令学习方式

*   Docker Stack

```
docker-compose 单机部署项目
docker stack 集群部署
 
# 单机
docker-compose up -d wordpress.yaml
# 集群
docker stack deploy wordpress.yaml
```

*   Docker Secret

```
安全！配置密码！证书！
 
[root@iZ2ze58v8acnlxsnjoulk5Z ~]# docker secret --help
 
Usage:  docker secret COMMAND
 
Manage Docker secrets
 
Commands:
  create      Create a secret from a file or STDIN as content
  inspect     Display detailed information on one or more secrets
  ls          List secrets
  rm          Remove one or more secrets
```

*   Docker Config

```
配置！
[root@iZ2ze58v8acnlxsnjoulk5Z ~]# docker config --help
 
Usage:  docker config COMMAND
 
Manage Docker configs
 
Commands:
  create      Create a config from a file or STDIN
  inspect     Display detailed information on one or more configs
  ls          List configs
  rm          Remove one or more configs
 
```

### 拓展到 k8s

**云原生时代**

[Go 语言](https://so.csdn.net/so/search?q=Go%E8%AF%AD%E8%A8%80&spm=1001.2101.3001.7020)！必须掌握！ Java Go！

并发语言！

B 语言，C 语言的创始人。Unix 创始人 VB

go`指针`