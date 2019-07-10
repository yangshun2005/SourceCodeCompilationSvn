# SourceCodeCompilationSvn
# 源码编译安装svn-1.9.10

### 安装OS
yum install wget -y 

yum -y groupinstall "Development tools"
yum -y install expat-devel pcre pcre-devel openssl-devel

#### apr install
wget https://mirrors.aliyun.com/apache/apr/apr-1.6.5.tar.gz 
tar zxf apr-1.6.5.tar.gz
cd apr-1.6.5
./configure --prefix=/usr/local/apr
make && make install

#### apr-util install
wget https://mirrors.aliyun.com/apache/apr/apr-util-1.6.1.tar.gz
tar zxf apr-util-1.6.1.tar.gz
cd apr-util-1.6.1
./configure --prefix=/usr/apr-util --with-apr=/usr/local/apr
make && make install

#### zlib install
wget http://www.zlib.net/zlib-1.2.11.tar.gz
tar zxf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure 
make &&make install

#### sqlite install
wget http://www.sqlite.com/2017/sqlite-autoconf-3210000.tar.gz
tar zxf sqlite-autoconf-3210000.tar.gz
cd sqlite-autoconf-3210000
./configure
make && make install

#### 本范围可根据自己需要选择安装与否
```
# 安装openssl1.0.1u
wget https://www.openssl.org/source/old/1.0.1/openssl-1.0.1u.tar.gz
wget https://www.openssl.org/source/old/1.0.2/openssl-1.0.2l.tar.gz
tar zxf openssl1.0.1u.tar.gz
cd openssl1.0.1u
./config --prefix=/usr/local/openssl -fPIC no-gost /*这里的参数一定要添加，不然后面编译http后会出现问题*/
make depend
make install

# 安装scons-3.0.1
#编译安装
　　wget https://nchc.dl.sourceforge.net/project/scons/scons/3.0.1/scons-3.0.1.tar.gz
　　mkdir scons
　　tar zxf scons-local-3.0.1.tar.gz -C /usr/svnpackage/scons
　　cd scons
　　python scons.py install 
#rpm安装
　　wget https://nchc.dl.sourceforge.net/project/scons/scons/3.0.0/scons-3.0.0-1.noarch.rpm
　　rpm -ivh scons-3.0.0-1.noarch.rpm


# 安装serf
wget https://mirrors.aliyun.com/apache/serf/serf-1.3.9.tar.bz2
tar jxf serf-1.3.9.tar.bz2
cd serf-1.3.9
scons PREFIX=/usr/local/serlf APR=/usr/local/apr apr-util=/usr/local/apr-util OPENSSL=/usr/lcoal/openssl
/*巨坑：此处会提示报错 File "/usr/svnpackage/serf-1.3.9/SConstruct", line 186
print 'Warning: Used unknown variables:', ', '.join(unknown.keys())
SyntaxError: invalid syntax
这里可以把/usr/svnpackage/serf-1.3.9/SConstruct内的185,186行注释掉，然后在安装*/
scons install
#安装完成后，将serf的lib库追加到动态链接库
echo "/usr/local/serf/lib" >> /etc/ld.so.conf
ldconfig -v


# 安装httpd-2.4.28
wget https://mirrors.aliyun.com/apache/httpd/httpd-2.4.28.tar.gz
tar zxf httpd-2.4.28.tar.gz
cd httpd-2.4.28
./configure --prefix=/usr/local/apache --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util --enable-so --enable-dav --enable-maintainer-mode --enable-rewrite --enable-ssl --with-ssl=/usr/local/openssl
make && make install
#设置http自启动
cp /usr/local/apache/bin/apachectl /etc/init.d/httpd
vim /etc/init.d/httpd
#在#!/bin/sh的下面加入
#chkconfig:2345 85 35
#设置httpd开机自启动
chkconfig httpd on
#检查确认，2345级别为on
chkconfig --list httpd
#添加环境变量
vim /etc/profile.d/svn_path
export HTTPD_HOME=/usr/local/apache2/bin
export PATH=$HTTPD_HOME:$PATH
#设置生效：
source /etc/profile

```

#### svn install
wget https://mirrors.aliyun.com/apache/subversion/subversion-1.9.10.tar.gz
tar zxf subversion-1.9.10.tar.gz
cd subversion-1.9.10
./configure --prefix=/usr/local/subversion_1.9.10 --with-apr=/usr/local/apr --with-apr-util=/usr/apr-util  --with-zlib=/usr/local/zlib
make
sudo make install
make install-tools  # 此步骤是安装svn-tools的


export PATH=/usr/local/subversion_1.9.7/bin:$PATH

### 至此已经搞定了全部svn源码编译安装
`配置svn等相关方法可以自行google`


svnauthz-validate常用于检查svn文件；可使用方法如下：
`$ svnauthz-validate /srv/svn/conf/authz`



