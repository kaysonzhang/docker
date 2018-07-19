# docker
<p>Centos7安装Docker Engine</p>

<p>yum安装</p>
<p>1.安装依赖</p>
<p>yum install -y yum-utils device-mapper-persistent-data lvm2</p>

<p>2.添加Docker的yum源</p>

<p>$ sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'</p>
<p>复制以下代码执行</p>
<p>[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/7/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF

<p>3.安装Docker软件包</p>

<p>$ sudo yum install docker-engine</p>
<p>4.设置Docker服务开机自启</p>

<p>$ sudo systemctl enable docker.service</p>
<p>5.启动Docker服务</p>

<p>$ sudo systemctl start docker</p>
<p>6.验证Docker是否安装成功</p>

<p>$ sudo docker run --rm hello-world</p></p>

<p>脚本安装Docker Engine</p>

<p>1.更新yum</p>

<p>$ sudo yum update</p>
<p>2.运行Docker安装脚本</p>

<p>$ curl -fsSL https://get.docker.com/ | sh</p>
<p>这个脚本将会添加docker.repo并且安装Docker</p>

<p>3.设置Docker服务开机自启</p>

<p>$ sudo systemctl enable docker.service</p>
<p>4.启动Docker服务</p>

<p>$ sudo systemctl start docker</p>
<p>5.验证Docker是否安装成功</p>

<p>$ sudo docker run --rm hello-world</p>
