# MySQL 5.7安装

## rpm地址:

wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.23-1.el7.x86_64.rpm-bundle.tar

解压目录

```shell
# 解压在/usr/local/mysql目录下编译
tar xvf mysql-5.7.23-1.el7.x86_64.rpm-bundle.tar -C /usr/local/mysql
```

## 查看是否安装了MySQL

```shell
rpm -qa |grep -i mysql
```

## 安装依赖组件

在我进行安装`msql-community-server-xxx`的时候出现了下面的问题

```shell
[root@localhost software]# rpm -ivh mysql-community-server-5.7.19-1.el7.x86_64.rpm 
warning: mysql-community-server-5.7.19-1.el7.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
error: Failed dependencies:
        /usr/bin/perl is needed by mysql-community-server-5.7.19-1.el7.x86_64
        libaio.so.1()(64bit) is needed by mysql-community-server-5.7.19-1.el7.x86_64
        libaio.so.1(LIBAIO_0.1)(64bit) is needed by mysql-community-server-5.7.19-1.el7.x86_64
        libaio.so.1(LIBAIO_0.4)(64bit) is needed by mysql-community-server-5.7.19-1.el7.x86_64
        net-tools is needed by mysql-community-server-5.7.19-1.el7.x86_64
        perl(Getopt::Long) is needed by mysql-community-server-5.7.19-1.el7.x86_64
        perl(strict) is needed by mysql-community-server-5.7.19-1.el7.x86_64
[root@localhost software]# 1234567891011
```

由上面的错误可以看出我们需要安装相应的依赖 
\1. `libaio` 
\2. `net-tools` 
\3. `perl`

- 安装依赖

```shell
yum -y install libaio
yum -y net-tools
yum -y perl
```

### 安装mysql组件

经过上面的解压操作，我们得到了很多`rpm`文件。但是我们不需要这么多，我们只需要安装一下四个组件就可以了：

> mysql-community-common-5.7.19-1.el7.x86_64.rpm 
> mysql-community-libs-5.7.19-1.el7.x86_64.rpm 
> mysql-community-client-5.7.19-1.el7.x86_64.rpm 
> mysql-community-server-5.7.19-1.el7.x86_64.rpm

因为具有**依赖关系**，所以我们需要按顺序执行。 
用`rpm -ivh 文件名`就能安装相应的组件。 
在执行server的时候，需要依赖安装一些工具组件，已经在上文有说明了

- 安装命令

```shell
rpm -ivh mysql-community-common-5.7.23-1.el7.x86_64.rpm
rpm -ivh mysql-community-libs-5.7.23-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-5.7.23-1.el7.x86_64.rpm
rpm -ivh mysql-community-server-5.7.23-1.el7.x86_64.rpm
```



安装MySQL组件时出现的依赖错误

```shell
error: Failed dependencies:
mysql-community-common(x86-64) >= 5.7.9 is needed by mysql-community-libs-5.7.19-1.el7.x86_64
mariadb-libs is obsoleted by mysql-community-libs-5.7.19-1.el7.x86_64
```


## 卸载mariadb-libs时出现的依赖错误
```shell
error: Failed dependencies:
libmysqlclient.so.18()(64bit) is needed by (installed) postfix-2:2.10.1-6.el7.x86_64
libmysqlclient.so.18(libmysqlclient_18)(64bit) is needed by (installed) postfix-2:2.10.1-6.el7.x86_64
```



## 卸载mariadb-libs

### 先查看是否安装了mariadb-libs

```shell
rpm -qa |grep -i mariadb-libs
```

- ### 开始卸载,不能卸载全程,只输入文件头就行,卸载完执行find命令查看残留文件夹,使用rm删除残留


```shell
yum remove mariadb-libs

find / -name mariadb-libs

rm -rf 文件路径

```

## 初次启动MySQL

- ### MySQL 5.7之后默认会有随机密码,初次启动需要先查询默认密码

```shell
[root@01 yum.repos.d]# grep 'temporary password' /var/log/mysqld.log 
2017-03-12T12:25:43.090102Z 1 [Note] A temporary password is generated for root@localhost: h6&%pgp/+BBl
```



## MySQL修改默认密码

```shell
set global validate_password_policy=0;

set global validate_password_mixed_case_count=0;

set global validate_password_number_count=3;

set global validate_password_special_char_count=0;

set global validate_password_length=3;

set password = password('root')
```



## 查看MySQL是否启动
```shell
service mysqld status
```

### 启动mysql
```shell
service mysqld start
```

### 停止mysql
```shell
service mysqld stop
```

### 重启mysql
```shell
service mysqld restart
```



# Redis安装

- ### redis 4.0.10版本下载

wget http://download.redis.io/releases/redis-4.0.10.tar.gz



- ### 需要安装tcl依赖库.

```shell
yum install tcl
```



## 配置



- step1:解压

  > ```shell
  > tar xvf redis-4.0.10.tar.gz
  > ```

- step2:移动，放到/usr/local⽬录下

  > ```shell
  > mv ./redis-4.0.10 /usr/local/redis/
  > ```

- step3:进⼊redis⽬录

  > ```shell
  > cd /usr/local/redis/
  > ```

- step4:生成

  > ```shell
  > make
  > ```

- step5:测试

  > ```shell
  > make test
  > ```

- step6:安装

  > ```shell
  > make install
  > ```

- step7:安装完成后，进入目录/usr/local/bin中查看

  > ```shell
  > cd /usr/local/bin
  > ls -all
  > ```

  > redis-server redis服务器
  >
  > redis-cli redis命令行客户端
  >
  > redis-benchmark redis性能测试工具
  >
  > redis-check-aof AOF文件修复工具
  >
  > redis-check-rdb RDB文件检索工具

- step8:配置⽂件，移动到/etc/⽬录下

- 配置⽂件⽬录为/usr/local/redis/redis.conf

  > ```shell
  > cp /usr/local/redis/redis.conf /etc/redis/
  > ```



- ### make test编译错误

```shell
cd src && make test
make[1]: Entering directory `/usr/local/redis-4.0.10/src'
    CC Makefile.dep
make[1]: Leaving directory `/usr/local/redis-4.0.10/src'
make[1]: Entering directory `/usr/local/redis-4.0.10/src'
You need tcl 8.5 or newer in order to run the Redis test
make[1]: *** [test] Error 1
make[1]: Leaving directory `/usr/local/redis-4.0.10/src'
make: *** [test] Error 2
```

### 

- Redis的配置信息在/etc/redis/redis.conf下。

- 查看

  > ```shell
  > vim /etc/redis/redis.conf
  > # 关闭服务器
  > redis-cli shutdown
  > # 执行修改后的redis.conf
  > redis-server /etc/redis/redis.conf
  > ```

## 核心配置选项

- 绑定ip：如果需要远程访问，可将此⾏注释，或绑定⼀个真实ip

  > bind 127.0.0.1

- 端⼝，默认为6379

  > port 6379

- 是否以守护进程运⾏

  - 如果以守护进程运⾏，则不会在命令⾏阻塞，类似于服务
  - 如果以⾮守护进程运⾏，则当前终端被阻塞
  - 设置为yes表示守护进程，设置为no表示⾮守护进程
  - 推荐设置为yes

  > daemonize yes

- 数据⽂件

  > dbfilename dump.rdb

- 数据⽂件存储路径

  > dir /var/lib/redis

- ⽇志⽂件

  > logfile "/var/log/redis/redis-server.log"

- 数据库，默认有16个

  > database 16

- 主从复制，类似于双机备份。

  > slaveof

## 服务器自启动配置

```shell
sudo cp /usr/local/redis/redis.conf /etc/redis
sudo cp /etc/redis/redis.conf /etc/redis/6379.conf

 sudo cp /usr/local/redis/utils/redis_init_script /etc/init.d/redisd
 update-rc.d redisd defaults
 service redisd start
```



# python3.7安装





## 卸载系统默认python(可选)

```shell
# 删除现有所有Python,强制删除已安装程序及其关联
rpm -qa|grep python|xargs rpm -ev --allmatches --nodeps 
#删除所有残余文件, xargs，允许你对输出执行其他某些命令
whereis python |xargs rm -frv 
#验证删除，返回无结果
whereis python
```

## 安装Python



```shell
# 更新yum
yum update
# 安装开发工具	contos版本
yum groupinstall "Development Tools"
# 安装依赖包		 contos版本
yum -y install libffi-devel zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel zlib*
# 安装依赖包		 Ubuntu版本
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev

# 下载地址,此版本为python3.7.0
wget https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tar.xz
# 解压
tar xvf Python-3.7.0.tgz.xz
# 新建python3文件夹
mkdir /usr/local/python3
# 进入安装目录
cd Python-3.7.0
# 更改编译后安装路径
./configure --prefix=/usr/local/python3 --enable-optimizations
# 编译
make
make install
# 更改默认python软连接
mv /usr/bin/python python27
ln -s /usr/local/python3/bin/python3.7 /usr/bin/python
# 更改默认pip软连接
mv /usr/bin/pip pip27
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip

# sudo -H pip xxx信息
The directory '/home/taoabsilent/.cache/pip/http' or its parent directory is not owned by the current user and the cache has been disabled. Please check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.

sudo chown root ~/.cache/pip/http


# 修改yum第一行"#!/usr/bin/python",改为"#!/usr/bin/python27"
# 因将默认python改为python27,所以第一行改为python27
vim /usr/bin/yum
# 安装软件提示错误后,更改此文件第一行为python27
vim /usr/libexec/urlgrabber-ext-down


# python版本之间兼容性不太好，使得2.X版本与3.0版本之间存在语法不一致问题。
# 更改默认python后yum会出现语法解释错误。
File "/usr/bin/yum", line 30
except KeyboardInterrupt, e:
                       ^
SyntaxError: invalid syntax

# 安装软件时提示错误
Downloading packages:
  File "/usr/libexec/urlgrabber-ext-down", line 28
    except OSError, e:
                  ^
SyntaxError: invalid syntax
```



# Virtualenv安装

```shell
pip install virtualenv
pip install virtualenvwrapper
# 创建目录
mkdir ~/.virtualenvs
vim ~/.bashrc
# find / -name virtualenv 查找文件位置,更改下方代码路径
mv /usr/local/python3/bin/virtualenv /usr/local/bin/virtualenv
# 添加下面3行代码
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/local/python/bin/python3
source /usr/local/python/bin/virtualenvwrapper.sh

# 执行
source ~/.bashrc
```





# Nginx安装

```shell
# 下载ngins1.15.3版本
wget http://nginx.org/download/nginx-1.15.12.tar.gz
# 安装Nginx前，需要安装以下三个依赖包：openssl, zlib, pcre,在安装ngins包
# 按照顺序安装,安装前先查看系统中是否有自带的依赖库
openssl version
# 安装python已经安装3个依赖库的deel了,系统又自带3个依赖库,可以直接安装
tar xvf nginx-1.15.3.tar.gz
./configure
make
make install
cd /usr/local/nginx/sbin/
# 查看是否安装成功
./nginx -t
# 若出现下方信息表示安装成功
nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful
# 在sbin目录下启动nginx
./nginx


# **********配置**********
cd /usr/local/nginx/conf/
vim nginx.conf
# 
vim /usr/local/nginx/conf/nginx.conf
# 启动nginx
/usr/local/nginx/sbin/nginx
# 停止nginx
/usr/local/nginx/sbin/nginx -s stop



location ~ \.php$ {
        root           html;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME /scripts$fastcgi_script_name;
        include        fastcgi_params;
    }

改为

location ~ \.php$ {
        root           html;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME /usr/local/nginx/html/$fastcgi_script_name;
        include        fastcgi_params;
    }


# **********安装版**********
# 安装 openssl	官网地址:https://www.openssl.org/source/
wget https://www.openssl.org/source/openssl-1.1.1.tar.gz
tar xvf openssl-1.1.1.tar.gz
cd openssl-1.1.1
./config 
make
make install
# 安装 zlib	官网地址:http://www.zlib.net/
wget http://www.zlib.net/zlib-1.2.11.tar.xz
tar xvf zlib-1.2.11.tar.xz
cd zlib-1.2.11
./configure 
make
make install
# 安装pcre	官网地址:http://www.pcre.org/
# pcre目前有2个版本,pcre为97年发布的版本,目前只维护不更新,pcre2为15年发布,新功能都在这个版本
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.42.tar.gz
tar xvf pcre-8.42.tar.gz
cd pcre-8.42
./configure 
make
make install


# **********升级版本**********
# openssl
wget https://www.openssl.org/source/openssl-1.1.1.tar.gz
tar xvf openssl-1.1.1.tar.gz
cd openssl-1.1.1
./config shared zlib
make
makeinstall
mv /usr/bin/openssl /usr/bin/openssl.bak
mv /usr/include/openssl /usr/include/openssl.bak
ln -s /usr/local/bin/openssl /usr/bin/openssl
ln -s /usr/local/include/openssl /usr/include/openssl
find / -name "libssl*"
echo "/usr/local/lib64/" >> /etc/ld.so.conf
ldconfig
openssl version -a

```



```shell
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev

sudo apt-get install libgdbm-dev liblzma-dev libreadline-dev tcl-dev tk-dev dpkg-dev
```

The directory '/home/taoabsilent/.cache/pip/http' or its parent directory is not owned by the current user and the cache has been disabled. Please check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
The directory '/home/taoabsilent/.cache/pip' or its parent directory is not owned by the current user and caching wheels has been disabled. check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.