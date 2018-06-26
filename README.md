# Docker + Nginx + MySql + PHP 开发环境

## 目录结构

    ├── data 容器数据文件夹
    │   ├── mysql 数据库文件
    │   ├── redis 数据库文件
    │   └── ... 更多服务容器数据文件
    ├── log 日志文件夹 
    │   ├── mysql 日志文件
    │   ├── redis 日志文件
    │   ├── nginx 日志文件
    │   ├── php   日志文件
    │   └── ... 更多服务容器日志文件
    ├── sercie 服务文件夹
    │   ├── mysql 服务
    │   │   └── conf 配置文件
    │   ├── redis 服务
    │   │   └── conf 配置文件
    │   ├── php 服务
    │   │   ├—— php7.2
    │   │   └── ... 更多php版本
    │   ├── nginx 服务
    │   │   ├── conf 配置文件
    │   │   └───── conf.d 虚拟机配置文件 
    │   └── ... 更多服务容器日志文件
    ├── wwwroot 项目目录
    │   ├── site1
    │   ├── site2
    │   └── ... 更多项目
    ├── docker-compose.yml 服务自动化部署

## 常用 Docker 命令

### 1、镜像相关管理

#### 查看镜像列表
    
    docker image ls

#### 删除所有镜像

    docker rmi $(docker images -q)

`删除镜像之前请确保没有容器在使用当前镜像`

#### 删除指定格式的所有镜像

    docker rm $(docker ps -qf status=exited)
    
#### 删除所有未打 tag 的镜像

    docker rmi $(docker images -q | awk '/^<none>/ { print $3 }')   

### 2、容器相关管理

#### 查看容器列表
    
    # 查看运行中的容器
    docker ps
    # 查看所有容器
    docker ps -a
    
#### 启动、停止、重启容器

    # 启动一个或多个已经被停止的容器
    docker start [OPTIONS] CONTAINER [CONTAINER...]
    # 启动一个容器并进入交互模式
    docker start -i CONTAINER [CONTAINER...] 
    # 停止一个或多个正在运行的容器
    docker stop [OPTIONS] CONTAINER [CONTAINER...] 
    # 重启一个或多个容器，-t 在杀死容器之前等待停止秒数
    docker restart -t CONTAINER [CONTAINER...] 

#### 删除容器

    # 删除一个或多个容器
    docker rm [OPTIONS] CONTAINER [CONTAINER...]
    # 参数解释
    Options:
      -f, --force     强制删除正在运行的容器（使用SIGKILL）
      -l, --link      删除指定的链接
      -v, --volumes   删除与容器关联的卷
      
#### 删除所有未运行的容器

    docker rm $(docker ps -a -q)  

### 3、网络管理

#### 查看网络列表

    docker network ls 

#### 其他网络管理命令

    docker network connect	将容器连接到网络
    docker network create	创建网络
    docker network disconnect	从网络断开容器
    docker network inspect	显示一个或多个网络的详细信息
    docker network ls	列出网络
    docker network prune	删除所有未使用的网络
    docker network rm	删除一个或多个网络
    
### 4、docker-compose 命令

    docker-compose -f 指定 yml 文件，默认使用 docker-compose.yml 作为默认文件
    docker-compose -f docker-compose.yml up    启动容器