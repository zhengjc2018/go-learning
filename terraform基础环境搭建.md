## 搭建基于Linux下terraform开发的基础环境

[TOC]
### 0. Linux环境搭建
- 百度

### 1. Ubuntu下安装Go
————————————————————————
##### 1. 下载tar包
```
wget https://dl.google.com/go/go1.10.linux-amd64.tar.gz
```
##### 2. 解压到指定路径
```
sudo tar zxvf go1.10.linux-amd64.tar.gz -C /vagrant/go
```
##### 3. 配置bashrc, 设置环境变量以及AK
```
export GOROOT=/vagrant/go
export GOPATH=/vagrant/gopath
export GOBIN=/vagrant/gopath/bin
export PATH=$PATH:$GOPATH:$GOBIN:$GOROOT/bin
export ALICLOUD_REGION=cn-shanghai    // 可选，这个可以控制测试用的regionID

export ALICLOUD_ACCESS_KEY="***"
export ALICLOUD_SECRET_KEY="***"
```
##### 4. source ~/.bashrc
##### 5. 检查版本
```
go version
```
##### 6. 安装相关包
```
go get golang.org/x/tools/cmd/goimports
```

### 2. 下载terraform provider
##### 1. clone代码
```
mkdir -p $GOPATH/src/github.com/terraform-providers
cd $GOPATH/src/github.com/terraform-providers
git clone https://github.com/terraform-providers/terraform-provider-alicloud
```
##### 2. build 
```
cd $GOPATH/src/github.com/terraform-providers/terraform-provider-alicloud
go get github.com/kardianos/govendor/
govendor sync -v
make devlinux   // 每次都会重新build，example中的main.tf调用的core是bin文件夹下的文件
```

### 3. 安装terraform
##### 1. 下载zip包
```
mkdir -p /vagrant/download && cd /vagrant/download
wget https://releases.hashicorp.com/terraform/0.12.4/terraform_0.12.4_linux_amd64.zip
```

##### 2. 解压
```
unzip terraform_0.12.4_linux_amd64.zip
```
##### 3. profile中添加环境变量
```
vi ~/.profile
export PATH="$PATH:/vagrant/download"
$source ~/.profile
```

### 4. 测试
##### 1. 命令
```
TF_ACC=1 go test ./alicloud -v -run=TestAccAlicloudEipAssociationEni -timeout=300s

TF_ACC=1 go test ./alicloud -v -run=TestAccAlicloudInstance -timeout=300s
```


















