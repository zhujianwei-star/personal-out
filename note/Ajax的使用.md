
# 概念

ASynchronous JavaScript And XML        异步的javascript和XML

1.同步和异步的概念
同步：客户端必须等待服务器端响应。在等待的期间客户端不能做其他操作
异步：客户端不需要等待服务器端做出响应。在服务器请求的过程中，客户端可以进行其他操作。
2.ajax：
AJAX是一种无需在加载整个网页的情况下，能够更新部分网页的技术。
通过在后台与服务器进行少量的数据交换，Ajax可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。传统网页（不适用Ajax）如果需要更新内容，必须重新加载整个页面。
可以提升用户的体验。



# 实现方式

## 原生js方式
 
1. 创建核心对象
   代码示例：
``` js
var xmlhttp;
if (window.XMLHttpRequest) { // 判断浏览器是否为IE6以下的版本
	xmlhttp = new XMLHttpRequest();
} else { // 当浏览器为IE6，IE5时会调用这段
	xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");

}
```

2. 建立连接
   语法：open(method,url,async)
   参数列表：
   method：method中表示get还是post方法
   url：请求的文件资源路径
   async：传递的参数为boolean值，true表示异步，false表示同步
   代码示例：
   `xmlhttp.open("get","ajaxServlet?username=tom",true);`

3. 发送连接
   语法：send()
   参数：当请求方式为POST请求时，请求需要传递的参数不能使用在url路径后面拼键值对的方式，参数需要作为入参传递进入send()方法中
   代码示例：
   `xmlhttp.send();`

4. 获取返回
   变量：responseText;
   服务器返回的数据会使用XMLHttpRequest对象中的responseText变量存储。
   代码示例：
```js
xmlhttp.onreadychange = function() {
	if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
		/*
			readyState表示XMLHttpRequest的状态，值为0~4
				0：请求未初始化
				1：服务器连接已建立
				2：请求已接收
				3：请求处理中
				4：请求已完成，且响应已就绪
			status表示请求响应的状态 200成功
		*/
		var resp = xmlhttp.responseText;
	}
}
```


## jQuery实现方式：

1.$.ajax()
语法：`$.ajax({键值对});`
代码示例：
```js
$.ajax({
	url:"ajaxSerlvet", // 请求资源的url路径和别的参数用逗号“,”隔开
	type:"post", // 请求的方式，默认为get，可以修改为post，其他的请求方式部分浏览器不支持，不建议使用
	// date:"username=jack&age=23", // 请求的参数，有两种写法，还可以使用json格式的字符串来书写。
	date:{"username":"jack", "age":23}, // 请求的参数的json格式的写法，推荐使用这种方式书写
	success:function () {}, // 请求成功后的回调函数
	error:function () {}, // 请求失败后的回调函数
	dateType:"text" // 设置接收到的响应数据的格式，最后一个参数不写逗号

});
```

2.$.get()
语法：
`$.get(url,[data],[callback],[type])`
参数列表:
url：请求资源的url路径
data：传递给服务器的参数，参数为json格式
callback：回调函数
type：响应结果的类型

3.$.post()
语法：
`$.post(url,[data],[callback],[type])`
参数列表:
url：请求资源的url路径
data：传递给服务器的参数，参数为json格式
callback：回调函数
type：响应结果的类型
