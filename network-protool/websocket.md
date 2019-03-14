# websocket

> 是一个支持 c-s 全双工的通信协议

## 参考文章
https://juejin.im/post/5c7cdaabf265da2daf79c15f

## 特点
1. 建立在tcp 协议之上 
2. 与http协议有很好的兼容性，使用80和443端口,握手阶段使用http协议
3. 数据格式轻量
4. 可发送文本，也可发送二进制
5. 浏览器没有同源限制
6. 协议标识为 ws , 加密为wss 

## 握手过程
