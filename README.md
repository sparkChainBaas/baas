## 火花区块链接入平台

火花区块链接入平台是对接于各行各业应用和各大公链之间的接入平台，采用一系列技术来提高公链的接入TPS，并统一公链之间的接入差异，简化应用的区块链接入，使得一套接口能快速在各种不同公链之间进行切换或进行跨链交易。

**注意：调用接口时，请务必使用https方式，保证传输内容不被篡改。**

## 各大版本

### <a href="./doc/v2.0/index.md"> 火花区块链接入平台 2.0版 文档</a>

   - 开始支持EOS、XRP链，只需要在使用过程中把其chainCode换成eos,xrp。它们对应的原生token分别为EOS,XRP。
   - 细化上链返回状态。
   - 为了限制某些应用不正常的访问，本版本对所有的访问加上次数的统计，在不久的将来采用经济手段来限制过大量的访问。

### <a href="./doc/v1.3/index.md"> 火花区块链接入平台 1.3版API文档</a>
   - “钱包余额同步”接口（这个版本开始已不支持）
   - “同步流水”接口（这个版本开始已不支持）
   - 除了“接入系统初始化”和“生成访问凭证”接口，所有的接口都需要accessToken入参（原没有accessToken的接口，2018年12月1日后将不支持）

### <a href="./doc/v1.0/index.md"> 火花区块链接入平台 1.0版API文档</a>
   - 提供获取应用的区块链信息接口
   - 提供账户的文本上链接口
   - 丰富了创建钱包的返回信息


### <a href="./doc/v0.9/index.md"> 火花区块链接入平台 0.9版API文档</a>
   - 提供基本的交易、文本上链的等接口
   - 实现不同公链的切换功能
   - 提供流水查询的接口
   - 提供基本的敏感词过滤功能

## 学习示例

 - 誓言墙：简单的快速上手的解决方案，https://github.com/moacDapp/oathWall
 - 扫雷小程序：小程序上链哦，可以玩一玩，https://github.com/moacDapp/mineScan
 - moac 流水查询器：学习moac chain3的最佳例子, https://github.com/moacDapp/moac_scan
 - 微信留言板：官方的标准示例，稍稍复杂了一点，https://github.com/sparkChainBaas/wechatMsg
 - Moac、ETH、Jingtum客户端的开发环境，不需要自己搭客户端，搭NodeJS，简单的一行docker命令即可使用，https://github.com/sparkChainBaas/baseDocker

## 官方联系方式

### 官方QQ群：594629943

![QQ群：594629943](./doc/sp.png)


## 第三方合作伙伴

 - <a href="https://www.jingtum.com/">井通科技</a>,github地址为：https://github.com/swtcpro ，开发者文档地址：http://developer.jingtum.com/  浏览器地址：http://state.jingtum.com

 - <a href="http://www.moac.io/">MOAC</a>,github地址为：ttps://github.com/MOACChain/,开发者文档地址：https://github.com/MOACChain/moac-core/wiki/Commands ,https://github.com/MOACChain/moac-core/wiki/Chain3 ,浏览器地址：http://explorer.moac.io/home
