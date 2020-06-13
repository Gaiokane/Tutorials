# Tutorials
记录各种杂七杂八稀奇古怪五花八门的软件？教程

<span id="home">目录</span>

* [1. IIS配置反向代理](#1)
  * [1.1 准备工作](#1.1)
  * [1.2 开始配置IIS反向代理](#1.2)

* [2. Nginx配置多站点访问](#2)

<h1 id="1">1. IIS配置反向代理</h1>

<h2 id="1.1">1.1 准备工作</h2>

> <!-- 安装[requestRouter_amd64.msi](files\1\requestRouter_amd64.msi "点击下载")、[rewrite_amd64_zh-CN.msi](files\1\rewrite_amd64_zh-CN.msi "点击下载") -->
> 
> 安装<a href="files\1\requestRouter_amd64.msi" target="_blank" title="点击下载">requestRouter_amd64.msi</a>、<a href="files\1\rewrite_amd64_zh-CN.msi" target="_blank" title="点击下载">rewrite_amd64_zh-CN.msi</a>
> 
> 安装步骤都是一路默认下一步

<h2 id="1.2">1.2 开始配置IIS反向代理</h2>

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