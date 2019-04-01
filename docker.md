# 启动docker

```dockerfile
docker run --name mysql -e MYSQL_ROOT_PASSWORD=root -d -p 3306:3306 daocloud.io/library/mysql:latest

docker run --name mysql -v /root/mysql:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=root --privileged=true -d -p 3306:3306 daocloud.io/library/mysql:latest
```

# 文件拷贝

## 从容器拷贝文件到宿主机

```dockerfile
docker cp 容器名：要拷贝的文件在容器里面的路径       要拷贝到宿主机的相应路径
```

## 从宿主机拷贝文件到容器

```dockerfile
docker cp 要拷贝的文件路径 容器名：要拷贝到容器里面对应的路径
```

