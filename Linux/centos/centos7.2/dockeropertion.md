## docker 常见问题解决方法
## docker hub 账号 docker1996jjc
##### 1 修改docker存储目录
    docker info # 查看Docker Root Dir 
    vi /usr/lib/systemd/system/docker.service
    # 在ExecStart 内添加一下配置，注意不要有多余空格
    --graph=/home/docker \ 
    --storage-driver=overlay \
    systemctl daemon-reload
    systemctl restart docker 
    
##### 2 从宿主机复制文件至容器
    docker cp 文件名 容器ID:目标路径
    
##### 3 创建虚拟端口的容器 并拥有所有权限
    docker run -p 8080:80 -d --privileged 容器ID init
##### 4 将容器保存位镜像
    docker commit 容器ID 镜像名称
##### 5 将镜像保存为文件，将文件转化为镜像
    docker save -o 文件名 镜像ID
    docker load < 文件名
    