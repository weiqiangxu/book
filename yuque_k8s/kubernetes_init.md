# 搭建集群-使用kubeadm

## 一、环境准备

CentOS 7.6 64bit 2核 2G * 2 ( 操作系统的版本至少在7.5以上 )


1. 防火墙

```
systemctl stop firewalld
systemctl disable firewalld
```

2. 设置主机名

```
# 在master节点执行
hostnamectl set-hostname k8s-master
```

```
# 在其他节点执行
hostnamectl set-hostname k8s-node1
```

3. 主机域名解析

```
# 注意：这里的IP地址是机器的局域网IP地址
cat >> /etc/hosts << EOF
10.0.20.2 k8s-master
10.0.8.13 k8s-master
EOF
```

4. 保持所有节点的时间一致所以统一时间同步机制

```
yum install ntpdate -y
ntpdate time.windows.com
```

5. 关闭selinux

```
#查看selinux是否开启
getenforce
```

```
#永久关闭selinux，需要重启
sed -i 's/enforcing/disabled/' /etc/selinux/config
```

6. 关闭swap分区

```
#永久关闭swap分区，需要重启：
sed -ri 's/.*swap.*/#&/' /etc/fstab
```

7. 将桥接的IPv4流量传递到iptables的链

```
#在每个节点上将桥接的IPv4流量传递到iptables的链
cat > /etc/sysctl.d/k8s.conf << EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
vm.swappiness = 0
EOF
```

```
# 加载br_netfilter模块
modprobe br_netfilter
```

```
# 查看是否加载
lsmod | grep br_netfilter
```

```
# 生效
sysctl --system
```

8. 开启ipvs

在kubernetes中service有两种代理模型，一种是基于iptables，另一种是基于ipvs的。ipvs的性能要高于iptables的，但是如果要使用它，需要手动载入ipvs模块


```
#在每个节点安装ipset和ipvsadm：
yum -y install ipset ipvsadm
```

```
#所有节点执行
cat > /etc/sysconfig/modules/ipvs.modules <<EOF
#!/bin/bash
modprobe -- ip_vs
modprobe -- ip_vs_rr
modprobe -- ip_vs_wrr
modprobe -- ip_vs_sh
modprobe -- nf_conntrack_ipv4
EOF
```

```
#授权、运行、检查是否加载：
chmod 755 /etc/sysconfig/modules/ipvs.modules && bash /etc/sysconfig/modules/ipvs.modules && lsmod | grep -e ip_vs -e nf_conntrack_ipv4
```

```
检查是否加载：
lsmod | grep -e ipvs -e nf_conntrack_ipv4
```

```
#重启机器
reboot
```


## 二、每个节点安装Docker、kubeadm、kubelet和kubectl


1. 安装docker

```
wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo -O /etc/yum.repos.d/docker-ce.repo
yum -y install docker-ce-18.06.3.ce-3.el7
systemctl enable docker && systemctl start docker
docker version
```

2. 设置Docker镜像加速器：


```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "exec-opts": ["native.cgroupdriver=systemd"], 
  "registry-mirrors": ["https://du3ia00u.mirror.aliyuncs.com"], 
  "live-restore": true,
  "log-driver":"json-file",
  "log-opts": {"max-size":"500m", "max-file":"3"},
  "storage-driver": "overlay2"
}
EOF
```

```
sudo systemctl daemon-reload
```

```
sudo systemctl restart docker
```

3. 添加阿里云的YUM软件源(由于kubernetes的镜像源在国外)

```
cat > /etc/yum.repos.d/kubernetes.repo << EOF
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
```

4. 安装kubeadm、kubelet和kubectl

```
yum install -y kubelet-1.18.0 kubeadm-1.18.0 kubectl-1.18.0
```

5. 为了实现Docker使用的cgroup drvier和kubelet使用的cgroup drver一致，建议修改"/etc/sysconfig/kubelet"文件的内容

```
vim /etc/sysconfig/kubelet
```

```
# 修改
KUBELET_EXTRA_ARGS="--cgroup-driver=systemd"
KUBE_PROXY_MODE="ipvs"
```

6. 设置为开机自启动即可，由于没有生成配置文件，集群初始化后自动启动


```
systemctl enable kubelet
```

7. 使用kubeadm初始化集群


```
#查看k8s所需的镜像
kubeadm config images list
```

```
# 部署k8s的master节点
# 由于默认拉取镜像地址k8s.gcr.io国内无法访问，这里需要指定阿里云镜像仓库地址
# 注意这里的192.168.18.100需要更改为部署master的节点的局域网IP地址
kubeadm init \
  --apiserver-advertise-address=192.168.18.100 \
  --image-repository registry.aliyuncs.com/google_containers \
  --kubernetes-version v1.18.0 \
  --service-cidr=10.96.0.0/12 \
  --pod-network-cidr=10.244.0.0/16
```


8. 初始化成功后按照提示执行

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```


## 三、部署CNI网络插件


```
# 查看节点状态
kubectl get nodes
```

```
# 获取yml
wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

```
# 安装
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

```
# 查看集群pods确认是否成功
kubectl get pods -n kube-system
```

```
# 查看节点状态
kubectl get nodes
```

```
#查看集群健康状况：
kubectl get cs
kubectl cluster-info
```


## 四、部署nginx服务测试集群

```
# 取消master的taint否则只有一个节点的集群下pod不会被调度到master节点
kubectl taint node --all node-role.kubernetes.io/master-
```

```
# 部署Nginx
kubectl create deployment nginx --image=nginx:1.14-alpine
```

```
# 暴露端口
kubectl expose deployment nginx --port=80 --type=NodePort
```

```
# 查看服务状态需要看到nginx服务为running
kubectl get pods,svc
```

```
# 访问nginx服务需要看到输出Welcome to nginx!
curl k8s-master:30185
```


# 五、其他节点部署服务并加入到集群之中


```
# 安装Docker、kubeadm、kubelet和kubectl
# master节点生成token
# 默认2小时后会过期的token生成
kubeadm token create --print-join-command
```

```
# 生成一个永不过期的token
kubeadm token create --ttl 0 --print-join-command
```

```
#在其他节点执行
kubeadm join 192.168.18.100:6443 --token xxx \
    --discovery-token-ca-cert-hash sha256:xxx
```


## 五、备注

```
# kubeadmn重新初始化
kubeadm reset
```

```
# 查看pod执行详情定位无法启动的原因
kubectl describe pod [pod name]
kubectl describe pod nginx-deployment-xxx
kubectl describe pod mongodb -n NamespaceName
```

```
# 当错误是/run/flannel/subnet.env无法找到时候手动创建subnet.env内容是
FLANNEL_NETWORK=10.244.0.0/16
FLANNEL_SUBNET=10.244.0.1/24
FLANNEL_MTU=1450
FLANNEL_IPMASQ=true
```









