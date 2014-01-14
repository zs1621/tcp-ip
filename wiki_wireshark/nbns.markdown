
##**NBNS(NetBIOS Name Service)**


参考 [wiki-nbns](http://wiki.wireshark.org/NetBIOS/NBNS)

------------------------------
###**简介**
 - 这个服务在Windows中叫 WINS
 - NBNS做的事和DNS有相同的目地: 把人类可读的名字转化为IP地址
 - 现在Windows networks中NBNS还是有的
 - NBNS 只支持IPV4地址不支持IPv6

###**协议依赖**
 - UDP: 典型的， NBNS 使用UDP作为其传输协议
 - TCP: NBNS也用TCP作为其传输协议，实际中一般不会用TCP


 ps: 看了半天，看了个没吊用的....

