# 2.2 https

## 概述

HTTPS 是 HTTP 协议与 SSL/TLS 协议的组合，这里 SSL 作为一种安全协议，它在传输层提供对网络连接加密的功能。对于应用层而言，它是透明的，数据在传递到应用层之前就已经完成了加密和解密的过程。

HTTPS 的协议栈是这样的：

    HTTP        HTTP
     |            ^
     v            |
    SSL/TLS     SSL/TLS
     |            ^
     v            |
     TCP         TCP
     |            ^
     v            |
     IP   ------> IP
