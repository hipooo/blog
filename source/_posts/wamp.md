---
layout: post
title: "Windows下配置Apache+MySql+PHP开发环境"
date: 2016-02-16 20:00:00
comments: true
categories: 学无止境
tags:
    - windows
    - apache
    - php
    - mysql
---
PHP 集成开发环境有很多，在刚接触 PHP 的时候也是把各种集成包体验了一番，用别人配置好的集成包总感觉少点什么，所以最后还是决定自己来配置 WAMP 开发环境。

配置过程是在 `Windows7 x64` 下进行的，并且安装过了 `VC11` 运行库。

- MySql: http://dev.mysql.com/downloads/mysql/
- PHP: http://windows.php.net/download/

# Apache

#### 下载和安装

1. 打开下载页面：[Apache 下载地址](https://www.apachelounge.com/download/)
2. 下载 httpd-2.\*.\*-win64-VC11.zip
3. 解压缩下载的安装包到安装目录，如：`D:\PHPDev\Apache24`（路径中尽量不要包含中文字符和空格）

#### 修改配置项

1. 打开配置文件，默认位置：`D:\PHPDev\Apache24\conf\httpd.conf`
2. 修改相关配置项：
    ```bash
    #37 修改 ServerRoot 的路径：
    ServerRoot "D:/PHPDev/Apache24"

    #58 修改端口号（默认端口号 80，如果 80 端口被占用，可以修改端口号）：
    Listen 8080

    #242 修改 DocumentRoot 访问的主文件夹目录
    #Apache 默认的主文件夹目录是 D:\PHPDev\Apache24\htdocs，将其配置为自己新建的文件夹 Workspace(D:\Workspace)：
    DocumentRoot "D:/Workspace"
    <Directory "D:/Workspace">

    #276 修改入口文件配置项 DirectoryIndex
    DirectoryIndex index.php index.html index.htm

    #359 修改 ScriptAlias 的目录：
    ScriptAlias /cgi-bin/ "D:/PHPDev/Apache24/cgi-bin/"
    #375行
    <Directory "D:/PHPDev/Apache24/cgi-bin">
    ```
3. 测试是否成功：
    - 打开命令提示符，进入 `D:\PHPDev\Apache24\bin` 目录，输入 `httpd` 然后回车
        如果没有显示错误信息，基本可以确定配置项没问题。
    - 保持命令提示符窗口的打开状态，将 `D:\PHPDev\Apache24\htdocs` 目录下的 `index.html` 文件拷贝到 `D:\Workspace` 目录中
    - 用浏览器访问 http://localhost:8080/ 如果出现 `It works` 说明 Apache 已经正确配置并启动成功
4. 将 Apache 加入 Windows 服务中，并设置为开机启动：
    - 关闭之前的命令行提示符
    - 重新打开一个新的命令行提示符窗口，进入 `D:\PHPDev\Apache24\bin` 目录
    - 执行如下命令（Apache24 为服务名称，可自定义）：
        ```bash
        httpd.exe -k install -n "Apache24"
        ```
        提示成功后，可以在 Windows 服务中看到 Apache24 服务。
    - 手工启动 Apache24 服务
    卸载 Apache24 服务：`httpd.exe -k uninstall -n "Apache24"`
