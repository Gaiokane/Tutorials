# Tutorials
记录各种杂七杂八稀奇古怪五花八门的软件？教程

<span id="home">目录</span>

* [1. IIS配置反向代理](#1)
  * [1.1 准备工作](#1-1)
  * [1.2 开始配置IIS反向代理](#1-2)

* [2. 将Nginx安装为Windows服务](#2)
  * [2.1 准备工作](#2-1)
  * [2.2 配置并安装Nginx服务](#2-2)

* [3. Nginx配置多站点访问](#3)
  * [3.1 准备工作](#3-1)
  * [3.2 配置多站点访问](#3-2)

* [4. WordPress插件](#4)

* [5. 内网IIS配置SSL自签证书（未完成）](#5)

* [6. jetbrains全家桶注册码](#6)

* [7. php、mysql 、nginx优化](#7)

* [8. Postman打开加载时间过长](#8)

* [9. 导出微信收藏](#9)

* [10. VM安装MacOS（Mojave 10.14）](#10)

* [11. 免费DDNS](#11)

<h1 id="1">1. IIS配置反向代理</h1>

<h2 id="1-1">1.1 准备工作</h2>

> <!-- 安装[requestRouter_amd64.msi](files\1\requestRouter_amd64.msi "点击下载")、[rewrite_amd64_zh-CN.msi](files\1\rewrite_amd64_zh-CN.msi "点击下载") -->
> 
> 安装<a href="files\1\requestRouter_amd64.msi" target="_blank" title="点击下载">requestRouter_amd64.msi</a>、<a href="files\1\rewrite_amd64_zh-CN.msi" target="_blank" title="点击下载">rewrite_amd64_zh-CN.msi</a>
> 
> 安装步骤都是一路默认下一步

<h2 id="1-2">1.2 开始配置IIS反向代理</h2>

> 此处以反向代理到百度为例
> 
> 1)打开IIS（试过IIS6.5？、IIS7、win10下IIS）
> 
> 2)双击中间IIS>Application Request Routing Cache
> 
> 3)点击右侧Proxy>Server Proxy Settings
> 
> 4)勾选Enable proxy>点击右侧应用
> 
> 5)展开左侧网站>点击需要反向代理的网站
> 
> 6)双击中间IIS>URL重写
> 
> 7)点击右侧添加规则
> 
> 8)入站规则>空白规则>确定
> 
> 9)名称：自定义
> 
> 10)匹配URL>模式：^(.*)
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;注：符合该正则则进行该配置处理
> 
> 11)条件>添加>
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;条件输入：{URL}
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;注：规则条件可参考[链接](https://shiyousan.com/post/635654920639643421 "新窗口访问")
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;模式：^/baidu/(.*)
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;注：匹配该正则则进行重写URL
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;确定
> 
> 12)操作>操作属性>
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;重写URL：https://www.baidu.com/s?wd={C:1}
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;注：可使用反向引用，如此处{C:0}就是在11)中点击测试模式>测试，匹配项的向后引用。向后引用可参考[链接](https://shiyousan.com/post/635648886502897428 "新窗口访问")
> 
> 13)点击右上角应用
> 
> 14)浏览器访问http://localhost/baidu/ip，相当于访问https://www.baidu.com/s?wd=ip

[返回目录](#home)

<h1 id="2">2. 将Nginx安装为Windows服务</h1>

<h2 id="2-1">2.1 准备工作</h2>

> 下载<a href="files\2\nginx-service.exe" target="_blank" title="点击下载">nginx-service.exe</a>、<a href="files\2\nginx-service.exe.config" target="_blank" title="点击下载">nginx-service.exe.config</a>、<a href="files\2\nginx-service.xml" target="_blank" title="点击下载">nginx-service.xml</a>
> 
> nginx-service.exe是通过下载[Windows Service Wrapper](http://repo.jenkins-ci.org/releases/com/sun/winsw/winsw/ "查看各版本")工具后重命名后得到，可选择合适的版本下载

<h2 id="2-2">2.2 配置并安装Nginx服务</h2>

> 将下载的nginx-service.exe、nginx-service.exe.config、nginx-service.xml放在Nginx根目录(C:\inetpub\nginx-1.18.0)
> 
> nginx-service.exe.config中内容：
> 
> ````
> <configuration>
> 	<startup>
> 		<supportedRuntime version="v2.0.50727" />
> 		<supportedRuntime version="v4.0" />
> 	</startup>
> 	<runtime>
> 		<generatePublisherEvidence enabled="false"/> 
> 	</runtime>
> </configuration>
> ````
> 
> nginx-service.xml中内容：(该文件中涉及Nginx相关路径自行更改)
> 
> ````
> <service>
> 	<id>Nginx 1.18</id>
> 	<name>Nginx Service 1.18</name>
> 	<description>High Performance Nginx Service</description>
> 	<logpath>C:\inetpub\nginx-1.18.0\logs</logpath>
> 	<log mode="roll-by-size">
> 		<sizeThreshold>10240</sizeThreshold>
> 		<keepFiles>8</keepFiles>
> 	</log>
> 	<executable>C:\inetpub\nginx-1.18.0\nginx.exe</executable>
> 	<startarguments>-p C:\inetpub\nginx-1.18.0</startarguments>
> 	<stopexecutable>C:\inetpub\nginx-1.18.0\nginx.exe</stopexecutable>
> 	<stoparguments>-p C:\inetpub\nginx-1.18.0 -s stop</stoparguments>
> </service>
> ````
> 
> 安装Nginx服务：
> 
> 在Nginx根目录中打开命令提示符(在Nginx根目录下按住Shift+鼠标右键，选择“在此处打开命令提示符”)
> 
>     nginx-service.exe install
> 
> 删除Nginx服务：
> 
>     sc delete "Nginx 1.18"
> 
> 注1：当服务名中间有空格时，需要用""
> 
> 注2：当服务正在运行时执行删除操作，cmd提示“[SC] DeleteService 成功”，服务列表中还在，服务停止后才会不显示在列表中

[返回目录](#home)

<h1 id="3">3. Nginx配置多站点访问</h1>

<h2 id="3-1">3.1 准备工作</h2>

> Nginx根目录：C:\inetpub\nginx-1.18.0
> 
> 在nginx-1.18.0\conf下新建文件夹vhosts

<h2 id="3-2">3.2 配置多站点访问</h2>

> 配置Nginx虚拟目录
> 
> 打开nginx-1.18.0\conf\nginx.conf
> 
> 将虚拟目录配置到http块末尾
> 
> ````
> http {
> 	······
> 	include ./conf/vhosts/*.conf;
> }
> ````
> 
> 创建web1配置文件
> 
> nginx-1.18.0\conf\vhosts\web1.conf
> 
> ````
> server {
>     listen       80;        # 监听端口
>     server_name  web1;      # 站点域名
> 
>     location / {
>         root   html/web1;   # 站点根目录
>         index  index.html;  # 默认文档
>     }
> }
> ````
> 
> 创建web1测试页面
> 
> > nginx-1.18.0\html\web1\index.html
> 
> ````
> <html>
> 	<head>
> 		<title>This is Web1</title>
> 	</head>
> 	<body>
> 		<h1>Web1</h1>
> 	</body>
> </html>
> ````
> 
> 创建web2配置文件
> 
> nginx-1.18.0\conf\vhosts\web2.conf
> 
> ````
> server {
>     listen       80;        # 监听端口
>     server_name  web2;      # 站点域名
> 
>     location / {
>         root   html/web2;   # 站点根目录
>         index  index.html;  # 默认文档
>     }
> }
> ````
> 
> 创建web2测试页面
> 
> nginx-1.18.0\html\web2\index.html
> 
> ````
> <html>
> 	<head>
> 		<title>This is web2</title>
> 	</head>
> 	<body>
> 		<h1>web2</h1>
> 	</body>
> </html>
> ````
> 
> 重启Nginx服务
> 
> 访问http://localhost/，对应文件路径为nginx-1.18.0\html\index.html
> 
> 访问http://web1/，对应文件路径为nginx-1.18.0\html\web1\index.html
> 
> &nbsp;&nbsp;&nbsp;&nbsp;注：需配置hosts文件，C:\Windows\System32\drivers\etc\hosts，增加127.0.0.1	web1
> 
> 访问http://web2/，对应文件路径为nginx-1.18.0\html\web2\index.html
> 
> &nbsp;&nbsp;&nbsp;&nbsp;注：需配置hosts文件，C:\Windows\System32\drivers\etc\hosts，增加127.0.0.1	web2
> 
> 参考配置文件：<a href="files\3\nginx.conf" target="_blank" title="点击下载">nginx.conf</a>、<a href="files\3\web1.conf" target="_blank" title="点击下载">web1.conf</a>、<a href="files\3\index.html" target="_blank" title="点击下载">index.html</a>

[返回目录](#home)

<h1 id="4">4. WordPress插件</h1>

> 1) Wordfence Security：安全性（登录两步验证、限制登录失败次数...）
> 
> 2) WP Statistics：站点分析统计工具
> 
> 3) Elementor：可视化页面编辑器
> 
> 4) UpdraftPlus Backup/Restore：站点备份恢复
> 
> 5) WP Super Cache：缓存
> 
> 6) Yoast SEO：SEO
> 
> 7) EWWW Image Optimizer：图像优化
> 
> 8) Modulobox：图片预览
> 
> 9) Real Media Library：媒体库管理
> 
> 10) Smart Slider 3：图片轮播插件

[返回目录](#home)

<h1 id="5">5. 内网IIS配置SSL自签证书</h1>

> 

[返回目录](#home)

<h1 id="6">6. jetbrains全家桶注册码</h1>

> [获得注册码](http://idea.medeming.com/ "获得注册码")
> 
> 微信公众号搜索"最美程序员"，进入公众号获取
> 
> [永久激活方案~](https://mp.weixin.qq.com/s/ayIzGQoIm_UCe-e0LdFhmw "永久激活方案~")
> 
> [下载地址1](http://idea.medeming.com/jets/ "下载地址1")
> 
> [下载地址2](http://idea.medeming.com/jihuoma/ "下载地址2")

[返回目录](#home)

<h1 id="7">7. php、mysql 、nginx优化</h1>

参考来源：https://bbs.vpser.net/m/?a=viewthread&tid=8914

> 修改/usr/local/php/etc/php-fpm.conf php 5.2调整：max_children的值
> 
> php 5.3以上版本调整：pm.min_spare_servers和pm.max_spare_servers的值适当增加
> 
> 最大值可以按内存xxMB/2/20 的整数来算，最小值可以按内存/2/40 的整数来算，可以少点或多大，可以自己调整运行看看。
> 
> ************************************
> 
> MySQL参数优化可以自己适当调整/etc/my.cnf 里的参数
> 
> key_buffer_size
> 
> table_open_cache
> 
> sort_buffer_size
> 
> read_buffer_size
> 
> myisam_sort_buffer_size
> 
> thread_cache_size
> 
> query_cache_size
> 
> tmp_table_size
> 
> innodb_buffer_pool_size
> 
> innodb_log_file_size
> 
> performance_schema_max_table_instances
> 
> 等，可以参考：https://github.com/licess/lnmp/blob/master/include/mysql.sh#L84 里面的内存设置
> 
> ***********************************
> 
> nginx可以调整 /usr/local/nginx/conf/nginx.conf 的worker_processes
> 
> Nginx作者说的：
> 
> 一般一个进程足够了，你可以把连接数设得很大。如果有SSL、gzip这些比较消耗CPU的工作，而且是多核CPU的话，可以设为和CPU的数量一样。或者要处理很多很多的小文件，而且文件总大小比内存大很多的时候，也可以把进程数增加，以充分利用IO带宽（主要似乎是IO操作有block）。
> 
> 现在大部分版本上也可以设置为：worker_processes auto; 自动调整

[返回目录](#home)

<h1 id="8">8. Postman打开加载时间过长</h1>

> 添加系统变量
> 
> POSTMAN_DISABLE_GPU = true

[返回目录](#home)

<h1 id="9">9. 导出微信收藏</h1>

## 1. 使用到的 系统/软件 版本信息

* MacOS（Mojave 10.14）虚拟机安装教程[点击这里](/files/4/vm%20install%20macos.pdf "新窗口访问")
* 微信（Mac版2.4.2）
* DB Browser for SQLite（3.12）
* lldb（1001.0.13.3）（使用时可以自动安装）
* python（2.7.10）（版本无所谓）

## 2.使用lldb调试获取数据库密码

````
1. 打开微信

2. 打开终端输入 lldb -p $(pgrep WeChat) 回车

注意：若没有安装过lldb会提示安装

3. br set -n sqlite3_key 回车

4. c 回车

5. 扫码登录微信

6. 回到终端输入 memory read --size 1 --format x --count 32 $rsi 回车

7. 应该会输出类似于如下的数据
   0x000000000000: 0xab 0xcd 0xef 0xab 0xcd 0xef 0xab 0xcd
   0x000000000008: 0xab 0xcd 0xef 0xab 0xcd 0xef 0xab 0xcd
   0x000000000010: 0xab 0xcd 0xef 0xab 0xcd 0xef 0xab 0xcd
   0x000000000018: 0xab 0xcd 0xef 0xab 0xcd 0xef 0xab 0xcd
   
8. 重新打开一个终端输入以下（3至6行自行替换）
   python
   source = """
   0x000000000000: 0xab 0xcd 0xef 0xab 0xcd 0xef 0xab 0xcd
   0x000000000008: 0xab 0xcd 0xef 0xab 0xcd 0xef 0xab 0xcd
   0x000000000010: 0xab 0xcd 0xef 0xab 0xcd 0xef 0xab 0xcd
   0x000000000018: 0xab 0xcd 0xef 0xab 0xcd 0xef 0xab 0xcd
   """
   key = '0x' + ''.join(i.partition(':')[2].replace('0x', '').replace(' ', '') for i in source.split('\n')[1:5])
   print(key)

9.  回车后返回的一串0x开头的66位字符串就是数据库密码
````

## 3. 打开数据库并查看数据

````
1. 打开终端输入以下
   ls -alh ~/Library/Containers/com.tencent.xinWeChat/Data/Library/Application\ Support/com.tencent.xinWeChat/*/*/Favorites/*.db

2. 回车后返回 收藏 数据库文件路径

3. 去到对应路径下双击favorites.db（确保已安装DB Browser for SQLite）

4. 在弹出的对话框中选择 原始密钥（raw key）、SQLCipher 3默认，在密码文本框中输入数据库密码，点击OK以连接

注意：要在Windows中打开.db文件，运行DB Browser for SQLite目录中的DB Browser for SQLCipher.exe

5. 收藏文章的标题（<pagetitle>）、链接（<link>）均在表FavoritesItemTable -> 字段xml中
````

微信还有其他本地数据库，如：

* Contact - wccontact_new2.db - 好友信息
* Group - group_new.db - 群聊和群成员信息
* Message - {msg_0.db - msg_9.db} - 聊天记录和公众号文章

## 4. 使用python清洗数据并导出

### 4.1 准备工作

使用DB Browser for SQLite打开数据库，点击任务栏中 工具>设置加密>OK，禁用数据库加密，以便之后使用python读取数据

此处以python3为例

该部分已转移至新项目，[继续阅读](https://github.com/Gaiokane/WeChat-DataBase-Export#4-1 "点击跳转")

———————————————————————————————————

参考文章：

https://www.lianxh.cn/news/d34f09cb214e0.html

https://blog.csdn.net/swinfans/article/details/88712593

https://www.v2ex.com/t/466053

[返回目录](#home)

<h1 id="10">10. VM安装MacOS（Mojave 10.14）</h1>

MacOS（Mojave 10.14）虚拟机安装教程[点击这里](/files/4/vm%20install%20macos.pdf "新窗口访问")

[返回目录](#home)

<h1 id="11">11. 免费DDNS</h1>

[Dynu](https://www.dynu.com/zh-CN/ "新窗口访问")

[返回目录](#home)