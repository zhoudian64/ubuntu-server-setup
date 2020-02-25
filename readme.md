# ubuntu-server-setup

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
```shell
sudo curl -fSL https://raw.githubusercontent.com/zhoudian64/ubuntu-server-setup/master/apt-docker-aliyun.sh | sh
sudo curl -fSL https://gitee.com/zhoudian64/ubuntu-server-setup/raw/master/apt-docker-aliyun.sh | sh
```

## k8s.gcr.io
```shell
curl -fSL https://raw.githubusercontent.com/zhoudian64/ubuntu-server-setup/master/k8s_gcr_io-aliyun.sh
# and you should modify those version tags
curl -fSL https://gitee.com/zhoudian64/ubuntu-server-setup/raw/master/k8s_gcr_io-aliyun.sh
```
