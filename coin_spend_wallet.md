## Coins, Spends and Wallets

(原文链接：https://chialisp.com/docs/coins_spends_and_wallets/)

本指南的这一部分将涵盖评估程序内部的程序，`ChiaLisp`与`Chia`网络上的交易和币的关系，并涵盖一些使用`ChiaLisp`创建`智能交易`的技术。如果有任何你不确定的术语，请务必查看`词汇表`。

### Coins

一个`Coin`的`ID`由3个部分构成。

* 父类的ID
* 他的puzzle哈希
* 金额

要构建一个Coin的ID，只需将这3条信息的哈希值按顺序连接起来即可。
```
coinID == sha256(parent_ID + puzzlehash + amount)
```

这意味着一个`Coin`的`puzzle_hash`和金额是其的内在组成部分，不能改变，只能花费(Spend)。

`Coin`的类也是由这3部分组成的，但不被`Hash`的，而是被完整地存储起来。下面是定义`Coin`的代码:
```
class Coin:
    parent_coin_info: bytes32
    puzzle_hash: bytes32
    amount: uint64
```

### Spend
当你花掉一个`Coin`的时候，他会被销毁。除非`puzzle`的代码指定了在花费`coin`的时候如何处理它，否则`coin`的价值也会在花费时被销毁。

要花费一枚`coin`，需要3个信息（第四个为可选项）:

* Coin的ID
* Coin的puzzle的完整来源
* 硬币puzzle的解决方案
* 一组签名的集合，称为聚合签名(可选)

请注意，`puzzle`和解决方案与第一部分中所涉及的相同，只是`puzzle`已经被储存在`coin`内，任何人都可以提交一个解决方案。

`Chia`网络没有`Coin`所有权的概念，任何人都可以在网络上花费任何`Coin`。这就需要`puzzle`来防止`Coin`被盗或以非预期的方式使用。

如果任何人都可以提交一个`Coin`的解决方案，你可能会想知道某人如何能"拥有"一个`Coin`。在本指南的下一节结束时，希望解决该问题。

### Puzzle和解决方案实例
通过`clvm_tool`，任何人都可以执行类似下面的指令：
```
$ brun '(+ 2 5)' '(40 50)
90

$ brun '(c (q . 800) 1)' '("some data" 0xdeadbeef)'
(800 "some data" 0xdeadbeef)
```

这些都是孤立的有趣练习，但这种格式可以用来向区块链网络传达一个`Coin`在花费时应该如何操作的指令。这些指令在下面的`OpCodes`的列表中：


### OpCodes

解决方案的操作码和参数如下：

注：操作码被分成两类：
* this spend is only valid if X
* if this spend is valid then X

#### AGG_SIG_UNSAFE - [49] - （49 pubkey message）
这个花费只有在所附的聚合签名包含来自给定`message`的给定`pubkey`的签名时才有效。这被标记为不安全，因为如果你在一条消息上签名一次，你所拥有的任何其他需要该签名的硬币也有可能被解锁。也许使用`AGG_SIG_ME`更好，因为引入了硬币的`ID`

#### AGG_SIG_ME - [50] - （50 pubkey message）
只有当所附的聚合签名包含来自该`message`的`pubkey`与硬币的ID相连接的签名时，该花费才有效

#### CREATE_COIN - [51] - (51 puzzlehash amount)
如果这个花费是有效的，那么就用给定的`puzzle_hash`和`amount`创建一个新的硬币

#### ASSERT_FEE - [52] - （52 amount）
只有在此交易中存在与`amount`相等的未使用价值时，此支出才有效，该价值明确地被用作费用

#### CREATE_COIN_ANNOUNCEMENT - [60] - （60 message）
如果这个花费是有效的，这将创建一个短暂的公告，其ID取决于创建它的`Coin`。其他币可以确信公告存在，以便在一个区块内进行币间通信

#### ASSERT_COIN_ANNOUNCEMENT - [61] - (61 announcementID)
这个花费只有在这个区块中存在与公告ID相匹配的公告时才有效。公告ID是被宣布的消息的哈希值与宣布它的`coinID`相连接 `announcementID == sha256(coinID + message)

#### CREATE_PUZZLE_ANNOUNCEMENT - [62] - （62 message）
如果这个花费是有效的，这会创建一个短暂的公告，其ID取决于创建它的`puzzle`。其他币可以确信公告的存在，以便在一个区块内进行币间通信

#### ASSERT_PUZZLE_ANNOUNCEMENT - [63] - (63 announcementID)
只有当这个区块中存在与公告ID相匹配的公告时，这个花费才有效。公告ID是宣布的消息与宣布该消息的硬币的`puzzle_hash`值相连接 `announcementID == sha256(puzzle_hash + message)

#### ASSERT_MY_COIN_ID - [70] - (70 coinID)
只有当`coinID`与包含此`puzzle`的`coinID`完全相同时，此花费才有效。

#### ASSERT_MY_PARENT_ID - [71] - (71 parentID)
只有当提交的`parentID`与包含此`puzzle`的硬币的`parentID`信息完全相同时，此花费才有效。

#### ASSERT_MY_PUZZLE_HASH - [72] - （72 puzzlehash）
只有当提交的`puzzlehash`与包含此`puzzle`的硬币的`puzzlehash`完全相同时，此花费才有效。

#### ASSERT_MY_AMOUNT - [73] - (73 amount)
只有当提交的`amount`与包含此`puzzle`的`coin`的`amount`完全相同时，此消费才有效。

#### ASSERT_SECONDS_RELATIVE - [80] -（80 seconds）
这个花费只有在这个硬币被创建后已经过了给定的时间才有效。

#### ASSERT_SECONDS_ABSOLUTE - [81] -（81 seconds）
只有当这个区块上的时间戳大于指定的时间戳时，这个花费才有效。

#### ASSERT_HEIGHT_RELATIVE - [82] - (82 block_age)
只有当这个`coin`被创建后已经过了指定的区块高度，这个花费才会有效。

#### ASSERT_HEIGHT_ABSOLUTE - [83] - (83 block_height)
这个花费只有在达到给定的`block_height`时才有效。


在`Chialisp`中，以上条件以`List`方式表述：
```
((51 0xabcd1234 200) (50 0x1234abcd) (53 0xdeadbeef))
```

### 实例1：使用口令锁定`Coin`

让我们创造一个`Coin`，只要知道密码，任何人都可以使用。


为了实现这一点，我们将把密码的哈希值放入`puzzle`中，如果出现正确的密码，`puzzle`将返回指令，用`puzzle`中给出的哈希值创建一个新的硬币。在下面的例子中，密码是"hello"，其哈希值为`0x2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824`。上述`Coin`的实现如下：

```
(i (= (sha256 2) (q . 0x2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824)) (c (q . 51) (c 5 (c (q . 100) ()))) (q . "wrong password"))
```

这个程序通过`(sha256 )`获取解决方案中第一个元素的哈希值`(2)`，并将其与已经提交的值进行比较。如果密码正确，它将执行`(c (q . 51) (c 5 (c (q . 100) ())))`，评估为`(51 0xmynewpuzzlehash 100)`。请注意，`51`是使用解决方案中的`puzzlehash`创建一个新coin的操作代码，`5`相当于`(f (r 1))`。

如果密码不正确，它将返回字符串`wrong password`。

这个解决方案的格式应该是`(passowrd newpuzzlehash)`。记住，只要知道`coinID`和完整的`puzzle`代码，任何人都可以花费这个硬币。

让我们用`clvm_tools`测试一下。

```
$ brun '(i (= (sha256 2) (q . 0x2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824)) (c (c (q . 51) (c 5 (c (q . 100) ()))) (q ())) (q . "wrong password"))' '("let_me_in" 0xdeadbeef)'
"wrong password"

$ brun '(i (= (sha256 2) (q . 0x2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824)) (c (q . 51) (c 5 (c (q . 100) ()))) (q . "wrong password"))' '("incorrect" 0xdeadbeef)'
"wrong password"

$ brun '(i (= (sha256 2) (q . 0x2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824)) (c (q . 51) (c 5 (c (q . 100) ()))) (q . "wrong password"))' '("hello" 0xdeadbeef)'
((51 0xdeadbeef 100))
```

在完成一个完整的智能交易前，我们还需要做最后一个动作。

如果你想让一个花费无效，那么你需要用`x`（一个动作）去引发一个异常。否则你只是有一个有效的花费，但没有返回任何`OpCodes`，这将破坏我们的`coin`，而不是创造一个新的！所以我们需要将失败条件改为`(x "wrong password")`，这意味着交易失败，硬币没有被花费。

如果我们这样做，那么我们也应该把`(i A B C)`模式改为`(a（i A（q . B）（q . C））1)`。这方面的原因将在第三部分解释。现在先不要担心原因。

下面是我们完成的`Password protected coin`合约代码：
```
'(a (i (= (sha256 2) (q . 0x2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824)) (q . (c (c (q . 51) (c 5 (c (q . 100) ()))) ())) (q . (x (q . "wrong password")))) 1)'
```

在`clvm_tool`中测试：
```
$ brun '(a (i (= (sha256 2) (q . 0x2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824)) (q . (c (c (q . 51) (c 5 (c (q . 100) ()))) ())) (q . (x (q . "wrong password")))) 1)' '("let_me_in" 0xdeadbeef)'
FAIL: clvm raise ("wrong password")

$ brun '(a (i (= (sha256 2) (q . 0x2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824)) (q . (c (c (q . 51) (c 5 (c (q . 100) ()))) ())) (q . (x (q . "wrong password")))) 1)' '("hello" 0xdeadbeef)'
((51 0xdeadbeef 100))
```

### 从谜题puzzle与解决方案中生成操作码
让我们花点时间来考虑`send`和`spender`之间的权力平衡问题。另一种说法是"解决方案应该对输出有多大的控制权？"

假设我们用下面的`puzzle`锁定一枚硬币：
```
(q . ((51 0x365bdd80582fcc2e4868076ab9f24b482a1f83f6d88fd795c362c43544380e7a 100)))
```
无论通过什么方案，这个`puzzle`都会返回指令，创建一个新的`Coin`，`puzzle_hash`为`0x365bdd80582fcc2e4868076ab9f24b482a1f83f6d88fd795c362c43544380e7a`，金额为`100`。

```
$ brun '(q . ((51 0x365bdd80582fcc2e4868076ab9f24b482a1f83f6d88fd795c362c43544380e7a 100)))' '(80 90 "hello")'
((51 0x365bdd80582fcc2e4868076ab9f24b482a1f83f6d88fd795c362c43544380e7a 100))

$ brun '(q . ((51 0x365bdd80582fcc2e4868076ab9f24b482a1f83f6d88fd795c362c43544380e7a 100)))' '("it doesn't matter what we put here")'
((51 0x365bdd80582fcc2e4868076ab9f24b482a1f83f6d88fd795c362c43544380e7a 100))
```
在这个例子中，花费`coin`的结果完全由`puzzle`决定。尽管任何人都可以花费`coin`，但是把`coin`锁起来的人拥有花费`coin`的所有权力，因为解决方案根本不重要。

反过来说，让我们考虑一个被锁住的`coin`，其谜底如下。
```

```
这个例子可能看起来有点奇怪，因为大多数`ChiaLisp`程序都是`list`，而这只是一个`atom`，但它仍然是一个有效的程序。这个`puzzle`只是简单地返回整个解决方案。你可以从权力和控制的角度来考虑这个问题。把`coin`锁起来的人已经把所有的权力交给了提供解决方案的人。

```
$ brun '1' '((51 0xf00dbabe 50) (51 0xfadeddab 50))'
((51 0xf00dbabe 50) (51 0xfadeddab 50))

$ brun '1' '((51 0xf00dbabe 75) (51 0xfadeddab 15) (51 0x1234abcd 10))'
((51 0xf00dbabe 75) (51 0xfadeddab 15) (51 0x1234abcd 10))
```
在这种情况下，不仅任何人都可以花钱，而且可以随心所欲地花钱! 这种权力的平衡决定了`ChiaLisp`中很多`puzzle`的设计方式。

例如，让我们创造一个`puzzle`，让`spender`选择输出，但有一个规定。
```
  (c (q . (51 0xcafef00d 200)) 1)
```
这将让`spender`通过解决方案返回他们想要的任何条件和`OpCodes`，但总是会添加条件来创建一个`puzzlehash`为`0xcafef00d`和价值`200`的硬币。

```
$ brun '(c (q . (51 0xcafef00d 200)) 1)' '((51 0xf00dbabe 75) (51 0xfadeddab 15) (51 0x1234abcd 10))'
((51 0xcafef00d 200) (51 0xf00dbabe 75) (51 0xfadeddab 15) (51 0x1234abcd 10))
```

本节旨在证明一点，即`OpCodes`可以来自收件人的解决方案和发件人的难题，以及这如何代表信任和权力的平衡。

在接下来的练习中，我们将把我们所知道的一切放在一起，并在`Chia`中创建"标准"交易，这也是钱包能够相互发送交易的基础。


### 实例2：签名锁仓代币
如果你要想"送硬币给别人"，你只需创建一个`收件人签名`的`puzzle`，但允许他们返回任何他们喜欢的其他操作码。这意味着硬币不能被其他人花掉，但产出却完全由接收者决定。

我们可以构建以下智能交易，其中`AGG_SIG_UNSAFE`为`50`，收件人的`pubkey`为`0xfadedcab`

```
(c (c (q . 50) (c (q . 0xfadedcab) (c (sha256 2) (q . ())))) 3)
```
这个`puzzle`强迫计算结果包含`(50 pubkey **hash_of_first_solution_arg**），但随后又加上了解决方案中的所有条件。

让我们在`clvm_tools`中测试一下 - 在这个例子中，收件人的`pubkey`将被表示为`0xdeadbeef`。收件人想花钱创建一个新的`Coin`，这个`coin`被锁在谜题`0xfadeddab`中。

```
$ brun '(c (c (q . 50) (c (q . 0xfadedcab) (c (sha256 2) ()))) 3)' '("hello" (51 0xcafef00d 200))'
((50 0xfadedcab 0x2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824) (51 0xcafef00d 200))
```

### Wallets
钱包Wallet是一些具有多种功能的软件，使用户能够轻松地与`Coin`互动。

* 钱包可以跟踪公钥和私钥
* 钱包可以生成`puzzle`和解决方案
* 钱包可以用它的钥匙签名
* 钱包可以识别和记忆用户所"拥有"的币
* 钱包可以花费`Coin`

你可能想知道，如果任何人都可以尝试花费`Coin`，那么钱包如何能够识别用户"拥有"哪些`Coin`呢？这是因为所有的钱包已经知道并同意向某人发送硬币的标准格式是什么。他们知道他们自己的`pubkeys`是什么，所以当一个新的硬币被创建时，钱包可以检查该硬币内的`puzzle`是否是一个`standard send puzzle`给他们的`pubkeys`之一。如果是的话，那么这个硬币就可以被认为是属于这个"钱包"的，因为其他人不能花费它。

如果"拥有"该币的钱包想把该币再次发送给其他人，他们将生成一个`standard send puzzle`，但使用新收件人的`pubkey`。然后他们可以花掉他们拥有的硬币，销毁它，并在这个过程中创建一个新的硬币，并与新的收件人的`pubkey`一起锁定。然后，新的收件人可以识别并"拥有"这个硬币，并可以在以后按他们的意愿发送给其他人。

#### 找零
找零很简单。如果一个钱包花的钱少于这个硬币的总量，他可以用剩余部分的数量创造另一个硬币，并再次为自己锁定`standard puzzle`。你可以把一个硬币拆成尽可能多的新硬币，其价值为原来的几分之一，只要你想。

你不能从同一个母体创建两个相同价值的硬币，并使用相同的拼图哈希，因为这将导致`ID碰撞`，交易将被拒绝。

#### 硬币聚合(Coin Aggregation)和消费捆绑(Spend Bundles)
你可以将一堆较小的硬币聚合成一个大硬币。要做到这一点，你可以创建一个`SpendBundle`，它将一个或多个具有单一聚合签名的花费组合在一起。

当使用公告(announcements)时，`SpendBundle`特别重要。因为创建的公告只对它们所创建的区块有效，你要确保宣称这些公告的币与宣布的币一起被花费。

我们将在后面的章节中更多地讨论`SpendBundle`和硬币聚合(`Coin Aggregation`)。

### 实例3:支付给委托谜题（Delegated puzzle)
我们可以构建一个更强大的签名锁定硬币（signature locked coin）：
```
(c (c (q . 50) (c (q . 0xfadedcab) (c (sha256 2) ()))) (a 5 11))
```

第一部分基本相同，`puzzle`总是返回`AGGSIG`对`pubkey 0xfadedcab`的检查。然而，它只对谜题的第一个元素进行检查。这是因为，这个谜题的答案不是一个要打印出来的条件列表，而是一个`程序/解决方案对`。这意味着接收者可以运行自己的程序作为生成答案的一部分，或者签署一个谜题，让其他人提供答案。当我们使用程序参数来生成谜题时，将其称为"委托谜题 Delegated puzzle"。

新的程序和解决方案里面的解决方案被评估，其结果被添加到`OpCode`输出中。我们将在本指南的下一部分更详细地介绍其工作原理。

这个标准交易的基本解决方案可能如下：
```
("hello" (q . ((51 0xmynewpuzzlehash 50) (51 0xanothernewpuzzlehash 50))) ())
```
在`clvm_tools`中运行：
```
$ brun '(c (c (q . 50) (c (q . 0xfadedcab) (c (sha256 2) ()))) (a 5 11))' '("hello" (q . ((51 0xdeadbeef 50) (51 0xf00dbabe 50))) ())'

((50 0xfadeddab 0x1f82d4d4c6a32459143cf8f8d27ca04be337a59f07238f1f2c31aaf0cd51d153) (51 0xdeadbeef 50) (51 0xf00dbabe 50))
```

### 结论
币的所有权指的是创建一个带有`谜题puzzle`的硬币，这意味着它只有在由币的"所有者"的私钥签署后才能被使用。钱包软件的目标是生成、解释和管理这些种类的硬币和谜题。

本指南的下一部分将进一步深入探讨`ChiaLisp`，并介绍如何编写更复杂的`puzzle`。如果这部分指南中的任何材料让你感到困惑，请尝试在下一部分之后再回来看。


