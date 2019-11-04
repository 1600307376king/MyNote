## Centos7安装mysql5.6教程

##### 下载mysql的repo源
    wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
##### 安装mysql-community-release-el7-5.noarch.rpm包
    rpm -ivh mysql-community-release-el7-5.noarch.rpm
##### 安装mysql
    yum install mysql-server
##### 加入开机启动
    systemctl enable mysqld
##### 启动mysql服务进程
    systemctl start mysqld
##### 重置密码
    mysql_secure_installation
##### mysql -u root -p
    mysql -u root -p