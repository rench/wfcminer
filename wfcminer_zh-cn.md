# docker 安装

1. [Windows 10 安装docker](https://store.docker.com/editions/community/docker-ce-desktop-windows)
2. [Ubuntu 安装docker](https://store.docker.com/editions/community/docker-ce-server-ubuntu)
3. [Macos 安装docker](https://store.docker.com/editions/community/docker-ce-desktop-mac)
4. [其他安装](https://www.docker-cn.com/community-edition#/download)

# wificoin 节点运行
1. 拉取镜像到本地 `docker pull registry.cn-hangzhou.aliyuncs.com/wificoin-project/wificoin:latest`
2. 镜像重新命令 `docker tag registry.cn-hangzhou.aliyuncs.com/wificoin-project/wificoin:latest wificoin:latest`
3. 在本地(例如:`D:/wificoin`或者`/home/user/.wificoin`)创建`wificoin.conf`
```
listen=1
server=1
rpcport=9665
rpcuser=test
rpcpassword=admin
rpcallowip=0.0.0.0/0
```
4. 启动节点 `docker run -p 9665:9665 -p 9666:9666 -v D:/wificoin:/root/.wificoin -d --rm --name wificoin wificoin:latest`

- `-v` 进行本地主机和docker进行文件的挂载映射,用`:`分隔
- `D:/wificoin` 本机存放wificoin.conf文件的目录,节点启动后,会同步数据.
- `/root/.wificoin` 固定不变
- `-v D:/wificoin:/root/.wificoin` 就是把docker容器中的`/root/.wificoin`目录映射到本地的`D:/wificoin`,容器中对`/root/.wificoin`所有操作将直接生效于`D:/wificoin`.

> docker容器停止后,容器中所有新产生的运行时数据将丢失.所以挂载文件到本地是强烈建议的.

# wfcminer 矿机运行
- 拉取镜像到本地 `docker pull registry.cn-hangzhou.aliyuncs.com/wificoin-project/wfcminer:latest`
- 镜像重新命令 `docker tag registry.cn-hangzhou.aliyuncs.com/wificoin-project/wfcminer:latest wfcminer:latest`
- 启动节点 `docker run -d --rm --name wfcminer wfcminer:latest minerd -o http://192.168.1.123:9665 -u test -p admin  --no-getwork -a sha256d --coinbase-addr=wiijXST1RShBC4eAo56PqXoipX1RKFQ49o -t 2`
- `-o http://192.168.1.123:9665` 一般是主机(内网/公网)ip地址+端口
- `-t 2` 表示使用2个线程来跑
- `-u test -p admin` 来源于`wificoin.conf` 的`rpcuser`和`rpcpassword`

---

# docker 节点操作
- 查看当前运行的容器 `docker ps -a`
- 查看本地的镜像`docker images -a`
- 停止运行中的容器`docker stop xxx`, xxx 来自于`docker ps -a` 的第一列
- 删除已停止的容器`docker rm xxx`, xxx 来自于`docker ps -a` 的第一列
- 查看容器的日志`docker logs -f xxx`, xxx 来自于`docker ps -a` 的第一列