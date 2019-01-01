# first-php-yii2
我的php项目

# 安装php环境
  1. 下载地址http://www.php.net/downloads.php；默认不是window的；点击Windows downloads 链接；non-thread-safe 非线程安全 与IIS 搭配环境；不要iis随便下，一般thread-safe线程安全版本，下载zip和Debug Pack（调式）
  2. 解压zip到目录，增加一个path路径为php的，把php.ini-* 修改一个成php.ini
  3. 下载xdebug 插件模块https://xdebug.org/docs/install；win版本在https://xdebug.org/download.php；下载对应的版本dll，放在D:\soft\php-7.3.0-nts-Win32-VC15-x64\ext目录下
    1.在php.ini中启用远程调试
    [XDebug]
    zend_extension=php_xdebug-2.7.0beta1-7.3-vc15-nts-x86_64.dll（dll文件名称）
    如果启用远程调试添加如下的
    xdebug.remote_enable = 1
    xdebug.remote_autostart = 1
# 安装vscode 的插件
  1. php-intellisense 和配合上面XDebug的php-debug插件
