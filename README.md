# java-nginx
使用 docker-compose 编排 java web apps 和 nginx 容器，支持多应用

## 目录说明
- nginx/logs/, 虚拟主机日志，如果有多个应用，使用子目录区分
- nginx/conf.d/, 虚拟主机配置，多应用同上
- webapps/, 应用 jar|war 包目录，多应用同上
- webapps/{$app_name}，应用目录

## 镜像说明
- nginx，官方 nginx:1.15，使用 docker-compose.yml 管理编排容器
- $app_name，基于 java:8 的自定义镜像，每个应用有自己的 Dockerfile 文件，同时使用 docker-compose.yml 管理编排容器
- nginx 和 java web apps 镜像分开管理（nginx 一个容器，每个 java web app 自己一个容器）

## docker-compose.yml
- 如果需要时间同步，将 localtime 备注开启（macos 无效，需要替换为对应的文件）
- 如果默认网络冲突，需要配置网络，将 networks 相关备注开启

## nginx、java 配置优化
> nginx、java 部分配置优化如下，请根据机器配置自行调整，相关文件及目录已挂载
- nginx.conf，client_max_body_size 1024m，大文件上传
- nginx.conf，worker_processes 4
- nginx.conf，gzip 相关已开启
- nginx.conf，log 日志重定向
- java，使用 -Djava.security.egd=file:/dev/./urandom 选项，优化随机数产生效率，加快 app 启动

## webapps，应用 Dockerfile、docker-compose.yml 生成器
> 根据包文件名 \*.war|jar 自动生成 docker 相关配置文件
```shell
// todo: shell
```

## 使用
1. 安装 docker-ce
```shell
yum update -y
yum install -y yum-utils device-mapper-persistent-data lvm2

yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install -y docker-ce
```

2. 启动 docker
```shell
# centos7+
systemctl enable docker    # 开机自启
systemctl start docker     # 启动

docker -v
```

3. 安装 docker-compose
```shell
curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

4. 启动 java-nginx
```shell
# 1.启动 java web app
#   - cd 到 wepapps 目录，新建 ${appName} 目录，拉取 ${appName} jar|war 包文件
#   - 使用生成器脚本，生成 Dockerfile、docker-compose.yml
#   - 启动 ${appName} 容器
docker-compose up -d --build

# 多个 apps 重复步骤 1

# 2.启动 nginx
#   - 新增 nginx/conf.d/${appName}.conf，配置反向代理虚拟主机
#   - 新增 ningx/logs/${appName}，应用 nginx 日志目录
#   - 启动 nginx 容器
docker-compose up -d --build
```
