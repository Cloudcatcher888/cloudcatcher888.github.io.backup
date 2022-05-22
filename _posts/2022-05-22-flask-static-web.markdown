---
layout: post
title:  "Flask-Static-Web"
date:   2022-05-22
categories: jekyll update
---
# 使用flask搭建静态网页，支持js html css和各种图片pdf以及文件下载（转载）

参考腾讯机器上cn-t\*\*p-c\*\*d-web目录下的实例


[ ugmp1343](https://user.open-open.com/u/139915) 5年前发布 | 16K 次阅读 [Web服务器](https://www.open-open.com/lib/tag/web-fuwuqi.html) [Flask](https://www.open-open.com/lib/tag/flask.html) [Python开发](https://www.open-open.com/lib/tag/python-kaifa.html)

这段时间新的项目，大部分都是动态的HTML5搭建的，需要在手机端测试适配问题，因此需要在本地搭建一个Web服务器，用于手机访问，但是可怜的网络下载100多M的XAMPP始终下不了，忽然灵机一动，以前学的Flask不是自带一个测试用的Web服务器，刚好可以用来做一个简单的静态Web服务器。

首先需要安装Python环境，可以 官网 去下载，然后next，next安装完成。

最新的Mac OS Sierra系统安装的Python没有自带 pip ，需要使用命令 sudo easy_install pip 手动安装 pip 。使用 sudo pip install Flask 安装好Flask框架，因为只是用来做一个简单的Web服务器，所以暂时不考虑使用 virtualenv 开发环境。

创建项目目录如下：

```
WebServer ├── static ├── WebServer.py
```

static 目录就是我们需要存放静态HTML以及资源文件， WebServer.py 就是我们开启服务器的文件, 代码如下：

```python
from flask import Flask

app = Flask(__name__)

@app.route('/<path:path>')
def hello_world(path):
    return app.send_static_file(path)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port='5000')
 
```

host='0.0.0.0' 表示Flask可以进行外网访问， port='5000' 为访问端口为5000，将你需要访问的静态文件放入到 static 目录中，然后在在命令行中用cd切换到 WebServer.py 的目录下，运行命令 python WebServer.py 启动服务器，然后可以在浏览器中输入:

```python
http://ip地址:port端口/静态文件Path
```

比如 http://192.168.1.104:5000/web/index.html ，就可以在局域内进行访问了。不过每次都复制文件到 static 目录中是比较麻烦的事情，我们可以使用 ln 命令创建Web项目文件夹的软链接到 static 目录中，命令为 ln -s 项目文件夹 static目录 。 建立软链接后，只需要命令启动服务器，就可以在浏览器中输入地址查看效果。

