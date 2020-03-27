+ [Centos7安装Python3.7](#Centos7安装Python3.7)
+ [pip安装psycopy2报错问题解决方案](#pip安装psycopy2报错问题解决方案)


### Centos7安装Python3.7

+ 安装相关编译工具

```
yum -y groupinstall "Development tools"
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
yum install libffi-devel -y
yum install wget
```

+ 下载并解压安装包

```
cd /opt
wget https://www.python.org/ftp/python/3.7.2/Python-3.7.2.tar.xz
tar -xvJf  Python-3.7.2.tar.xz
```

+ 编译并安装

```
mkdir /usr/local/python3
cd Python-3.7.2
./configure --prefix=/usr/local/python3
make && make install
```

+ 创建软连接

```
ln -s /usr/local/python3/bin/python3 /usr/local/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/local/bin/pip3
```

+ 验证

```
python3 -V
pip3 -V
```

### pip安装psycopy2报错问题解决方案
```
pip3 install psycopy2 Error: pg_config executable not found.

解决方案1：
pip3 install psycopg2-binary

解决方案2：
yum install python-devel postgresql-devel
pip3 install psycopg2
```
