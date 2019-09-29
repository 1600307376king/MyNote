##Apache + flask windows环境搭建
####步骤 1 
* 下载对应版本 Apache mod_wsgi Python

* Apache,mod_wsgi和Python都必须用相同版本的C/C++编译器生成，要么都是32位的，要么都是64位的，不能混用。
 
* Apache和mod_wsgi 也必须选择相同位数相同VC编译版本（比如：都是x64 VC14编译）

* python flask Apache 下载安装步骤忽略

####步骤 2 
* 安装mod_wsgi 
    - 下载连接 https://www.lfd.uci.edu/~gohlke/pythonlibs/ 然后选择点击mod_wsgi
    
    - 将下载的文件后缀改为zip，解压出来，拷贝mod_wsgi.cp37-win_amd64.pyd 文件放到C:\Apache24\modules 目录，并改名为mod_wsgi.pyd
    
* 修改 httpd.conf 配置 例Apache配置文件：C:\Apache24\conf\httpd.conf
    - 搜索SRVROOT 并修改apache目录的路径
    
    - 加载mod_wsgi模块，在httpd.conf中找到LoadModule最后一行后面增加行 LoadModule wsgi_module modules/mod_wsgi.pyd
    
    - 修改httpd.conf配置，末尾增加内容：8090端口与apache 监听端口保持一致。"c:/web" 为项目路径，以下三个路径保持相同
    

        <VirtualHost *:8090 >
          ServerAdmin "0.0.0.0"
          DocumentRoot "c:/web"
        
        <Directory "c:/web">
          Require all granted
          Require host ip
          Allow from all
        </Directory>
          WSGIScriptAlias / c:/web/test.wsgi
        </VirtualHost>
    
    
   - 继续修改httpd.conf，找到一下内容启用他


        LoadModule access_compat_module modules/mod_access_compat.so #基于主机的组授权（名称或IP地址） httpd 2.x兼容的模块，
        LoadModule proxy_module modules/mod_proxy.so #apache的代理模块
        LoadModule proxy_http_module modules/mod_proxy_http.so #代理http和https请求
        LoadModule vhost_alias_module modules/mod_vhost_alias.so #虚拟主机动态配置
        LoadModule authz_host_module modules/mod_authz_host.so #基于主机的组授权
        Include conf/extra/httpd-vhosts.conf#启用虚拟主机配置
    
    
* 创建一个flask 项目 如命名为web
    
   - 新建文件test.wsgi
    
   - 把刚创建的web目录中的app.py 重命名为main.py 也可以不改，添加如下代码(示例)


        from flask import Flask
    
        app = Flask(__name__)
         
        @app.route('/')
        def hello_world():
            return 'Hello World!'
            
        @app.route("/index/")
        def foo():
            return "index page"
            
        
        @app.route("/login/")
        def login():
            return "login page"
         
        if __name__ == '__main__':
            app.run()
        
   - 添加test.wsgi代码 注意 application 不能改变，"C:/web"为项目路径，main 为main.py名称

 
        import sys
    
        sys.path.insert(0, "C:/web")
        
        from main import app
        
        application = app
    

#### 步骤 3
* 最后启动Apache/bin/目录下cmd 命令输入httpd， 浏览器输入localhost:8090 返回hello world就算配置成功