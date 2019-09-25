# 后台部署文档

[TOC]

### 环境需求

 - docker ^18.06
 - docker-compose ^1.20

### 部署步骤

#### 预执行脚本

```shell
sudo sysctl -w vm.max_map_count=262144
sudo mkdir /data
sudo chmod 777 /data
# ES_MASTER_PATH 
sudo mkdir -p /data/master
sudo chmod g+rws /data/master
# PG_PATH
sudo mkdir -p /data/pg
# REDIS PATH
sudo mkdir -p /data/redis
# MINIO_PATH
sudo mkdir -p /data/minio
# LOGS_PATH
sudo mkdir -p /data/nitrogen_log
```

#### 拷贝配置文件

从默认配置文件模版拷贝一份出来使用

```shell
make cp-backend-config
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
|APP_ID|无|skylark的对接参数|
|APP_SECRET|无|skylark的对接参数|
|APP_DOMAIN|无|skylark的对接参数|
|TOKEN|无|应用请求校验|

#### 登陆docker私库

```shell
docker login [私库地址]
```

#### 启动后台

```shell
make start-backend
```