wget https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo -O /etc/yum.repos.d/docker-ce.repo

1 移除旧的安装包

yum remove docker \
docker-client \
docker-client-latest \
docker-common \
docker-latest \
docker-latest-logrotate \
docker-logrotate \
docker-selinux \
docker-engine-selinux \
docker-engine


yum makecache fast
yum install -y yum-utils device-mapper-persistent-data lvm2
yum -y install docker-ce

tee -a /etc/sysctl.conf <<-EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl -p

systemctl enable docker
systemctl start docker

#groupadd docker
#usermod -aG docker $USER



#  移除
yum -y remove docker-ce
rm -rf /var/lib/docker




部署 portainer

docker pull portainer/portainer
docker run -d -p 12456:9000 -v /var/run/docker.sock:/var/run/docker.sock -v /data/portainer:/data --name portainer --restart=always portainer/portainer
