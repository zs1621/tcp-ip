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

####LCP协议数据报文的分类
 - 链路配置报文
  - 用来建立和配置一条链路， 主要包括Config-request[0x01]、Configure-Ack[0x02]、Configure-Nak[0x03]和Configure-Reject[0x04]
 - 链路终止报文
  - 用来终止一条链路， 主要包括Terminate-Request[0x05]和Terminate-Reply[0x06]
 - 链路维护报文
  - 用来管理和调试链路， 主要包括Code-Reject[0x07]、Protocal-Reject[0x08]、Echo-Request[0x09]、Echo-Reply[0x0a]和Discard-Request[0x0b]报文

####常用的协商Type
 - 0x01 MRU(maximum-receive-unit)
 - 0x02 Async-Control-Character-Map(4Byte, 用于异步链路控制字符的转义)
 - 0x03 Authentication-Protocol(至少2Byte, 鉴权协议协商)
 - 0x04 Quality-Protocol(至少2Byte, 链路质量检测协议协商)
 - 0x05 Magic-Number(4字节， 用于链路环路检测)
 - 0x06 RESERVED 保留字符
 - 0x07 Protocol-Field-Compression(指示性质的属性， 协议域压缩指示)
 - 0x08 Address-and-Control-Field-Compression(地址、控制字段压缩。在PPP协议中这两个字段不变)


####PPP协议层次结构
  ![png](http://iworkout.cn/static/test/blog/blog_ppp_layout.png)

####LCP协商
 - LCP的组成
  - 认证
  - 回拨
  - 压缩
  - 多链路PPP
 - 协商原则
  - 只协商不满足项
  - 直到达成一致为止

####LCP协商的数据报
 - 协议域
  - C021
 - Code域
  - 0x1~0x0b
 - LCP数据报其本质还是PPP的报文， 所以PPP帧头中Address域和Control域始终是FF03


####**ppp协议流程**
 - 当用户PC机拨号接入ISP后，就建立了从PC机到ISP的物理连接, 这时PC机向ISP发送一序列的LCP分组， 封装成PPP帧， 以便建立LCP连接。这些分组及其响应选择了将要使用的一些PPP参数。接着还要进行网络配置， NCP给新接入的PC机分配一个临时IP地址， 这样， 用户PC机就成为了因特网上的一个有IP地址的主机了。当用户通信完毕后，NCP释放网络层的连接， 回收原来分配出去的IP地址，接着， LCP释放数据链路层连接， 最后释放的是物理层的连接.
  - 具体步骤
    - 用户的PC机通过调制解调器呼叫路由器时， 路由器能够检测到调制解调器发出的载波信号。 在双方建立了物理层连接后， ppp就进入了链路建立的状态， 其目的是建立链路层的LCP连接
    - LCP开始协商配置选项， 即对发送LCP的配置请求帧， 这是个PPP帧，其协议字段设置微LCP对应的代码， 而信息字段包含特定的配置请求。 链路的另一端可以发送以下的响应:
        - 配置确认帧-所有选项都接收
        - 配置否认帧-所有选项都理解但不能接收
        - 配置拒绝帧-选项有的无法识别或不能接收， 需要协商
   - LCP配置选项包括链路上的最大帧长, 所使用的鉴别协议的规则， 以及不适用PPP帧中的地址和控制字段。协商结束后，双方就建立了LCP链路，接着进入认证阶段，在这一状态下，只允许传送LCP分组、认证协议的分组及检测链路质量的分组。若使用口令鉴别协议， 则需要发起通信的一方发送身份标志符和口令。系统允许用户重试多次。如果要求安全性， 则可以使用复杂的握手认证协议， 若身份鉴别失败， 则转到链路终止协议状态， 若鉴别成功， 则进入到网络层协议状态。
  - 在网络层协议状态， PPP链路的两端的网络控制协议NCP根据不同协议互相交换网络层特定的网络控制分组。这个步骤重要， 因为现在的路由器都能够同时支持多种的网络层协议。总之， PPP协议的两端可以运行不同的网络层协议，但仍然可以使用统一个PPP协议进行通信。如果在PPP链路上运行的是IP协议， 则对PPP链路的每一端配置IP协议模块， IPCP分组也封装成PPP帧在PPP链路上传送。当网络层配置完毕后，链路就进入可进行数据通信的链路打开状态。链路的两个PPP端点可以彼此向对方发送分组。两个PPP端点还可以发送会送请求LCP分组和回送回答LCP分组，以检查链路的状态。数据传输结束后，可以由链路的一段发送终止请求LCP分组请求终止链路连接，在接收到对方发送来的终止确认LCP分组后，就转到链路终止状态。如果链路出现故障，则也会从链路打开到链路终止状态。当调制解调器的载波结束后，则回到链路静止状态。
