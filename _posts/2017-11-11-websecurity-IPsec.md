---
layout:     post
title:      "IPSec 传输模式下ESP报文的装包与拆包过程"
subtitle:   "web安全专题"
date:       2017-11-11
author:     "tomylee"
header-img: "img/1217913296033.jpg"
catalog: true
tags:
    - Web Security
---

### 一、IPsec简介
IPSec ( IP Security )是IETF（Internet Engineering Task Force，Internet工程任务组）的IPSec小组建立的一组IP安全协议集。IPSec定义了在网络层使用的安全服务，其功能包括数据加密、对网络单元的访问控制、数据源地址验证、数据完整性检查和防止重放攻击。
IPSec是安全联网的长期方向。它通过端对端的安全性来提供主动的保护以防止专用网络与 Internet 的攻击。在通信中，只有发送方和接收方才是唯一必须了解 IPSec 保护的计算机。在 Windows XP 和 Windows Server 2003 家族中，IPSec 提供了一种能力，以保护工作组、局域网计算机、域客户端和服务器、分支机构（物理上为远程机构）、Extranet 以及漫游客户端之间的通信。
### 二、ESP简介
IPsec 封装安全负载(IPsec ESP)是 IPsec 体系结构中的一种主要协议，其主要设计来在 IPv4 和 IPv6 中提供安全服务的混合应用。IPsec ESP 通过加密需要保护的数据以及在 IPsec ESP 的数据部分放置这些加密的数据来提供机密性和完整性。且ESP加密采用的是对称密钥加密算法，能够提供无连接的数据完整性验证、数据来源验证和抗重放攻击服务。根据用户安全要求，这个机制既可以用于加密一个传输层的段(如:TCP、UDP、ICMP、IGMP)，也可以用于加密一整个的 IP 数据报。封装受保护数据是非常必要的，这样就可以为整个原始数据报提供机密性。ESP提供机密性、数据起源验证、无连接的完整性、抗重播服务和有限业务流机密性。该协议能够在数据的传输过程中对数据进行完整性度量，来源认证以及加密，也可以防止回放攻击。
传输模式是IPSec工作的两种方式之一。与隧道模式不同的是，保护的仅仅是真正传输的数据，而不是整个IP报文。在处理的方法上，原来的IP报文会先被解开，再在数据前加上新的ESP或AH协议头，最后再装回原来的IP头中，即原来的IP包被修改过再传输了。

### 三、传输模式下ESP报文的装包与拆包过程
在传输模式下的IPSec ESP报文的结构图如下：
![这里写图片描述](http://img.blog.csdn.net/20171111203814419?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM0NTQxMTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 装包过程：
```
1.在原IP报文末尾添加尾部(ESP trailer)。尾部信息如图所示，包含三部分。由于所选加密算法可能是块加密，当最后一块长度不够时，需要进行补充(padding)。且需要附上填充长度(Pad length)来方便解包时顺利找出用来填充的那一段数据。而Next header则用来表明被加密的数据报文的类型。
```
![这里写图片描述](http://img.blog.csdn.net/20171111203952222?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM0NTQxMTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
```
2.将原数据报文和刚添加的ESP尾部信息作为一个整体进行加密，具体的加密算法由密钥和SA给出。            
3.在第2步得到的加密数据前添加ESP Header。ESP Header由SPI和序号(Sequence number)两部分组成。加密数据与ESP头合称为”enchilada”。
4.附加完整性度量结果(ICV, Integrity check value)。对第三步得到的”enchilada”做摘要，得到一个完整性度量值，并附在ESP报文的尾部。
5.将原IP头放回到第4步后形成的报文的头部前，组织成一个新的IP报文。
```

#### 拆包过程：
```
1.接收方收到数据报文后，发现协议类型是50，知道这是一个IPSec包。首先查看ESP头，通过里面的SPI决定数据报文所对应的SA。
2.计算”enchilada”部分的摘要，与附在末尾的ICV做对比，如果一样，说明数据完整；否则断定收到的报文已经不是原来的报文了。
3.检查Seq里的顺序号，保证数据是”新鲜”的，不是回放攻击。
4.根据SA所提供的加密算法和密钥，解密被加密过的数据 ”enchilada”。得到原IP报文的数据部分和ESP尾部(trailer)。
5.根据ESP尾部的填充长度信息，可以找出填充字段的长度，删去后就得到原来的IP报文。
6.根据获取的原IP包目标地址进行转发。 
```

