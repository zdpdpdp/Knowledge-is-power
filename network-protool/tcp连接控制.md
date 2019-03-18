## TCP 首部报文格式

2字节源端口 2字节目的端口
4字节序列号
4字节确认号
4位首部长度

URG ACK PSH RST SYN FIN 
2字节窗口大小

### RST
[参考文章](https://www.cnblogs.com/JohnABC/p/6323046.html)

## TCP连接过程

1. client 发送SYN=1 seq=x 连接申请 进入syn_sent 阶段
2. server 接受后，发送SYN=1 ACK=1 seq=y ack=x+1 进入syn_revd 阶段
3. client 发送ACK=1 ack=y+1 如果带了数据， seq 要增加多一个序号

### 为什么TCP需要三次握手？

一次明显不够，四次多余了。 为什么不是两次呢？ 

TCP三次握手是让双方都确认对方有发送，接受消息的能力

第三次握手确认是防止旧的握手请求再次到达 server 端，导致 server 无端建立了一个连接，耗费资源

## TCP挥手过程

1. client FIN=1 seq=x 进入FIN_WAIT_1
2. server ACK=1 seq=y ack=x+1 server 进入CLOSE_WAIT client进入FIN_WAIT_2
//服务端赶紧把没传完的数据传完
3. server FIN=1 ACK=1 seq=w ack=x+1 server进入LAST_ACK 
4. client ACK=1 seq=x+1 ack=w+1 client进入TIME_WAIT server CLOSE

### 为什么需要四次挥手？

server 端是被动分手的，所以收到分手消息后， 自己还要把没发完的消息赶紧发完，发完以后要进行确认

### 客户端为什么要进入TIME_WAIT

1. 让所有消息在2MSL 阶段内都基本消亡
2. client 发完最后一个包以后， 如果马上关闭了，那么 server 可能没收到， 这时候 server 会重新发第三个包，如果 client 响应不了， 那么 server 又断不了了


### 服务端 close wait 过多是什么原因导致的？

一般都是程序写了 bug，没有关闭 socket 连接, 

可以通过设置连接的访问和超时时间

可以调整tcp 内核tcp/ip 参数来规避问题， 但主要还是要从代码本身去解决