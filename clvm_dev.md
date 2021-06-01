## CLVM开发环境搭建
#### 什么是CLVM？
`CLVM`是`CliaLisp`智能交易合约的运行环境。

#### 安装
```
cd ~/chia #为了方便起见，本教程统一使用~/chia为工作根目录
git clone https://github.com/Chia-Network/clvm_tools
python3 -m venv venv
. ./venv/bin/activate
pip install -e . #安装chialisp
```

#### 运行
```
cd ~/chia/clvm_tools/
. ./venv/bin/activate
```

#### 运行命令
命令行下有两个运行命令，一个是`brun`，一个是`run`。

区别在于，`brun`命令偏底层，`run`命令少许高级些，`run`的执行结果是`brun`的输入。具体区别在下一章体会。

可以输入下面一些命令体验下：
```
brun '(+ (q . 1) (q . 2))' # 假发运算，返回3
brun '(divmod (q . 50) (q . 3))' #求mod
brun '(concat (q . "hello") (q . 49))' #返回hello1，字符串链接
brun '(i (= (q . 2) (q . 3)) (q . "true") (q . "false"))' # 判断语句，如果2<>3，返回'false'
```

