# PHP本地开发环境说明

## 准备
开始前，需要确保本地已经安装 docker。然后先将 docker 开发环境的脚本 git clone 到本地：
```shell
git git@github.com:xiaqiancheng/myDocker.git
cd myDocker
```

docker目录结构如下：
```shell
myDocker
    ├── files
    │   ├── mysql
    │   ├── openresty
    │   ├── php72
    │   ├── redis
    │   ├── .gitignore
    │   ├── docker-composer.yml
    │   ├── env-example
    ├── logs
    └── README.md
        .gitignore
```

> 第一次启动时，需要通过 docker pull 拉去镜像到本地，所以需要等待一段时间

## 配置安装
### 1、配置 `.env`,重命名`env-example`为`.env`
### 2、构建镜像
进入files目录，执行：`docker-compose build`
### 3、启动容器
执行: `docker-compose up -d`
