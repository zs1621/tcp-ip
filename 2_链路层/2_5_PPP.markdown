##ppp协议做什么?
 - 提供一种标准方式在点对点的链路上传输多种网络层协议的数据报, 简单来说就是一种对数据报的封装标准(就是SLIP协议的改进版)

##ppp协议对应osi模型的哪一层?
 - 链路层


##PPP协议基于的物理层?
 - 串行链路


##PPP组成
 - 链路控制协议LCP
 - 网络控制协议NCP
 - 扩展协议(如Multilink Protocol)
 ![gif](http://iworkout.cn/static/test/blog/ppp_stack.gif)


##PPP协议特点
 - 支持点到点的链接， 且具有CHAP、PAP验证协议，更好的保证了网络的安全性。
 - PPP的物理层既支持8位和无奇偶校验的异步模式，还支持面向比特位的同步链接，如Frame relay 协议必须为同步电路
 - PPP有针对不同网络层的网络控制协议， 如IPCP, IPXCP.并且允许双方协商是否对报文首部进行压缩


##PPP数据报帧结构

  ![png](http://iworkout.cn/static/test/blog/ppp.png)

  - 每帧都以标志字符0x7e开始和结束.紧跟着一个地址字节，始终是0xff, 然后是一个值为0x03的控制字节
  - 接下来是协议字段, 类似与以太网中类型字段的功能， 如下图

  ![png](http://iworkout.cn/static/test/blog/ppp_chengzai.jpg)
  
   - 下面是CRC字段， 监测数据中的错误

###PPP协议的简单过程
  -![png](http://iworkout.cn/static/test/blog/blog_ppp_process.png)
### LCP数据报文结构
   - NCP数据报文与其基本一致
   ![png](http://iworkout.cn/static/test/blog/lcp.png)

