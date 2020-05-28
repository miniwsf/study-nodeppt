title: QUIC
speaker: wsf
url: 

<slide class="bg-black-blue aligncenter" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">

QUIC {.text-intro.animated.fadeInDown.delay-800}
- 是什么？
- 为什么？
- 怎么做？

<slide image="./image/WechatIMG1.png .right-bottom">

:::{.content-left}
### QUIC是什么？

:::flexblock {.specs}

快速UDP网络连接（英语：Quick UDP Internet Connections），是一种实验性的网络传输协议。

---

QUIC是由Google开发，旨在提供基于TLS/DTLS的网络安全保护，减少数据传输及创建连线时的延迟时间，双向控制带宽，以避免网络拥塞。

---
Google希望使用这个协议来取代HTTPS/HTTP协议，使网页传输速度加快。

:::

<slide>
!![](https://miniwsf.github.io/img/20180917/1522739493016.jpg .size-40.alignleft)
:::{.content-right}
### 为什么出现QUIC？

:::flexblock {.specs.bg-trans-dark}

### 连接延迟

`在建立连接期间，TCP和TLS/SSL通常需要一个或多个往返时间（RTT）`

RTT：从发送端发送数据开始，到发送端收到来自接收端的确认（接收端收到数据后便立即发送确认），总共经历的时延。{.animated.fadeInUp.delay-800}

<slide>
### 实现0-RTT

!![](https://lh3.googleusercontent.com/o62Ohn1Ppxna6zz0NtavqRyetjryOj-81Sz4bRt3U8lURVblk5RKOaCcf57i6BkmprremePJpq_sQcxfJiuA4wJBmRp3pR4BS1P-yiT6UNUPvnBeP_rLz9bvHxFE15kuNBM2hpE .size-40.aligncenter)

:::{.content-center}
QUIC的设计宗旨,如果客户端以前已经与给定服务器进行了对话，则它可以开始发送数据而无需进行任何往返操作，从而可以更快地加载网页

<slide>

!![](https://www.yinchengli.com/wp-content/uploads/2018/06/8.png .size-50.aligncleft)

!![](https://www.yinchengli.com/wp-content/uploads/2018/06/9.png .size-50.aligncright)

<slide>
!![](https://miniwsf.github.io/img/20180917/1522739493016.jpg .size-40.alignleft)
:::{.content-right}
### 为什么出现QUIC？

:::flexblock {.specs}

### 连接延迟

---

{.bg-trans-dark}
### 队头阻塞

队头阻塞——一个TCP分节丢失，导致其后续分节不按序到达接收端的时候。

`HTTP/2实现多路复用：多个HTTP请求共用一个TCP连接，以减少TCP连接数，达到复用高速信道的作用。但是TCP连接中的单个丢失数据包使该连接上的所有多路复用停顿下来`{.animated.fadeInUp.delay-800}

<slide>
:::{.content-center}
## 多路复用解决队头阻塞

QUIC使用UDP协议作为其基础，不包括丢失恢复。相反，每个QUIC流是单独控制的，并且在QUIC级别而不是UDP级别重传丢失的数据。这意味着如果在一个流中发生错误，协议栈仍然可以独立地继续为其他流提供服务

<slide>
!![](https://miniwsf.github.io/img/20180917/1522739493016.jpg .size-40.alignleft)
:::{.content-right}
### 为什么出现QUIC？

:::flexblock {.specs}

### 连接延迟

---

### 队头阻塞

---

网络切换 {.text-subtitle.animated.fadeInUp.delay-800}

当移动设备的用户从WiFi热点切换到移动网络时发生的情况。 当这发生在TCP上时，一个冗长的过程开始了：每个现有连接一个接一个地超时，然后根据需要重新创建 {.animated.fadeInUp.delay-800}

<slide>
:::{.content-center}
## 网络切换
QUIC包含一个连接标识符，该标识符唯一地标识客户端与服务器之间的连接，而无论源IP地址是什么。这样只需发送一个包含此ID的数据包即可重新创建连接，因为即使用户的IP地址发生变化，原始连接ID仍然有效

<slide class="aligncenter">
:::div {.text-cols}

已经应用的公司：  
`Google`
`youtube` 
`腾讯` 
`快手`

感兴趣的话，可以细看下面的资料  
[快速UDP网络连接](https://zh.wikipedia.org/wiki/%E5%BF%AB%E9%80%9FUDP%E7%BD%91%E7%BB%9C%E8%BF%9E%E6%8E%A5)  
[Draft-ietf-quic-transport-20](https://tools.ietf.org/html/draft-ietf-quic-transport-20)  
[关于Google实验性运输的QUIC更新](https://blog.chromium.org/2015/04/a-quic-update-on-googles-experimental.html)  

<slide class="aligncenter">

感激～  
over

