## 安装podman
```
yum-yinstall podman
```

## 使用命令大多和docker一样，只是把docker改成domain



## podman-compose只支持python3.x以上,only depend on podman and Python3 and PyYAML
```
yum install gcc gcc-c++ libffi-devel openssl-devel python-setuptools vim wget make sqlite-devel zlib* -y
wget https://www.python.org/ftp/python/3.7.1/Python-3.7.1.tar.xz
tar xvf Python-3.7.1.tar.xz
cd Python-3.7.1
./configure --with-ssl
make
make install

用下面命令可以看到/usr/bin下是否存在python3
ls -al /usr/bin | grep python

将 /usr/local/bin/python3 软连接到 /usr/bin/python3
ln -s /usr/local/bin/python3 /usr/bin/python3

pip3 install --upgrade pip

```

## 安装podman-compose
```
pip3 install podman-compose
```
or
```
curl -o /usr/local/bin/podman-compose https://raw.githubusercontent.com/containers/podman-compose/devel/podman_compose.py

chmod +x /usr/local/bin/podman-compose
```

## 开启用户名称空间[https://github.com/containers/podman/blob/master/docs/tutorials/rootless_tutorial.md]
```
sysctl user.max_user_namespaces=15000
cat /proc/sys/user/max_user_namespaces
```