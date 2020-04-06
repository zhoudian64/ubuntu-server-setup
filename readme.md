# ubuntu-server-setup

## important
After struggle for a month I give up this silly project.
It's MORE clever to create a gateway, which can access to gcr.io and has a clean DNS.

it's now ONLY for 18.04 LTS, i may update it after 20.04 published
some script for high-speed LAN internet connect.
maybe you should run all these script as root user...

## apt main 
```shell
curl -fSL https://raw.githubusercontent.com/zhoudian64/ubuntu-server-setup/master/apt-main-aliyun.sh | sh
# user repo on gitee even faster
curl -fSL https://gitee.com/zhoudian64/ubuntu-server-setup/raw/master/apt-main-aliyun.sh | sh
```

## apt k8s
```shell
curl -fSL https://raw.githubusercontent.com/zhoudian64/ubuntu-server-setup/master/apt-k8s-aliyun.sh | sh
# use repo on gitee even faster
curl -fSL https://gitee.com/zhoudian64/ubuntu-server-setup/raw/master/apt-k8s-aliyun.sh | sh
```

## apt docker-ce
FAILED mirrors.aliyun.com down
just use
```shell
curl -skSL https://mirror.azure.cn/repo/install-docker-ce.sh | sh -s -- --mirror AzureChinaCloud
```
instead of 
```shell
curl -fSL https://raw.githubusercontent.com/zhoudian64/ubuntu-server-setup/master/apt-docker-aliyun.sh | sh
curl -fSL https://gitee.com/zhoudian64/ubuntu-server-setup/raw/master/apt-docker-aliyun.sh | sh
```

## k8s.gcr.io
```shell
curl -fSL https://raw.githubusercontent.com/zhoudian64/ubuntu-server-setup/master/k8s_gcr_io-aliyun.sh -o k8s_gcr_io-aliyun.sh
# and you should modify those version tags
curl -fSL https://gitee.com/zhoudian64/ubuntu-server-setup/raw/master/k8s_gcr_io-aliyun.sh -o k8s_gcr_io-aliyun.sh
chmod +x k8s_gcr_io-aliyun.sh
./k8s_gcr_io-aliyun.sh
```
### kubeadm init
with flannel
```shell
# you need to disable swap
swapoff -a
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
  // you may get a registry-mirrors from aliyun.com
  // actually I tried aliyun's registry for some days, maybe use HTTP_PROXY is the only way...
  // checkout https://docs.docker.com/config/daemon/systemd/
  // unfortunately HTTP_PORXY is the only mechanism can't be modified in daemon.json
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

### tidb-opertor
```shell
# kubectl apply -f https://raw.githubusercontent.com/zhoudian64/ubuntu-server-setup/master/tiller-rbac.yaml && \
# helm init --service-account=tiller --upgrade
# (recommended)
kubectl apply -f https://gitee.com/zhoudian64/ubuntu-server-setup/raw/master/tiller-rbac.yaml && \
helm init --service-account tiller --upgrade --tiller-image gcr.azk8s.cn/kubernetes-helm/tiller:v2.16.1 \
--stable-repo-url https://mirror.azure.cn/kubernetes/charts/
kubectl apply -f https://gitee.com/zhoudian64/ubuntu-server-setup/raw/master/crd.yaml && \
kubectl get crd tidbclusters.pingcap.com
```
