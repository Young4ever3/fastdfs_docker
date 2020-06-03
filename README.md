# fastdfs docker镜像部署

## 镜像参考链接
> 初始镜像链接: https://www.jianshu.com/p/1038b4d95912  

> fastdfs 项目地址：https://github.com/happyfish100/fastdfs/wiki

## 启动容器
```
docker-compose up -d
```
## 进入容器
```
docker exec -it fastdfs /bin/bash
```
## 测试上传文件
```
/usr/bin/fdfs_upload_file /etc/fdfs/client.conf  文件名
```
## 访问端口
```
8888
```