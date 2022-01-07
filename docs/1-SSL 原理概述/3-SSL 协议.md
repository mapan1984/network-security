# 1.2 SSL 协议

SSL(Secure Sockets Layer) 是由 Netscape 公司设计，用于为网络通信提供安全及数据完整性的一种安全协议，随着 SSL 的广泛应用与发展，SSL 逐渐成为互联网的一个标准，并更名为 TLS(Transport Layer Security)。

SSL 协议一般位于 TCP/IP 协议与各种应用层协议之间，为数据通讯提供安全支持。

## 基本思路

SSL 协议的基本思路是将通信过程分为 2 个阶段：

1. 握手阶段：利用非对称加密验证服务端身份，并协商出一个对称加密算法与密钥
2. 记录阶段：利用握手阶段协商得到的对称加密算法与密钥，加密通信内容

## 详细过程

详细过程如下：

![加密](/network-security/images/ssl_handshake_rsa.jpg)

1. client 向 server 发送加密通信请求，请求中携带 client 支持的协议版本，client 生成的随机数 client-random，加密算法，压缩方法等内容
2. server 首先从 client 发送的请求中选择要使用的协议版本与加密算法，之后向 client 发送响应，响应中携带要使用的协议版本与加密算法，server 生成的随机数 server-random 以及 server 的数字证书
3. client 收到 server 的响应后
    1. 验证 server 数字证书：证书是不是可信 CA 发布的；证书中的域名与 server 域名是否一致；证书是否过期；
    2. 如果证书可信或者 client 接受了不可信的证书，client 会从证书中获取 server 公钥，否则握手失败；
    3. client 产生一个随机数 pre-master-key，用 server 的公钥加密后发送给 server
    4. client 用 client-random, server-random, pre-master-key 计算本次会话使用的密钥
4. server 收到 client 发送的公钥加密的 pre-master-key 后，用自己的私钥解密，用 client-random, server-random, pre-master-key 计算本次会话使用的密钥
5. 握手结束后，client 和 server 使用握手阶段协商得到的加密算法与会话密钥进行对称加解密的传输。

握手阶段传输内容是明文的（没法加密），如果第三方窃听，他可以获取双方选择的加密算法，client-random 和 server-random。但是 pre-master-key 是使用 server 公钥加密的，如果第三方无法破解出 pre-master-key，则无法得到会话密钥，会话仍是安全的。

## 证书的作用

在这个过程中，为了避免中间人冒充服务端，提供虚假的公钥，服务端需要用自己的信息向 CA 申请 SSL 证书。CA 会通过自己的私钥将网站的公钥和网站的各种相关信息，比如域名等进行签名，形成电子证书，颁发给申请者。

证书中包含了服务器的名称和地址，服务器的公钥，签名颁发机构名称，来自签名颁发机构的签名等内容。

客户端不直接获取公钥，而是获取 CA 颁发的证书，之后通过验证证书为可信 CA 签名，且信息与服务端吻合，未篡改，未过期，验证服务端，从而产生信任关系，之后从证书中获取服务端公钥。

## 单向认证与双向认证

### ssl 单向认证

单向认证是指仅验证服务端的身份。

### ssl 双向认证

双向认证相比于单向认证，客户端需要向服务端提供证书，服务端需要用客户端的证书验证客户端身份，并且服务端返回的加密算法是用客户端的公钥加密后发送给客户端的

## 参考

- https://www.ruanyifeng.com/blog/2014/02/ssl_tls.html
- https://www.ruanyifeng.com/blog/2014/09/illustration-ssl.html
- https://blog.cloudflare.com/announcing-keyless-ssl-all-the-benefits-of-cloudflare-without-having-to-turn-over-your-private-ssl-keys/
- https://blog.cloudflare.com/keyless-ssl-the-nitty-gritty-technical-details/
