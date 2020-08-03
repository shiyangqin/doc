# 使用kubeadm部署Kubernetes

## 准备

+ 设置系统时区、同步时间（可选）

```shell
timedatectl set-timezone Asia/Shanghai
yum install -y ntp ntpdate
ntpdate asia.pool.ntp.org
/sbin/hwclock --systohc
```

+ 关闭防火墙

```shell
systemctl disable firewalld
systemctl stop firewalld
```

+ 禁用 swap

```shell
swapoff -a
sed -i 's/.*swap.*/#&/' /etc/fstab
```

+ 关闭 SELinux

```shell
setenforce 0
sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
```

+ 配置内核参数

```shell
cat > /etc/sysctl.d/k8s.conf <<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
```

## 所有节点安装docker

+ 安装docker

```shell
yum remove docker \
           docker-client \
           docker-client-latest \
           docker-common \
           docker-latest \
           docker-latest-logrotate \
           docker-logrotate \
           docker-engine
yum install -y yum-utils device-mapper-persistent-data lvm2
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
yum install -y docker-ce docker-ce-cli containerd.io
systemctl enable docker
systemctl start docker
```

+ 配置docker

修改registry-mirrors字段

```shell
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2",
  "storage-opts": [
    "overlay2.override_kernel_check=true"
  ]
}
EOF
systemctl daemon-reload
systemctl restart docker
```

## 安装kubelet kubeadm kubectl

node节点不需要安装kubectl

```shell
cat > /etc/yum.repos.d/kubernetes.repo <<EOF
[kubernetes]
name=Kubernetes
baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
    http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
yum -y install kubelet kubeadm kubectl
cat > /etc/sysconfig/kubelet <<EOF
KUBELET_EXTRA_ARGS=--fail-swap-on=false --cgroup-driver=systemd
EOF
systemctl enable kubelet
```

## Master节点init

如果需要初始化Master节点，请执行 kubeadm reset;

```shell
kubeadm init \
        --apiserver-advertise-address 0.0.0.0 \
        --apiserver-bind-port 6443 \
        --cert-dir /etc/kubernetes/pki \
        --control-plane-endpoint 10.255.175.41 \
        --image-repository registry.cn-hangzhou.aliyuncs.com/google_containers \
        --kubernetes-version 1.18.2 \
        --node-name 10.255.175.41 \
        --pod-network-cidr 10.11.0.0/16 \
        --service-cidr 10.20.0.0/16 \
        --service-dns-domain cluster.local \
        --upload-certs
```

说明：

```shell
--apiserver-advertise-address 0.0.0.0
# API 服务器所公布的其正在监听的 IP 地址,指定“0.0.0.0”以使用默认网络接口的地址
# 切记只可以是内网IP，不能是外网IP，如果有多网卡，可以使用此选项指定某个网卡

--apiserver-bind-port 6443
# API 服务器绑定的端口,默认 6443

--cert-dir /etc/kubernetes/pki
# 保存和存储证书的路径，默认值："/etc/kubernetes/pki"

--control-plane-endpoint 10.255.175.41
# 为控制平面指定一个稳定的 IP 地址或 DNS 名称

--image-repository registry.cn-hangzhou.aliyuncs.com/google_containers
# 选择用于拉取Control-plane的镜像的容器仓库，默认值："k8s.gcr.io"
# 因 Google被墙，这里选择国内仓库

--kubernetes-version 1.18.2
# 为Control-plane选择一个特定的 Kubernetes 版本， 默认值："stable-1"

--node-name master01
#  指定节点的名称,不指定的话为主机hostname，默认可以不指定

--pod-network-cidr 10.10.0.0/16
# 指定pod的IP地址范围

--service-cidr 10.20.0.0/16
# 指定Service的VIP地址范围

--service-dns-domain cluster.local
# 为Service另外指定域名，默认"cluster.local"

--upload-certs
# 将 Control-plane 证书上传到 kubeadm-certs Secret
```

init成功后，记录生成的最后部分内容，此内容需要之后执行。

```shell
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of the control-plane node running the following command on each as root:

  kubeadm join 10.255.175.41:6443 --token p35b1l.j8y37p6sf8quiv2c \
    --discovery-token-ca-cert-hash sha256:7e1f9fa0be0befe8f9b2cdc33fc2d933a9690967c353a31b38e43e2c949ef1c0 \
    --control-plane --certificate-key 1c0503d0dc047ae1efe5f680f289d70c9b1a89755d8a20c2e3df61e111c1a924

Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use
"kubeadm init phase upload-certs --upload-certs" to reload certs afterward.

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 10.255.175.41:6443 --token p35b1l.j8y37p6sf8quiv2c \
    --discovery-token-ca-cert-hash sha256:7e1f9fa0be0befe8f9b2cdc33fc2d933a9690967c353a31b38e43e2c949ef1c0

```

## master节点运行配置命令

master init 最后显示的信息中，直接粘贴复制即可

```shell
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## master节点安装网络插件

```shell
wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
kubectl apply -f kube-flannel.yml
```

## node节点加入Kubernetes集群

master init 最后显示的信息中，直接粘贴复制即可

可通过 --node-name 参数指定节点的名称

```shell
kubeadm join 10.255.175.41:6443 --token p35b1l.j8y37p6sf8quiv2c \
    --discovery-token-ca-cert-hash sha256:7e1f9fa0be0befe8f9b2cdc33fc2d933a9690967c353a31b38e43e2c949ef1c0
```

## 其他master节点加入Kubernetes集群

master init 最后显示的信息中，直接粘贴复制即可

可通过 --node-name 参数指定节点的名称

```shell
  kubeadm join 10.255.175.41:6443 --token p35b1l.j8y37p6sf8quiv2c \
    --discovery-token-ca-cert-hash sha256:7e1f9fa0be0befe8f9b2cdc33fc2d933a9690967c353a31b38e43e2c949ef1c0 \
    --control-plane --certificate-key 1c0503d0dc047ae1efe5f680f289d70c9b1a89755d8a20c2e3df61e111c1a924
```

## 查询所有节点

```shell
[root@master k8s]# kubectl get nodes
NAME            STATUS   ROLES    AGE     VERSION
10.255.175.41   Ready    master   3m24s   v1.18.5
10.255.175.90   Ready    <none>   31s     v1.18.5
10.255.175.97   Ready    <none>   14s     v1.18.5
```

## 卸载清理

```shell
kubeadm reset -f
modprobe -r ipip
lsmod
rm -rf ~/.kube/
rm -rf /etc/kubernetes/
rm -rf /etc/systemd/system/kubelet.service.d
rm -rf /etc/systemd/system/kubelet.service
rm -rf /usr/bin/kube*
rm -rf /etc/cni
rm -rf /opt/cni
rm -rf /var/lib/etcd
rm -rf /var/etcd
yum clean all
yum remove kube*
```
