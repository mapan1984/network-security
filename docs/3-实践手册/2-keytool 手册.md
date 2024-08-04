# keytool 手册

## keystore/truststore

- keystore: 保存自身的公钥、私钥，以及自身的签名证书
- truststore: 保存信任的证书，一般是 CA 证书，进而可以信任所有 CA 签名的证书

## 格式 JKS/PKCS12

java 8 默认生成 JKS 格式文件，java 9 之后默认生成 PKCS12 格式文件

可以通过 `-storetype` 参数指定格式为 `JKS` 或者 `PKCS12` 

## 验证内容

验证 store 文件内容：

    keytool -list -v -keystore client.truststore.jks [-storepass <storepass>]

    keytool -list -rfc -keystore client.truststore.jks [-storepass <storepass>]

    keytool -list -v -keystore client.truststore.jks -storetype pkcs12

