## uWSGI 配置

#### 1 centos 7.2 环境下
##### 1.1 Flask 例子
    #!/usr/bin/python3
    from flask import Flask
    
    app = Flask(__name__)
    
    @app.route("/")
    def helloWorld():
        return "你好"
    
    if __name__ == '__main__':
        app.run(host='0.0.0.0', port=8080)
##### 1.2 创建uswgi.ini 运行 uwsgi --ini uwsgi.ini
    [uwsgi]
    socket = 127.0.0.1:8099 # 选择socket时，ip 与 nginx 对应
    wsgi-file = app.py
    callable = app  
    processes = 4
    threads = 2
    #daemonize = /var/www/pj/wsgi.log
    
##### 1.3 修改nginx 配置文件 nginx.conf 
    # 在server 内配置location
    location / {
        include uwsgi_params;
        uwsgi_pass 0.0.0.0:8099; # 与uwsgi.ini 文件内socket 端口对应
    }

