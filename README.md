# first-php-yii2
我的php项目

# 安装php环境
  1. 下载地址http://www.php.net/downloads.php；默认不是window的；点击Windows downloads 链接；non-thread-safe 非线程安全 与IIS 搭配环境；不要iis随便下，一般thread-safe线程安全版本，下载zip和Debug Pack（调式）
  2. 解压zip到目录，增加一个path路径为php的，把php.ini-* 修改一个成php.ini
  3. 下载xdebug 插件模块https://xdebug.org/docs/install；win版本在https://xdebug.org/download.php；下载对应的版本dll，放在D:\soft\php-7.3.0-nts-Win32-VC15-x64\ext目录下
    1.在php.ini增加调试
    extension_dir = "ext"（搜索这个，并删除前面;符号的注解；如果目录不是C:\php 需要放开注释）
    zend_extension=php_xdebug-2.7.0beta1-7.3-vc15-nts-x86_64.dll（dll文件名称）
    [XDebug]
    如果启用远程调试添加如下的
    xdebug.remote_enable = 1
    xdebug.remote_autostart = 1
    xdebug.remote_host= localhost
    xdebug.remote_port = 9000
    最后重新执行php -v就能看到输出with Xdebug v2.* 的字样了
# 安装vscode 的插件
  1. php-intellisense 和配合上面XDebug的php-debug插件
  2. 配置php：文件--->首选项--->设置；然后在用户/工作区（只这针对该项目（项目下会生成settings.json文件））的搜索框输入【php】，然后找到如下两个属性，点击在setting.json中编辑 或者 进去每项前面的编辑，把左边的设置到右边的{}中去。最后保存生效
    1. "php.validate.executablePath": "D:/soft/php-7.3.0-nts-Win32-VC15-x64/php.exe"
    2. "php.executablePath": "D:/soft/php-7.3.0-nts-Win32-VC15-x64/php.exe"
  3. 点击debug图标ctrl+shift+D 打开debug 。如果没有设置过，设置图标按钮上会有个红点。点击。根据提示选择php。会自动生成launch.json 文件。默认配置就好
# php 环境配置
  1. php-cgi -b 127.0.0.1:9003 -c C:\php\php.ini （9000端口好像不能被nginx发现）ctrl+c关闭;-c C:\php\php.ini不加也行。默认就是，如果有特殊配置请加;(强制关闭：taskkill /F /IM php-cgi.exe > nul)
  2. 下载nginx http://nginx.org/en/download.html ，解压到目录。cd 进入该目录下。对于无法关闭nginx时，可以执行taskkill /fi "imagename eq nginx.EXE" /f强行关闭。本项目start.bat用来启动php和nginx的脚步，stop反之；
    1. 修改nginx 目录下的conf目录编辑nginx.conf 做以下配置修改
      a. 找到location / { 把下面的root   html;修改为root   D:/workspace/vscode;   （如果php项目包含除php之外文件
      b. 找到location ~ \.php$ {  把前面注释去掉。没有就新增如下内容
        location ~ \.php$ {
            root           D:/workspace/vscode;
            fastcgi_pass   127.0.0.1:9003;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
  3. 切换到nginx解压迷路运行，nginx常用命令：start nginx.exe 可以后台运行nginx。 nginx.exe -s reload 重新加载配置;nginx.exe -s stop  //暂停;nginx.exe -s quit   //退出nginx；（强制关闭nginx：taskkill /F /IM nginx.exe > nul）
# 调式
  启动nginx.exe  ,然后启动php-cgi -b 127.0.0.1:9003 ，最后打开vscode的dubug。访问http://localhost/first-php-yii2/test.php就能调试debug了
  关于windows下端口监听查看：netstat -ano | findstr "8001"    查看端口8001被哪个进程占用；由下图可以看出，被进程为3736的占用
  通过进程号查看进程 命令：tasklist | findstr "3736" 查看进程号为3736对应的进程
  结束该进程  taskkill /f /t /im java.exe

# 安装yii2
  1. 安装教程https://getcomposer.org/download/ 通过过composer，我们可以使用大量的第三方库，而无需自己造轮子。linux和mac安装curl -sS https://getcomposer.org/installer | php 然后复制sudo mv composer.phar /usr/local/bin/composer
    命令安装：切换到php的根目录，修改php.ini 搜索"ext"去掉前面的; 搜索openssl 注释前面的;mbstring 模块也放开
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
    php -r "if (hash_file('sha384', 'composer-setup.php') === '93b54496392c062774670ac18b134c3b3a95e5a5e5c8f1a9f115f203b75bf9a129d5daa8ba6a13e2cc8a1da0806388a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
    php composer-setup.php
    php -r "unlink('composer-setup.php');"
    操作执行结束，会在当前目录下生成composer.phar文件。在Linux里，composer.phar是可执行程序。我们可以使用php composer.phar update执行更新操作。
  2. （也可以不加 使用 php composer.phar -v）添加composer命令。安装完新增composer.bat文件 添加@php "%~dp0composer.phar" %*最后保存；composer -V 查看版本；-h帮助文档（也可composer command --help） -v详细信息；composer self-update 更新composer到最新版本；具体参考https://docs.phpcomposer.com/03-cli.html
    1. 安装包 具体参考https://www.cnblogs.com/52fhy/p/5246013.html
    中国镜像配置[composer config]/[php composer.phar config] -g repo.packagist composer https://packagist.phpcomposer.com g表示全局
    指定版本 composer require monolog/monolog修改为composer require "monolog/monolog:1.2.*"（>，>=，<，<=，!=，~,-, ||）  composer update monolog/monolog
    移除某个包composer remove monolog/monolog 加-vvv可以输出详细信息
    搜索 php composer.phar search -name  完全匹配
    创建项目 php composer.phar create-project doctrine/orm path 2.2.* --repository-url: 提供一个自定义的储存库来搜索包，这将被用来代替 packagist.org
  3. 安装Yii（composer 可以跨目录，）官网https://www.yiiframework.com/doc/guide/2.0/zh-cn/start-installation
    可选：安装完 Composer，如果需要指定Asset版本运行下面的命令来安装 Composer Asset 插件php composer.phar global require "fxp/composer-asset-plugin:^1.2.0"
    国内最好切换源composer config -g repo.packagist composer https://packagist.phpcomposer.com   不然可能会造成安装失败
    安装基本的应用程序模板composer create-project [--prefer-dist --stability=dev] yiisoft/yii2-app-basic basic 或者高级版php composer.phar create-project yiisoft/yii2-app-advanced first-yii2-app（项目路径或者名称（就是当前目录））
    进入项目目录first-yii2-app 执行.\init 会提示选择开发还是生产。根据序号输入回车，然后有提示选择yes。最后初始化完成。
    win下执行.\yii.bat serve --docroot="frontend/web/" （linux 环境验证安装（内置PHP Web服务器）php yii serve 或者 php yii serve --port=8888） 浏览器http://localhost:8080/

# vscode安装yii2 插件