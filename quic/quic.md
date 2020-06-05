title: QUIC
speaker: wsf
url: 

<slide class="bg-black-blue aligncenter" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">

## QUIC 
(Quick UDP Internet Connections) {.animated.fadeInDown.delay-800}


<slide class="aligncenter">
### QUIC是什么？

:::column {.vertical-align}

快速UDP网络连接（英语：Quick UDP Internet Connections），是一种实验性的网络传输协议。Google希望使用这个协议来取代HTTPS/HTTP协议，使网页传输速度加快。{.text-description}
 
---

QUIC是由Google开发，  
旨在提供基于TLS/DTLS的网络安全保护，  
减少数据传输及创建连线时的延迟时间，  
双向控制带宽，以避免网络拥塞。

:::

<note>
1. 读音是quick
2. quic是一个传输层协议
</note>
<slide class="aligncenter"> 

:::{.content-center}
### QUIC优点

- 减少了 TCP 三次握手及 TLS 握手时间 
- 多路复用，避免队头阻塞  
- 快速迭代，广泛支持  
- 连接迁移：当移动设备的用户从WiFi热点切换到移动网络时发生的情况。 当这发生在TCP上时，一个冗长的过程开始了：每个现有连接一个接一个地超时，然后根据需要重新创建
:::

<slide>
!![](https://miniwsf.github.io/study-nodeppt/quic/image/WechatIMG1.png .size-40.alignleft)

:::{.content-right}
### Why QUIC？

:::flexblock {.specs}

### 协议历史悠久导致中间设备僵化

---

### 依赖于操作系统的实现导致协议本身僵化

---

### 建立连接的握手延迟大

---

### 队头阻塞

:::

<slide>

:::{.content-center}
### Why QUIC——建立连接的握手延迟

:::flexblock

### 网络延迟

处理延迟-路由器处理数据包头所需的时间   
排队延迟-数据包在路由队列中花费的时间  
传输延迟-将数据包的比特推送到链路上所花费的时间  
传播延迟-信号到达目的地的时间  

`RTT（往返时间）：是网络请求从起点到目的地并再次回到起点所花费的持续时间（以毫秒为单位）——衡量网络延迟的重要指标`

:::

<note>
- 处理延迟  -路由器处理数据包头所需的时间（在处理数据包期间，路由器可以检查传输过程中发生的数据包中的位级错误，并确定数据包的下一个目的地在哪里。高速路由器中的处理延迟通常约为微秒或更短。经过此节点处理后，路由器会将数据包定向到可能发生进一步延迟（排队延迟）的队列）
- 排队延迟  –数据包在路由队列中花费的时间（当数据包到达路由器时，必须对其进行处理和传输。路由器一次只能处理一个数据包。如果数据包到达的速度比路由器处理数据包的速度快（例如在突发传输中），则路由器会将其放入队列（也称为缓冲区）中，直到可以绕开传输它们为止）
- 传输延迟  –将数据包的比特推送到链路上所花费的时间（由链路的数据速率引起的延迟）
- 传播延迟  –信号到达目的地的时间（传播延迟是指信号头从发送器传播到接收器所花费的时间）传播时延 = 信道长度(m) / 电磁波在信道上的传播速率(m/s)。例如，cdn主要就是为了减少传播的距离从而减少传播延迟

距离说明：
接收快递：
1. 快递中间点就是路由器，快递就是数据包，交通方式就是我们的物理介质（铜线，光缆这些），就是分组
2. 处理延迟：就是每个服务站需要检查驾照啊或者看车内有没有什么违禁品啊是否酒驾等等这期间的时间
3. 排队延迟：处理其他人的时候，需要排队等待的时间
4. 传输延迟：xxx
5. 传播延迟：假如高速公路速度是60km/h，泥土路：10km/h，飞机xxx
</note>
<slide>

:::column {.sm.vertical-align}

!![](https://miniwsf.github.io/study-nodeppt/quic/image/chrome_waterfall.png .alignleft)

--- 

| 延迟时间（单位ms） | 用户反应 |
| :----------- | :------------: |
| 0 - 16  |   用户可以感知每秒渲染 60 帧的平滑动画转场。也就是每帧 16 毫秒，留给应用大约 10 毫秒的时间来生成一帧。   |
| 0 - 100 |    在此时间窗口内响应用户操作，他们会觉得可以立即获得结果。时间再长，操作与反应之间的连接就会中断。    | 
| 100 - 300  |   用户会遇到轻微可觉察的延迟。   |
| 300 - 1000  |   在此窗口内，延迟感觉像是任务自然和持续发展的一部分。对于网络上的大多数用户，加载页面或更改视图代表着一个任务。   |
| 1000+  |   超过 1 秒，用户的注意力将离开他们正在执行的任务。   |
| 10,000+  |   用户感到失望，可能会放弃任务；之后他们或许不会再回来。   |

:::
<note>
1. 初始化连接包含tcp三次握手+tsl认证（四次握手），上图占用了我们整个请求时间的80%。
2. 在用户的感知行为中，100ms应该能感觉到延迟了，目前初始化连接占用的时间已经是不能忍受的了
</note>
<slide>

:::flexblock {.clients.border}

 {.blacklogo}
### TCP
TCP快启动  
`允许在SYN和SYN-ACK中携带数据，数据包和接收端在初始阶段消耗的数据包。连接握手，最多可节省一个完整的往返时间（RTT）`

**缺点**  
`兼容差（TCP是操作系统内核实现）`  
`隐私问题`

---

 {.blacklogo}
### TSL
TSL/1.3 

首次：2RTT减少至1RTT  
0-RTT：如果客户端和服务器之前已在彼此之间建立TLS连接，则它们可以使用从该会话中缓存的信息来建立新的TLS，而不必从头开始协商连接的参数。

---

 {.blacklogo}
### HTTP
HTTP/1.1   
`持久连接（Connetion: keep-alive）——减少tcp连接数`

HTTP/2  
`多路复用 ——减少tcp连接数`

**缺点**  
`队头阻塞问题`  
`没有根本解决TCP连接延迟问题`

:::
<note>
- TCP连接：用于保证可靠性和流控制机制的数据，包括 Socket、序列号以及窗口大小。（通过三次握手才能阻止重复历史连接的初始化）
1.syn: Synchronize Sequence Numbers（同步序列编号）
2.ack: Acknowledge character(确认字符)
</note>
<slide>
### QUIC实现0-RTT

:::column {.vertical-align}
- 基于UDP（面向无连接的），没有三次握手过程
- 首次进入：合并原有的TCP连接和TLS认证过程，将TCP(1RTT)+TSL(2RTT)减少为1RTT，后续进入实现0-RTT

--- 

!![](https://lh3.googleusercontent.com/o62Ohn1Ppxna6zz0NtavqRyetjryOj-81Sz4bRt3U8lURVblk5RKOaCcf57i6BkmprremePJpq_sQcxfJiuA4wJBmRp3pR4BS1P-yiT6UNUPvnBeP_rLz9bvHxFE15kuNBM2hpE)

:::

<slide>

:::column {.vertical-align}

TCP+TLS\:

!![](https://www.yinchengli.com/wp-content/uploads/2018/06/8.png)

---
QUIC\:

!![](https://www.yinchengli.com/wp-content/uploads/2018/06/9.png)
:::

（chrome数据：QUIC在糟糕的网络条件下胜过TCP，将Google搜索页面的加载时间缩短了整整一秒钟，这是最慢的1％的连接。对于YouTube等视频服务而言，这些优势更加明显。通过QUIC观看视频时，用户报告的重新缓冲减少了30％）

<slide>
!![](https://miniwsf.github.io/study-nodeppt/quic/image/WechatIMG1.png .size-40.alignleft)
:::{.content-right}

### Why QUIC——队头阻塞

:::flexblock {.specs}

{.bg-trans-dark}
### 队头阻塞
`（Head-of-line blocking）`   
当顺序发送的请求序列中的一个请求因为某种原因被阻塞时，在后面排队的所有请求也一并被阻塞

<slide :class="size-80">

## HTTP历程  
`仅针对连接逻辑`

---
:::steps

## HTTP/1.0

一次只能发起一个请求，下一个请求需要等上一个请求收到响应之后才能继续。

---

## HTTP/1.1

流水线（pipelining）：多个请求整批提交（一般浏览器允许的最大连接为6个）

---

## HTTP/2

多路复用（Multiplexing）：在一个信道上传输多路信号或数据流的过程和技术

:::

<slide>
:::column {.vertical-align}

HTTP/1.1\:

!![](https://freecontent.manning.com/wp-content/uploads/HTTP-vs-with-Push-HTTP1.gif)

---
HTTP/2\:

!![](https://freecontent.manning.com/wp-content/uploads/HTTP-vs-with-Push-HTTP2.gif)
:::


<slide>
:::{.content-center}
## 多路复用解决队头阻塞

- QUIC使用UDP协议作为其基础，不包括丢失恢复
- 每个QUIC流是单独控制的，并且在QUIC级别而不是UDP级别重传丢失的数据。这意味着如果在一个流中发生错误，协议栈仍然可以独立地继续为其他流提供服务

<slide class="aligncenter">
:::div {.text-cols}

已经应用的公司：  
`Google`
`youtube` 
`腾讯` 
`快手`

---

HTTP/3：使用QUIC协议实现

<slide class="aligncenter">

感激～  
over

