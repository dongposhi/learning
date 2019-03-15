


《Mastering Bitcoin 2nd Edition》

# Chapter 1: What's Bitcoin

1. POW is a solution for 拜占庭将军问题。解决了如何在去中心化的环境中达成一致。
2. Bitcoin wallet is the UI to the bitcoin system. just like web browser.
	3. Bitcoin wallet 有很多，可以运行在各种设备和平台。
3. Bitcoin 交易是不可恢复的。
4. Bitcoin 可以以小于一个单位（1 bitcoin）进行交易
5. Bitcoin 的交易就是value 在不同wallet 地址之间传递。
6. Bitcoin 交易   propagate  ---->validation---->clearing
7. 大约每10 分钟系统完成一次clearing，也就是把validate 的交易更新到系统的分布式账本上。

# Chapter 2: How Bitcoin Works
1. bitcoin system 的构成
	- users：拥有钱包
	- transactions： 被全网传播
	- miners：交易的权威账本记录者
2. 交易完成后，接收方能看到的是交易已经提交验证，正在等待完成，不能直接看到完成。因为clearing 需要10 minutes。（这是我的理解，待验证）
3. Term: "Spending"   signing a transaction.
4. 找零 （change）不收取费用，找零可以转入到Owner 钱包的另一个地址。
	- 一个疑问，因为交易的input 是上次的转入金额，所以有可能远大于本次交易金额。
5. 一个钱包可能会拥有多个支付地址（好比不同面额的纸币），有钱包来决定如何组合来完成支付。
6. People subconsciously find a balance between these two extremes, and bitcoin wallet developers strive to program this balance.  （有用的英文表达）
7. 一个block 包含多个transactions
	8. block 是矿工解题的一个单位。包含了多个交易的验证。
# Chapter 3 Bitcoin core
# Chapter 4 Keys And Addresses
1. 在 希腊语中 Cryptography 意味着 secret writing。
2. Ownership of bitcoin is established through digital keys, bitcoin addresses, and digital signatures
3. keys are stored in wallet ( a simple database). 独立于比特币协议，与区块链本身无关。
4. witness： digital signature used to spend funds.
5. **Bitcoin Addresses**: A bitcoin address is a string of digits and characters that can be shared with anyone who wants to send you money. Addresses produced from public keys consist of a string of numbers and letters, beginning with the digit “1.”
	1. bitcoin addresses 可以非常灵活。他可以代表一个key pair 的拥有者，也可以是一段支付脚本。 
	7. bitcoin addresses 从public key 单向（不可逆）推演（hash）出来
		1. SHA256, RIPEMD160 
		9. encoded as BASE58Check
6.  Vanity address （类似于电信的吉祥号）
	1. address 是一串无意义的随机数字。Vanity Address 是头部几个字母构成有意义的一种地址。使用上没有什么特殊。它的生成过程需要考虑概率
7. Paper wallets： Paper wallets are bitcoin private keys printed on paper. Paper wallets are a very effective way to create backups or offline bitcoin storage, also known as “cold storage.”
# chapter 5 Wallets

1. 广义而言，Wallet 是主要的用户接口，控制了对用户money的访问，管理keys 和地址，tracking the balance， 创建、签名交易
2.  狭义而言，wallet 指的是存储和管理keys 的数据结构。（这是本章的重点）通常实现为结构化的文件或简单的database
3. 一个误区是 **wallet 包含bitcoin**。bitcoin 是被记录在bitcoin network 的区块链上。wallet 只包含 “keys”
4. 两种主要类型的wallet
	1. JBOK wallet：“just a bunch of keys”    Keys 是各自独立的。
		1. 不推荐，难于备份和使用
	2. HD wallet。 所有的keys 来源于一个 master key （seed）
4. wallets 已经比较成熟，有一些工业标准了



          
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk1MDc5NjI0OV19
-->