+ [Centos7安装Python3.7](#Centos7安装Python3.7)
+ [pip安装psycopy2报错问题解决方案](#pip安装psycopy2报错问题解决方案)


### Centos7安装Python3.7

+ 安装相关工具
```
yum install -y gcc openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel libffi-devel tk-devel wget curl-devel make
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

pip3更新：pip3 install --upgrade pip
```

+ 删除安装包
```
cd /opt
rm -rf Python*
```

+ shell
```
tee ./redis_deploy.sh <<-'EOF'
#!/bin/bash
yum install -y gcc openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel libffi-devel tk-devel wget curl-devel make
wget -P /opt https://www.python.org/ftp/python/3.8.2/Python-3.8.2.tar.xz
tar -xvJf /opt/Python-3.8.2.tar.xz -C /opt
mkdir /usr/local/python3
/opt/Python-3.8.2/configure --prefix=/usr/local/python3
make
make install
ln -s /usr/local/python3/bin/python3 /usr/local/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/local/bin/pip3
rm -rf /opt/Python*
EOF
sh ./redis_deploy.sh
rm -rf redis_deploy.sh
python3 -V
```
