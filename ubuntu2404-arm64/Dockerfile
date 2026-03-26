# 以最新的Ubuntu镜像为模板
FROM ubuntu:24.04

# 设定环境变量，防止交互卡住
ENV DEBIAN_FRONTEND=noninteractive
ARG TZ=Asia/Shanghai

# 设置时区
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 将本目录下的sources.list作为容器的一个文件
ADD sources.list /root/sources.list
# 使用国内Ubuntu源，更新快
RUN mv /etc/apt/sources.list /etc/apt/sources.list_bak && \
    cp /root/sources.list /etc/apt/sources.list

# 安装常用工具并清理
RUN apt update && apt install -y vim git sudo openssh-server && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

# 警告：使用固定密码仅用于开发环境，生产环境请使用更安全的方式
RUN echo 'root:123456' | chpasswd && \
    adduser zhumouren --gecos "" --disabled-password --shell /bin/bash && \
    echo 'zhumouren:123456' | chpasswd && \
    usermod -aG sudo zhumouren && \
    mkdir -p /var/run/sshd

# 开放22端口
EXPOSE 22

# 设置自启动命令
CMD ["/usr/sbin/sshd", "-D"]