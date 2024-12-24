
# Array.prototype.includes()方法

Includes方法用来检测数组中是否包含某个元素，返回boolean类型的值

~~~ es7
	// 1.Array.prototype.includes()方法
    // Includes方法用来检测数组中是否包含某个元素，返回boolean类型的值
    // 与indexOf()方法相似，但是indexOf()方法返回的是-1
    const mingzu = ['西游记', '红楼梦', '水浒传', '三国演义'];
    console.log(mingzu.includes('金瓶梅')); // false
~~~

# `**` 指数操作符

ES7中引入指数运算符【`**`】，用来实现幂运算，功能与Math.pow相同

~~~ es7
    // 2.** 指数操作符
    // ES7中引入指数运算符【**】，用来实现幂运算，功能与Math.pow相同
    console.log(2 ** 10); // 1024
    console.log(Math.pow(2, 10)); // 1024
    console.log(2 ** 10 === Math.pow(2, 10)); // true
~~~

