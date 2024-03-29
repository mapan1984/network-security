# 1.2 证书与 CA

> **证书** 与 **CA** 存在的目的是为了提供可以安全可信地分发公钥的方法

## 证书

### 包含公钥和身份信息的证书

非对称加密体系中，每个人的公钥需要对外公布，每个人都可能接收到多个公钥，这时就需要将公钥与身份信息绑定，便于区分。

如果我想对外分发我的公钥，我不再直接对外提供公钥，而是对外提供我的证书，证书里包含我的公钥，同时还包含我的身份信息，例如我的名字、所属的国家、组织等信息。

特别的是，对于网络通信，可以把通信端的域名或者 IP 作为身份信息的一部分，这样网络连接建立之后，就可以从这个连接获取对端的证书，同时检查证书身份信息中的域名或者 IP 和这个连接对端的域名或者 IP 是否一致。

![证书](/network-security/images/证书.svg)

这样可以解决一部分问题，但是仍然不能防止中间人拦截并修改我的证书，将证书里的公钥替换成他的公钥，从而冒充我。

## CA

> CA （Certificate Authority）证书颁发机构

### 找 CA 对证书进行签名

怎么样可以防止其他人拦截并篡改我的证书内容，这个要求可以用 **数字签名** 完成。

可以找一个权威的第三方 CA 对我的证书进行 **数字签名**，之后我对外分发签名后的证书：

1. 用户可以使用 CA 的公钥验证 **数字签名**，从而确认这个证书是权威 CA 签发的，且内容未被篡改
2. 如果证书通过验证，用户可以从证书中正确地获取我身份信息和公钥

![CA签名](/network-security/images/CA签名.svg)

> 这里当然不能用我的私钥签名，不要忘了证书的目的就是为了让其他人正确的获取我的公钥，在没有获取我的公钥之前，其他人无法验证我的私钥签名

#### 前提是所有人都要信任 CA

这里有一个前提是 **所有人都要信任 CA**，即提前正确的获取到 CA 的证书（CA 的公钥，身份信息，（其他CA的签名））：

1. 浏览器/操作系统会内置一些权威 CA 证书
2. 可以手动添加/指定自己信任的 CA 证书
3. CA 信任链: CA 的证书同样可以让其他 CA 对其进行签名，这样如果 CA_1 不够权威，没有内置在浏览器/操作系统中，那么 CA_1 可以找更权威的 CA_2 对自己进行签名，这条 CA 签名的链条可以一直延伸下去。信任链的终点被称为 root CA，它是自签名的证书。

### CA 需要验证我的身份

CA 在对我的证书进行签名之前，要验证我的身份，一般是这样做的：

例如我对外提供一个网站服务，那么我证书身份信息中也要有这个网站域名，CA 会要求我在我的网站增加一个文本链接，文本内容是 CA 随机生成的内容，我在网站增加了这个文本后，CA 就可以确认我的确是这个网站的拥有者。
