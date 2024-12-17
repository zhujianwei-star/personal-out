
# 概念

Cascading Style Sheets 层叠样式表
层叠：多个样式可以作用在同一个html的元素上，同时生效。

# 好处

1. 功能强大
2. 将内容展示和样式控制分离
   降低耦合度，解耦。
   让分工协作更容易。
   提高开发效率。

# CSS的使用，引入CSS的3种方式

## 内联样式

在标签内使用style属性来指定css代码。
如：`<div style="color: red;">hello css</div>`

## 内部样式

在head标签内，定义style标签，style标签的标签体内容就是css代码。
```HTML
<head>
	<style>
		div{
			color: blue;
			 color:rgb(255,0,0);
			 color:#FFFF00;
		}
	</style>
  </head>
  <body>
	<div>hello css</div>
  </body>
```

### 外部样式

1. 定义css资源文件。
2. 在head标签内，定义link标签，引入外部的资源文件。
``` html
a.css文件：
div{
	color: green;
}

html文件：
<head>
	<link rel="stylesheet" href="css/a.css">
</head>
<body>
	<div>hello css</div>
</body>
```

> 注意：1，2，3种方式，css作用的范围越来越大
              1种方式不常用，后期常用2，3
              3种方式还可以写为：
        `<style> @import "css/a.css"; </style>`
        不过一般不常用。

# css语法

格式：
选择器 {
属性名1: 属性值1;
属性名2: 属性值2;
......
}
选择器：筛选具有相似特征的元素
注意：
每一对属性值需要使用;隔开，最后一对属性可以不加「;」，但是一般都会加上「;」。

