# ubuntu-server-setup

it's now ONLY for 18.04 LTS, i may update it after 20.04 published
some script for high-speed LAN internet connect.
maybe you should run all these script as root user...

## apt main 
```shell
sudo curl -fSL https://raw.githubusercontent.com/zhoudian64/ubuntu-server-setup/master/apt-main-aliyun.sh | sh
# user repo on gitee even faster
sudo curl -fSL https://gitee.com/zhoudian64/ubuntu-server-setup/raw/master/apt-main-aliyun.sh | sh
```

## apt k8s
```shell
sudo curl -fSL https://raw.githubusercontent.com/zhoudian64/ubuntu-server-setup/master/apt-k8s-aliyun.sh | sh
# use repo on gitee even faster
sudo curl -fSL https://gitee.com/zhoudian64/ubuntu-server-setup/raw/master/apt-k8s-aliyun.sh | sh
```

## apt docker-ce
FAILED mirrors.aliyun.com down
```shell
sudo curl -fSL https://raw.githubusercontent.com/zhoudian64/ubuntu-server-setup/master/apt-docker-aliyun.sh | sh
sudo curl -fSL https://gitee.com/zhoudian64/ubuntu-server-setup/raw/master/apt-docker-aliyun.sh | sh
```

## k8s.gcr.io
```shell
curl -fSL https://raw.githubusercontent.com/zhoudian64/ubuntu-server-setup/master/k8s_gcr_io-aliyun.sh -o k8s_gcr_io-aliyun.sh
# and you should modify those version tags
curl -fSL https://gitee.com/zhoudian64/ubuntu-server-setup/raw/master/k8s_gcr_io-aliyun.sh k8s_gcr_io-aliyun.sh -o k8s_gcr_io-aliyun.sh
chmod +x k8s_gcr_io-aliyun.sh
./k8s_gcr_io-aliyun.sh
```
### kubeadm init
with flannel
```shell
# you need to disable swap
sudo swapoff -a
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
  // you may get a registry-mirrors from aliyun.com
}
EOF
mkdir -p /etc/systemd/system/docker.service.d
systemctl daemon-reload
systemctl restart docker

kubeadm init --pod-network-cidr=10.244.0.0/16 --image-repository registry.aliyuncs.com/google_containers
# kubectl apply -f https://raw.githubusercontent.com/zhoudian64/ubuntu-server-setup/master/kube-flannel.yaml
# (recommended)
kubectl apply -f https://gitee.com/zhoudian64/ubuntu-server-setup/raw/master/kube-flannel.yaml
```
