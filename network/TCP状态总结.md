## TCP总结



![TCP通信流程](https://github.com/fpagyu/notebook/raw/master/images/tcp-ip-handshark.png)



###  三次握手

如上图所示，客户端初始化序列号为x，服务端初始化序列号为y。

1.首先客户端调用connect主动发起连接，会向服务端发送一个包含x序列号的SYN分节，告诉服务端要建立一个连接。 

2.服务端接收到SYN信号，如果没有问题则向客户端发送一个包含y序列号的SYN分节和一个x+1的ACK响应，告诉客户端我收到你发起的连接请求了。

3.客户端收到服务端的ACK响应，然后向服务端发送一个y+1的ACK响应，此时tcp连接进入ESTABLISHED状态。



### 四次挥手

1.其中一端(可以是客户端，也可是服务端)调用close, 主动关闭连接。向对端发送一个x+2的FIN信号和一个y+1的ACK信号。连接处于FIN_WAIT_1状态。

2.对端接收到主端发送的断开连接请求进入CLOSE_WAIT状态，首先会向主端发送一个x +3的ACK响应，连接进入FIN_WAIT_2  状态。

3.接着继续调用close，向主端发送一个y+1 的FIN信号, 同时连接进入LASK_ACK状态

4.主端收到对端的FIN信号，连接进入TIME_WAIT状态。同时向对端发送y+2的ACK响应



### 状态解释

| 状 态       | 描 述                                              |
| ----------- | -------------------------------------------------- |
| CLOSED      | 关闭状态，没有连接活动或正在进行                   |
| LISTEN      | 监听状态，服务器正在等待连接进入                   |
| SYN RCVD    | 收到一个连接请求，尚未确认                         |
| SYN SENT    | 已经发出连接请求，等待确认                         |
| ESTABLISHED | 连接建立，正常数据传输状态                         |
| FIN WAIT 1  | （主动关闭）已经发送关闭请求，等待确认             |
| FIN WAIT 2  | （主动关闭）收到对方关闭确认，等待对方关闭请求     |
| TIMED WAIT  | 完成双向关闭，等待所有分组死掉                     |
| CLOSING     | 双方同时尝试关闭，等待对方确认                     |
| CLOSE WAIT  | （被动关闭）收到对方关闭请求，已经确认             |
| LAST ACK    | 被动关闭）等待最后一个关闭确认，并等待所有分组死掉 |