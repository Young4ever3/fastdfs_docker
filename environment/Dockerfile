FROM ubuntu:xenial
MAINTAINER topsale@vip.qq.com

# 更新数据源
WORKDIR /etc/apt
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse' > sources.list
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse' >> sources.list
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse' >> sources.list
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse' >> sources.list
RUN apt-get update

# 安装依赖
RUN apt-get install git make gcc libpcre3-dev zlib1g-dev  build-essential --assume-yes

# 复制工具包
ADD nginx-1.15.4.tar.gz /usr/local/src/

RUN mkdir -p /root/fastdfs

RUN mkdir /root/fastdfs/tracker   \
  &&  mkdir /root/fastdfs/storage   \
  &&  cd /usr/local/src/  \
  &&  git clone https://github.com/happyfish100/libfastcommon.git --depth 1 \
  &&  cd libfastcommon/   \
  &&  ./make.sh && ./make.sh install  \
  &&  cd ../  \
  &&  git clone https://github.com/happyfish100/fastdfs.git --depth 1 \
  &&  cd fastdfs/   \
  &&  ./make.sh && ./make.sh install  \
  &&  cd ../  \
  &&  git clone https://github.com/happyfish100/fastdfs-nginx-module.git --depth 1 \
  &&  cd nginx-1.15.4/  \
  &&  ./configure --add-module=/usr/local/src/fastdfs-nginx-module/src/   \
  &&  make && make install

# 配置 FastDFS 跟踪器
ADD tracker.conf /etc/fdfs

# 配置 FastDFS 存储
ADD storage.conf /etc/fdfs

# 配置 FastDFS 客户端
ADD client.conf /etc/fdfs

ADD mod_fastdfs.conf /etc/fdfs

WORKDIR /usr/local/src/fastdfs/conf
RUN cp http.conf mime.types /etc/fdfs/

# 配置 Nginx
ADD nginx.conf /usr/local/nginx/conf


COPY entrypoint.sh /usr/local/bin/
RUN   chmod +x /usr/local/bin/entrypoint.sh
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

WORKDIR /
EXPOSE 8888
CMD ["/bin/bash"]