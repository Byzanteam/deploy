# 后台部署文档

[TOC]

### 环境需求

 - linux
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

#### 替换环境变量

根据以下描述替换 `configs/backend/.envs.conf` 中环境变量的值

|变量名|默认值|描述|是否必须|
|:--:|:--:|:--:|:--:|
|STAGE|prod|项目阶段(只有为prod才会使用sentry采集日志)|true|
|MQ_USER|guest|RABBITMQ用户名|true|
|MQ_PASS|guest|RABBITMQ密码|true|
|GEO_KEY|无|geo服务所需高德接口密钥|false|
|MINIO_ACCESS|无|minio access key|true|
|MINIO_SECRET|无|minio secret key|true|
|DB_PASSWORD|无|数据库密码|true|
|DB_NAME|无|数据库名称|true|
|SENTRY_DSN|无|sentry日志收集配置|false|
|SMS_ENABLE|true|是否启用短信服务|true|
|SMS_ACCESS_KEY_ID|无|短信服务id|true|
|SMS_ACCESS_KEY_SECRET|无|短信服务密钥|true|
|SMS_SIGN_NAME|无|短信服务签名|true|
|SMS_TEMPLATE_CODE|无|短信服务模版id|true|

#### 利用工具替换环境变量(可选)

1. 修改模版文件`envs.conf`

2. 执行命令批量替换配置文件

```shell
make replace-backend-config
```

#### 地址转经纬度功能配置(可选)

1. 修改`configs/backend/geo.json`内容

2. uncomment`configs/backend/docker-compose.yml`中的geo service

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
