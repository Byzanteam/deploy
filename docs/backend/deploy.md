# 后台部署文档

[TOC]

### 环境需求

 - docker ^18.06
 - docker-compose ^1.20

### 部署步骤

#### 预执行脚本

```shell
sudo sysctl -w vm.max_map_count=262144
# ES_MASTER_PATH 
sudo mkdir -p /data/master
sudo chmod g+rws /data/master
# PG_PATH
sudo mkdir -p /data/pg
# MINIO_PATH
sudo mkdir -p /data/minio
# LOGS_PATH
sudo mkdir -p /data/nitrogen_log
```

#### 修改配置文件

 1. 配置目录 `configs/backend/.env`中的环境变量 `STAGE`
 2. 将`configs/backend/app`下配置文件重命名

|阶段名|STAGE|文件名|
|:--:|:--:|:--:|
|默认|DEFAULT|api.default.yml|
|生产|PROD|api.prod.yml|
|测试|TEST|api.test.yml|

例如:

```
vi configs/backend/.env # 将STAGE设置为DEFAULT
cp api.STAGE.yml api.default.yml
```

#### 替换环境变量

|变量名|默认值|描述|
|:--:|:--:|:--:|
|REDIS_HOST|localhost|redis服务器地址|
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
make up-backend
```