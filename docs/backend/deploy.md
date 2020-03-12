# 后台部署文档

[TOC]

### 环境需求

 - docker ^18.06
 - docker-compose ^1.20

### 部署步骤

#### 预执行脚本

```shell
sudo mkdir /data
sudo chmod 777 /data
# PG_PATH
sudo mkdir -p /data/pg
# REDIS PATH
sudo mkdir -p /data/redis
# MINIO_PATH
sudo mkdir -p /data/minio
```

#### 下载部署文档

```shell
git clone https://github.com/Byzanteam/deploy.git
cd deploy
```

`以下所有命令均在deploy目录下执行`

#### 拷贝配置文件

从默认配置文件模版拷贝一份出来使用

```shell
make cp-backend-config
```

#### 登陆docker私库

```shell
docker login [私库地址]
```

#### 后台拉取镜像

`新开启一个终端后执行下面命令`

```shell
make pull-backend
```

#### 修改配置文件

 1. 配置目录 `configs/backend/.env`中的环境变量 `STAGE`
 2. 将`configs/backend/app`下配置文件重命名

|阶段名|STAGE|文件名|
|:--:|:--:|:--:|
|默认|DEFAULT|config.default.yml|
|生产|PROD|config.prod.yml|
|测试|TEST|config.test.yml|

例如:

```
vi configs/backend/.env # 将STAGE设置为DEFAULT
cp config.STAGE.yml config.default.yml
```

#### 替换环境变量

|变量名|默认值|描述|
|:--:|:--:|:--:|
|MQ_USER|guest|RABBITMQ用户名|
|MQ_PASS|guest|RABBITMQ密码|
|GEO_KEY|无|geo服务所需密钥|
|MINIO_ACCESS|无|minio access key|
|MINIO_SECRET|无|minio secret key|
|DB_PASSWORD|无|数据库密码|
|DB_NAME|无|数据库名称|

#### 利用工具替换环境变量(可选)

1. 修改模版文件`envs.conf`

2. 执行命令批量替换配置文件

```shell
make replace-backend-config
```

#### 启动后台

```shell
make start-backend
```

#### 停止后台服务

```shell
make stop-backend
```

#### 遇到问题

[帮助文档](https://github.com/Byzanteam/deploy/issues)
