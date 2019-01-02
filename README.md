# first-php-yii2
我的php项目

# 安装php环境
  1. 下载地址http://www.php.net/downloads.php；默认不是window的；点击Windows downloads 链接；non-thread-safe 非线程安全 与IIS 搭配环境；不要iis随便下，一般thread-safe线程安全版本，下载zip和Debug Pack（调式）
  2. 解压zip到目录，增加一个path路径为php的，把php.ini-* 修改一个成php.ini
  3. 下载xdebug 插件模块https://xdebug.org/docs/install；win版本在https://xdebug.org/download.php；下载对应的版本dll，放在D:\soft\php-7.3.0-nts-Win32-VC15-x64\ext目录下
    1.在php.ini增加调试
    extension_dir = "ext"（搜索这个，并删除前面;符号的注解）
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
  1. php-cgi -b 127.0.0.1:9003 -c C:\php\php.ini （9000端口好像不能被nginx发现）ctrl+c关闭;-c C:\php\php.ini不加也行。默认就是，如果有特殊配置请加
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
  3. 切换到nginx解压迷路运行，nginx常用命令：start nginx.exe 可以后台运行nginx，不加start表示直接执行。 nginx.exe -s reload 重新加载配置;nginx.exe -s stop  //暂停;nginx.exe -s quit   //退出nginx
# 调式
  启动nginx.exe  ,然后启动php-cgi -b 127.0.0.1:9003 ，最后打开vscode的dubug。访问http://localhost/first-php-yii2/test.php就能调试debug了

