## 使用

```
docker-compose build
docker-compose up -d
```

访问：

- jellyfin: http://your-ip:8096
- aria2: http://your-ip

## 工具介绍

### 下载工具

- [docker-aria2-pro](https://github.com/P3TERX/docker-aria2-pro)
- [AriaNg](https://github.com/mayswind/AriaNg)

`AriaNg` 是一个静态 `html` 文件，所以通过 nginx 挂载进去就可以使用了。

### jellyfin

#### 中文电影名是乱码

Dockerfile-jellyfin-with-utf8

    FROM jellyfin/jellyfin

    COPY debian-10-apt-source /etc/apt/sources.list
    RUN apt-get update
    RUN apt-get install -y locales
    RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && locale-gen
    ENV LC_ALL en_US.UTF-8
    ENV LANG en_US.UTF-8
    ENV LANGUAGE en_US:en


#### jellyfin 的 DLNA 无法工作

发现局域网中的 DLNA 设备

安装 simpletr64

    pip install simpletr64

搜索局域网中的 DLNA 设备

    import simpletr64

    devices = simpletr64.Discover.discover()
    print(devices)

确保：

- 通过上面的代码能找到 `jellyfin DLNA`
- docker 中运行需要加上 `network_mode: host` 选项
- 确保该主机中的 `1900/udp` 没有被其它进程占用

注：

- jellyfin 中可以创建不同的用户，可以配置不同用户能查看的影片资源不同
