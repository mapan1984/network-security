# OpenSSL 实践

利用openssl分别生成服务器和客户端的私钥和公钥：

    # 生成 server 端 rsa 私钥
    $ openssl genrsa -out server.key 1024

    # 生成 client 端 rsa 私钥
    $ openssl genrsa -out client.key 1024

    # 生成私钥对应的 rsa 公钥
    $ openssl rsa -in server.key -pubout -out server.pem
    $ openssl rsa -in client.key -pubout -out client.pem

为了避免第三方攻击，需要第三方 CA(Certificate Authority) 为站点颁发数字证书，这个证书具有CA通过自己的公钥和私钥实现的签名。

为了得到签名，服务器需要通过自己的私钥生成CSR(Certificate Signing Request)文件。CA将通过这个文件颁发属于该服务器的签名证书，只要通过CA机构就能验证证书是否合法。


自己扮演CA机构，给自己的服务器颁发证书：

    # 生成 CA 机构 rsa 私钥
    $ openssl genrsa -out ca.key 1024

    # 生成 CA 机构 CSR(Certificate sign request) 文件
    $ openssl req -new -key ca.key -out ca.csr

    # 通过私钥子签名生成证书(CA 自签名证书)
    $ openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt

生成服务器的证书：

    # 生成 server 端 CSR文件
    $ openssl req -new -key server.key -out server.csr

    # 生成 server 证书
    $ openssl x509 -req -CA ca.crt -CAkey ca.key -CAcreateserial -in server.csr -out server.crt

