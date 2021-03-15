**安装**

```shell
brew install supervisor
```

**配置解读**

supervisord.conf

```
[inet_http_server]  # 启用TCP server
port=127.0.0.1:9001

[supervisorctl]  # 客户端控制台
serverurl=http://127.0.0.1:9001  # 告诉控制台serverurl，用于连接supervisor服务

[include]  # 扩展的配置文件
files=/usr/local/etc/supervisor.d/*.ini
```

supervisor.d/self_strong.ini

```
[program:self_strong]  # 定义一个program名字，用于supervisor管理进程
environment=PYTHONPATH=/Users/exocr/Env/web_py3/bin/python
directory=/Users/exocr/Desktop/python-code/self_strong
command=gunicorn --workers=3 server:app --bind 127.0.0.1:8000
autostart=true
startsecs=5
autorestart=true
startretries=3
```

**shell**

`supervisord` - 启动supervisor

`supervisorctl`

```txt
> reread  # 读取有更新（增加）的配置文件，不会启动新添加的程序，也不会重启任何程序
> reload  # 载入最新的配置文件，停止原有的进程并按照新的配置启动
> update  # 重启配置文件修改过的程序，配置没有改动的进程不会收到影响而重启
> status  # 查看状态
> start   # 启动某进程
> stop    # 终止某进程
> shutdown  # 终止supervisor，执行后会把它下面管理的所有进程都关闭
```

`kill -9 pid` - 关闭supervisor