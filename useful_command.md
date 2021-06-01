## 常用命令行命令

#### 查看帮助
```
chia -h
```

#### 查看版本
```
chia version
```

#### 初始化节点
```
chia init #启动，更改设置后需要使用
```

#### 启动节点
```
chia start all
```

#### 创建账号key
```
chia keys generate
```
注意：创建账号后，请务必将24个单词的助记词安全保存下来

#### 现实本地账号key
```
chia keys show
```

#### 显示钱包内指纹fingerprint信息
```
chia wallet show
```

#### 现实节点同步状况
显示网络状态，如当前高度，同步状态，当前难度等
```
chia show -s
```

#### 发送代币
```
chia wallet send -f 汇出帐号fingerprint -t 汇入帐号地址 -a 金额
```
注意：必须要`wallet`同步完成后才能执行，即执行 `chia wallet show`，显示钱包 Wallet height: 最新高度，Sync status: Synced

#### plot盘（P盘）
```
chia plots create #P盘
举例：
chia plots create -k 32 -b 4000 -t ~/Downloads/chia/tmp -d ~/Downloads/chia/final
```

#### 检查P盘情况
```
chia plots check -n 30
```

#### 现实P盘目录
```
chia plots show #显示P盘的工作目录
```

注：以上命令都可以通过加`-h`，打印出详细的帮助信息