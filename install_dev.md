## 安装测试节点与节点同步
Chia的测试网络目前版本为`testnet_7`，需要到`github`上下载该版本测试网。

本文基于`Mac`安装测试网络，需要有`python3.7`的环境。

### 第一步：新建测试节点工作目录：
```
cd ~
mkdir chia
cd chia
```

### 第二步：下载并安装`testnet_7`
```
git clone https://github.com/Chia-Network/chia-blockchain.git -b testnet_7
cd chia-blockchain
sh install.sh
```
如果本地已经安装了主网的钱包，并且已经设置了账号，没关系，因为测试网的账号与主网不冲突，但是不能安装到同一个目录中。
建议提前安装python3.6或者以上版本
国内用户建议使用github镜像访问，比如https://github.com.cnpmjs.org或者https://hub.fastgit.org。直接在访问链接中替换域名部分即可
新目录记得处理下权限，chmod 777 ./*.sh。然后使用./install.sh


测试网的账号以`txch`开头，主网的账号以`xch`开头。

### 第三步：启动测试节点
在`chia-blockchain`目录中，运行以下命令：
```
. ./activate
chia init # 如果更改配置文件的话，需要执行，否则略过
chia start all
```

### 第四步：同步节点数据
运行以上启动命令后，节点同步即开始，请通过以下命令查看节点同步是否完成
```
chia show -s
```
![](https://tva1.sinaimg.cn/large/008i3skNgy1gr2t3gfw23j31hy0u07wi.jpg)

如果`当前网络高度`等于`本节点高度`时，说明节点同步完成。

### 升级安装
使用`git pull`拉取最新源码后，执行以下命令：
```
cd chia-blockchain
chia stop -d all
deactivate
git fetch
git checkout latest
git pull
sh install.sh
. ./activate
chia init
```

