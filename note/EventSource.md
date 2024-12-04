# 概念

> 取自MDN
1. 定义了所有处理与服务器连接、接收事件/数据、处理错误、关闭连接等功能的特性。
2. EventSource 接口是 web 内容与服务器发送事件通信的接口。
3. 一个 EventSource 实例会对 HTTP 服务器开启一个持久化的连接，以 text/event-stream 格式发送事件，此连接会一直保持开启直到通过调用 EventSource.close() 关闭。
4. 一旦连接开启，来自服务端传入的消息会以事件的形式分发至你代码中。如果接收消息中有一个 event 字段，触发的事件与 event 字段的值相同。如果不存在 event 字段，则将触发通用的 message 事件。

# 与WebSocket的区别

## 相似点
都是建立浏览器与服务器之间的通信渠道，然后服务器向浏览器推送信息

## 不同点

### WebSocket的优点

1. WebSocket 是全双工的，EventSource 是单向的。
2. WebSocket 更强大和灵活。因为它是全双工通道，可以双向通信,EventSource是单向通道，只能服务器向浏览器发送，因为流信息本质上就是下载。如果浏览器向服务器发送信息，就变成了另一次 HTTP 请求。

### EventSource 的优点

1. SSE 使用 HTTP 协议，现有的服务器软件都支持。WebSocket 是一个独立协议。
2. SSE 属于轻量级，使用简单；WebSocket 协议相对复杂。
3. SSE 默认支持断线重连，WebSocket 需要自己实现。
4. SE 一般只用来传送文本，二进制数据需要编码后传送，WebSocket 默认支持传送二进制数据。
5. SSE 支持自定义发送的消息类型。

# EventSource对象
> 取自MDN
1. EventSource 对象表示一个事件源（event source）。
2. EventSource 对象用于创建与服务器的连接，并从服务器接收数据流。该流会包含一系列的数据块，这些数据块会被解释为 text/event-stream 类型的文本。
3. EventSource 对象是 HTML5 中引入的一个新接口，它允许 JavaScript 脚本通过一段简单的代码来监听服务器端的推送消息。

# EventSource对象属性
1. readyState：返回当前状态，0 表示未连接，1 表示正在连接，2 表示已经连接，3 表示已经关闭。
2. withCredentials：设置是否需要验证信息，默认为 false。

# EventSource对象方法
1. close()：关闭连接。
2. getURL()：获取 URL。
3. connect(): 连接服务器。
4. send(data): 发送数据。

# EventSource对象事件
1. on