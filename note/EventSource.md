# 概念

> 取自MDN
1. 定义了所有处理与服务器连接、接收事件/数据、处理错误、关闭连接等功能的特性。
2. EventSource 接口是 web 内容与服务器发送事件通信的接口。
3. 一个 EventSource 实例会对 HTTP 服务器开启一个持久化的连接，以 text/event-stream 格式发送事件，此连接会一直保持开启直到通过调用 EventSource.close() 关闭。