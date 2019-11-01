## Nginx 安装教程 

### 1. 卸载Nginx
##### service nginx stop
##### whereis nginx
##### rm -rf [所有相关文件]
##### yum remove nginx


### 2. 安装
###### 2.1 安装依赖包
     yum -y install gcc gcc-c++ make libtool zlib zlib-devel openssl openssl-devel pcre pcre-devel
 ##### 2.2 下载源码解压编译
     wget http://nginx.org/download/nginx-1.17.5.tar.gz
     tar -zxvf nginx-1.17.5.tar.gz
     cd nginx-1.17.5
     ./configure
     make && make install
     
 ## 3. 配置nginx的systemctl命令
    cd /usr/lib/systemd/system
    vi nginx.service
 ##### 3.1 复制粘贴以下
    [Unit]
    Description=The nginx HTTP and reverse proxy server
    After=network.target remote-fs.target nss-lookup.target
    
    [Service]
    Type=forking
    PIDFile=/usr/local/nginx/logs/nginx.pid
    ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
    ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
    ExecReload=/usr/local/nginx/sbin/nginx -s reload
    ExecStop=/usr/local/nginx/sbin/nginx -s stop
    ExecQuit=/usr/local/nginx/sbin/nginx -s quit
    
    [Install]
    WantedBy=multi-user.target
 ##### 3.2 重启服务
    systemctl daemon-reload
 ##### 4 启动nginx,如无法启动检查端口是否被占用
    systemctl start nginx.service