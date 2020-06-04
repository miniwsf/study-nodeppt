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

QUIC是由Google开发，旨在提供基于TLS/DTLS的网络安全保护，减少数据传输及创建连线时的延迟时间，双向控制带宽，以避免网络拥塞。  

:::

<slide>
!![](https://miniwsf.github.io/study-nodeppt/quic/image/WechatIMG1.png .size-40.alignleft)

:::{.content-right}
### Why QUIC？

:::flexblock

### 网络延迟

网络延迟包含：处理延迟，排队延迟，传输延迟，传播延迟

`RTT（往返时间）：是网络请求从起点到目的地并再次回到起点所花费的持续时间（以毫秒为单位）——衡量网络延迟的重要指标`

:::

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

<slide>
### QUIC实现0-RTT

:::column {.vertical-align}
- 基于UDP（面向无连接的），没有三次握手过程
- 首次进入：合并原有的TCP连接和TLS认证过程，将TCP(1RTT)+TSL(2RTT)减少为1RTT，后续进入实现0-RTT（0-RTT存在一定
安全风险，可以默认不开启）

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

### Why QUIC？

:::flexblock {.specs}

### 网络延迟

---

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

QUIC使用UDP协议作为其基础，不包括丢失恢复。相反，每个QUIC流是单独控制的，并且在QUIC级别而不是UDP级别重传丢失的数据。这意味着如果在一个流中发生错误，协议栈仍然可以独立地继续为其他流提供服务

<slide>
!![](https://miniwsf.github.io/study-nodeppt/quic/image/WechatIMG1.png .size-40.alignleft)
:::{.content-right}
### Why QUIC？

:::flexblock {.specs}

### 网络延迟

---

### 队头阻塞

---

### 其他 {.text-subtitle.animated.fadeInUp.delay-800}

- 网络切换：当移动设备的用户从WiFi热点切换到移动网络时发生的情况。 当这发生在TCP上时，一个冗长的过程开始了：每个现有连接一个接一个地超时，然后根据需要重新创建 {.animated.fadeInUp.delay-800}

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

