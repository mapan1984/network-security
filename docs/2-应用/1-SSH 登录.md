# 2.1 SSH 登录

## 登录远程主机

远程登录主机"host"时，在本地输入命令(host_name可以是主机的ip地址):

    $ ssh user_name@host_name

请求登录host，这时会询问是否接受host的公钥，接收后会要求输入你的登录密码，输入的密码会用host的公钥进行加密，host接收加密后的登录信息后，会用自己的私钥解密，确认后会允许登录。这样你的登录信息就不会泄露给他人(即便被他人截获登录信息，但无host私钥，所以无法获得你的登录信息)。

## 免密登录

### 生成自己的公钥与私钥

在本地输入命令:

    $ ssh-keygen -t rsa -C "you-key-comment"

会在在目录`~/.ssh`中生成私钥: `id_rsa`,  公钥: `id_rsa.pub`。

### 上传公钥与免密码登录(数字签名)

输入命令上传公钥到host:

    $ ssh-copy-id user_name@host_name

host会最后一次要求你输入密码确认身份，密码正确后会将公钥加到host的`/home/user_name/.ssh/authorized_keys`文件中，此后你再次登录时，host会向你发送信息，你用自己的私钥进行数字签名后发送给host，host用你的公钥进行验证，验证成功后会允许登录，不需要输入密码。
