---
title: PicGo + Github 图床配置踩坑过程
tags:
  - github图床
  - 遇坑记录
abbrlink: e7b33cbf
date: 2020-05-24 22:06:18
---
> PicGo: 用于快速上传图片并获取图片URL链接的工具，支持拖拽图片上传，简单易用，配合github做图床可以获取url，可作为markdown的图片路径。
<!-- more -->
## Github图床

1.安装了Node.js,版本大于8
2.拥有一个github账号，创建一个**公有仓库**
3.访问https://github.com/settings/tokens， 点击<code>Generate new token</code>，勾选repo，生成一个token
4.配置picgo，仓库名必须为<code>用户名/仓库</code>

下图为官网配置的截图
{% asset_img picgo-config.jpg 官网配置 %}

### 遇到的坑
&emsp;&emsp;使用PicGo上传图片的时候，总是显示红色进度条，但是和官网比照了好几遍配置，又没发现什么错误，可是就是上传不了。于是谷歌搜了一下遇坑同盟，跟着她们的解决方法解决了一遍。
  &emsp;&emsp;1.[先关闭sever再重启](https://blog.csdn.net/weixin_42932905/article/details/106268022?fps=1&locationNum=2?utm_medium=distribute.pc_relevant.none-task-blog-baidujs-2)
  &emsp;&emsp;2.[校准电脑时钟](https://blog.csdn.net/liuergo/article/details/102630912)
  &emsp;&emsp;3.[文件名里不能有 ‘+’ 号](https://blog.csdn.net/twodogbanana/article/details/95609760)
  &emsp;&emsp;结果还是不行。打开了PicGo设置 - 设置日志文件 - 点击设置，想看看日志文件究竟是什么问题。因为我尝试上传了很多遍，所以报错原因有两个。
  &emsp;&emsp;①[PicGo ERROR] RequestError: Error: read ECONNRESET
  &emsp;&emsp;②[PicGo ERROR] StatusCodeError: 404 - {"message":"Not  Found","documentation_url":"https://developer.github.com/v3/repos/contents/#create-or-update-a-file"}
  &emsp;&emsp;感觉离胜利更近了一步！打开了picgo的github issue继续寻找同盟。在同个报错下看到开发者了接近崩溃的呐喊
   {% asset_img issue.jpg  %}
   {% asset_img issue2.jpg  %}
  惭愧的我又看了一遍官网，逛了一遍faq，可是依旧发现自己是按着正确步骤的。于是我改了仓库名字，因为一开始名字是和PicGo同名，以为是同名造成的bug，结果还是不行。十分烦躁的情况下做最后一次尝试，重新配置了token，才发现之前的token一直是never used的状态。
{% asset_img never-used.png  %}
希望渺小的情况下上传了图片，成功了！！所以应该就是token哪里出了问题吧
  {% asset_img picgo-upload.jpg  %}  
  <center>{% asset_img last.jpg  %}</center>
  
