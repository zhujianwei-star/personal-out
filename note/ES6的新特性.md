
# Let关键字

## let变量声明以及声明特性

### 1.变量不能重复声明

```js
使用let声明  
let a = "1";  
let a = "2";  
这样在浏览器中会报错  
但是使用var声明  
var a = "1";  
var a = "2";  
浏览器不会报错
```

### 2.块儿级作用域

```js
es5中的作用域分为全局，函数，eval三个，es6中增加了块级作用域  
{  
    let a = '1';  
}  
console.log(a);  
浏览器会报错  
但是使用  
{  
    var a = '1';  
}  
console.log(a);  
浏览器不会报错，而且会输出1
```

### 3.不存在变量提升

```js
console.log(a);  
let a = '1';  
浏览器报错  
console.log(a);  
var a = '1';  
浏览器不会报错，而会输出一个undefined，因为使用var的时候就相当于在前面进行了一个var a;的声明
```

### 4.不影响作用域链

```js
{  
    let a = '1';  
    fn() {  
        console.log(a);  
    }  
    fn();  
}  
浏览器会正常输出1，浏览器在函数体中找不到a，还是会往前找的。
```

# const关键字

## const声明常量

1. 一定要赋初始值
   不赋初始值的话会报错
2. 一般常量使用大写(潜规则，不会报错)
3. 常量的值不能修改，否则会报错
4. 块级作用域
5. 对于数组和对象的元素修改不会报错
   因为修改数组或对象的元素，常量指向的地址值没有变化，所以不算做对常量的修改，所以不会报错

# 解构赋值

## 定义：

​	ES6允许按照一定模式从数组和对象中提取值，对变量进行赋值，这就是解构赋值。

## 1.数组的解构

~~~ es6
const ITEM = ['ha','hei','he'];
let [a,b,c] = ITEM
console.log(a); // ha
console.log(b); // hei
console.log(c); // he
~~~

## 2.对象的解构

~~~ es6
const zhao = {
	name: '赵本山',
	age: '不详',
	xiaopin: () => {
		console.log('出演小瓶');
	}
}
let {name, age, ccc, xiaopin} = zhao;
console.log(name); // 赵本山
console.log(age); // 不详
console.log(ccc); // undefined
xiaopin();	// 出演小瓶
~~~

​**注意：当使用对象解构时，必须将变量名与对象中的属性名对应，如果变量名在属性名中对应不上，则只会初始化，不会赋值**

# 模板字符串

## 定义

ES6中引入新的声明字符串的方式	\` \`	，ES6之前只能使用	''	或者	""	来声明字符串，ES6就可以使用反引号	\` \` 来声明字符串，而使用反引号声明的字符串就叫做模板字符串。

## 特性

### 1.内容中可以直接出现换行符

~~~ es6
let str = `<ul>
				<li>aaa</li>
				<li>bbb</li>
			</ul>`;
这样写不会报错
但是使用'' 或者"" 声明就会报错
~~~

### 2.可以直接进行变量拼接

~~~ es6
let a = '费翔';
let b = `${a}是我最喜欢的喜剧演员`;
console.log(b);		// 费翔是我最喜欢的喜剧演员
~~~

# 简化对象写法

## 1.将变量存入对象

~~~ es6
let a = 'ccc';
let b = function() {
	console.log('I am changing');
}
let me = {
	a,
	b
}
------------------
ES6以前需要写：
let me = {
	a: a,
	b: b
}
可以简化书写
~~~

## 2.对象中的函数的简化

~~~ es6
let me = {
	improve: function() {
		console.log('aaa');
	}
}
-----------
ES6中简化书写：
let me = {
	improve() {
		console.log('aaa');
	}
}
~~~

# 箭头函数

## 定义：

ES6中允许使用箭头 => 定义函数。

~~~ es6
如:function() {}
可以写为 () => {}
~~~

## 箭头函数与普通function() {}函数定义的区别

### 1. this是静态的

~~~ es6
ES6中的箭头函数中的this是静态的，this始终指向函数声明所在作用域下的this的值。不是指向作用域。
function函数中this指向的是函数的调用者。
如：
window.name = 'aaa';
let school = {
	name: 'bbb'
}
let fn = () => {
	console.log(this.name);
}
let fn1 = function() {
	console.log(this.name);
}

fn();  // aaa
fn1(); // aaa
// call 方法可以用来代替另一个对象调用一个方法。call 方法可将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。也就是说相当于是使用school对象调用当前方法。
fn.call(school);	// aaa
fn1.call(school); 	// bbb
~~~

### 2. 不能作为构造函数实例化对象

~~~ es6
let Person = (name, age) => {
	this.name = name;
	this.age = age;
}
let persion = new Person('aaa', 23);
这样写浏览器会报错
但是function函数可以
~~~

### 3. 不能使用arguments变量

~~~ es6
let func = () => {
	console.log(arguments);
}
浏览器会报错
~~~

### 4. 箭头函数可以简写

#### 1. 当形参有且只有一个时

~~~ es6
let fn = (n) => {
	return n * n;
}
可以简写为
let fn = n => {
	return n * n;
}
省略了入参的括号
~~~

#### 2. 当函数体只有一句语句时

~~~ es6
let fn = n => {
	return n + n;
}
可以简写为：
let fn = n => n + n;
省略了大括号和return关键字，并且此时语句的执行结果就时函数的返回值
~~~

## 使用场景

**箭头函数适合与this无关的回调，如：定时器、数组方法的回调**
**不适合与this有关的回调，如：事件回调、对象的方法的回调**

# 参数初始值

## 定义：

ES6 中允许给函数参数赋初始值

## 1.形参的初始值

具有默认值的参数，一般位置要靠后(潜规则)，而且如果是有参数没有传，将带有初始值的参数放在前面也没有意义。

~~~ es6
let fn = function(a, b, c=10) {
	return a + b + c;
}
fn(1, 2); //当没有传入形参c的值时，c的值就为10
fn(1, 2, 3); // 当传入了形参c的值，就以实际传入的值为准
~~~

## 2. 与解构赋值相结合

~~~ es6
let fn = ({host = 'localhost', port, username}) => {
	console.log(host);
	console.log(port);
	console.log(username);
}
fn({
	host: '127.0.0.1',
	port: 8001,
	username: 'root'
});
原来的写法为
let fn = (option) => {
	console.log(option.host);
	console.log(option.port);
	console.log(option.root);
}
相当于简写了对象的形参名，并且可以赋初始值。
~~~

# Rest参数

定义：ES6 中引入rest参数，用于获取函数的实参，用来代替arguments。

~~~ es6
// es6引入rest参数，用来代替arguments
let fn = function() {
	console.log(arguments); // 打印出来的为arguments参数对象
}
fn(1, 2, 3);    // 打印出来的为arguments参数对象
let data = function(...args) {
	console.log(args); // 打印出来的为args数组
}
data(1, 2, 3);  // 打印出来的为args数组
~~~

**注意：rest参数使用时必须形参中必须有...args作为参数列表，才可以获得实参**
**且如果有多个参数...args参数必须放在最后，负责会报错**

# 扩展运算符

## 定义

使用时在变量前加	...	，可以将数组转化为带逗号的参数
~~~ es6
如
	let arr = [1, 2, 3];
	console.log(...arr); // 输出为 1, 2, 3 
	console.log(arr);	// 输出为[1, 2, 3]
~~~

## 应用

### 1. 数组的合并

~~~ es6
const arr1 = [1, 2];
const arr2 = [3, 4];
const arr3 = [...arr1, ...arr2];
console.log(arr3); // [1, 2, 3, 4]
~~~

### 2.数组的克隆

~~~ es6
const arr1 = [1, 2];
const arr2 = [...arr1];
console.log(arr2); // [1, 2]
~~~

### 3. 将伪数组转化为真正的数组

~~~ es6
// 将伪数组转化为真正的数组
let divs = document.querySelectorAll('div');
console.log(divs); // NodeList(3) [div, div, div]
let divArr = [...divs];
console.log(divArr); // [div, div, div]
~~~

# Symbol数据类型

## 定义：

Symbol是一种ES6引入的新的原始数据类型，表示独一无二的值。它是JavaScript的第七种数据类型，是一种类似于字符串的数据类型。

## 特点

1. Symbol值是唯一的，用来解决命名冲突的问题
2. Symbol值不能与其他数据进行运算
3. Symbol定义的对象属性不能使用for...in循环遍历，但是可以使用Reflect.ownKeys来获取对象所有键名

## 创建Symbol

~~~ es6
// 创建Symbol
let s = Symbol();
console.log(s , typeof s);

// 加入描述字符串
let s2 = Symbol('aaa'); // 可以表示这个Symbol值是用来做什么的等
let s3 = Symbol('aaa'); // 但是传入相同的描述字符串后得到的Symbol值不等
console.log(s2 , typeof s2);
console.log(s3 , typeof s3);
console.log(s2 === s3);

// 使用Symbol.for创建，此时Symbol是一个函数对象
let s4 = Symbol.for('aaa');
let s5 = Symbol.for('aaa'); // 此时是可以通过传入的描述值获取唯一一个Symbol函数对象值的
console.log(s4 , typeof s4);
console.log(s5 , typeof s5);
console.log(s4 === s5);
~~~
**注意：当遇到唯一性场景时，使用Symbol**

## JS中的基础数据类型

~~~es6
USONB	you are so niubility
U undefined
S string symbol
O object
N null number
B boolean
~~~

## Symbol的内置属性

除了定义自己使用的Symbol 值以外， ES6 还提供了 11个内置的 Symbol值，指向语言内部使用的方法。
可以称这些方法为魔术方法，因为它们会在特定的场景下自动执行。

### Symbol.hasInstance

当其他对象使用instanceof运算符，判断是否为该对象的实例时，会调用这个方法

~~~ es6
class Person {
static [Symbol.hasInstance](param) {
console.log(param);
console.log('我被用来检测类型了');
return false;
}
}

let obj = {};

console.log(obj instanceof Person); // 会根据Symbol.hasInstance方法的返回值打印true或false
控制台打印为：{}
			我被用来检测了
			false
~~~

### Symbol.isConcatSpreadable

对象的Symbol.isConcatSpreadable属性等于的是一个布尔值，表示该对象用于 Array.prototype.concat()时，是否可以展开。

### Symbol.species

创建衍生对象时，会使用该属性

### Symbol.match

当执行str.match(myObject) 时，如果该属性存在，会调用它，返回该方法的返回值。

### Symbol.replace

当该对象被str.replace(myObject)方法调用时，会返回该方 法的返回值。

### Symbol.search

当该对象被str. search (myObject)方法调用时，会返回该方法的返回值。

### Symbol.split

当该对象被str. split (myObject)方法调用时，会返回该方法的返回值。

### Symbol.iterator

对象进行for...of循环时，会调用 Symbol.iterator方法，返回该对象的默认遍历器

### Symbol.toPrimitive

该对象被转为原始类型的值时，会调用这个方法，返回该对象对应的原始类型值。

### Symbol. toStringTag

在该对象上面调用toString方法时 ，返回该方法的返回值

### Symbol. unscopables

该对象指定了使用with关键字时，哪些属性会被 with环境排除。

# 迭代器

## 定义

迭代器（Iterator）是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署Iterator接口，就可以完成遍历操作。

1. ES6创造了一种新的遍历命令for...of循环，Iterator接口主要供for...of消费
2. 原生具备Iterator的数据(可用for...of遍历)
   1. Array
   2. Aruguments
   3. Set
   4. Map
   5. String
   6. TypedArray
   7. NodeList

3. 工作原理
    1. 创建一个只针对性，指向当前数据结构的起始位置
    2. 第一次调用对象的next方法，指针自动指向数据结构的第一个成员
    3. 接下来不断调用next方法，指针一直向后移动，直到指向最后一个成员
    4. 每调用next方法返回一个包含value和done属性的对象

**注：需要自定义遍历数据的时候，要想到迭代器**

~~~es6
迭代器的实现案例
	// 将下面自定义的对象使用for...of输出，并且输出的结果为users数组中的值
	let team = {
        name: 'LPL',
        users: [
            'xiaoming',
            'xiaotian',
            'xiaowei',
            'xiaoning',
            'xiaoA'
        ],
        [Symbol.iterator]() {
            // 索引变量
            let index = 0;
            let _this = this;
            return {
                next: function() {
                    if (_this.users.length > index) {
                        let result = {
                            value: _this.users[index],
                            done: false,
                        }
                        index++;
                        return result;
                    } else {
                        return {value: undefined, done: true}
                    }
                }
            }
        }
    }

    for (const v of team) {
        console.log(v);
    }
~~~

# 生成器

生成器函数是ES6提供的一种异步编程方案，语法行为与传统函数完全不同。

## 生成器创建代码

~~~ es6
functoin * gen(arg) {
	console.log(arg);
	let one = yield('aaa');
	let two = yield('bbb');
	let three = yield('ccc');
}
let iterator = gen(111); // 生成器函数执行完成后会得到一个迭代器，而且默认只执行第一个yield前的代码，后续的代码需要使用iterator.next()方法才会继续执行，并且调用一次只会执行到下一个yeild语句之前
console.log(iterator.next('222')); // next方法可以传入参数，并且这个参数会作为前一个yeild的返回值
~~~

## 生成器使用实例

### 按照1秒输出111，2秒后输出222，3秒后输出333的方式输出数据

~~~ es6
   // 按照1秒输出111，2秒后输出222，3秒后输出333的方式输出数据
    // setTimeout(() => {
    //     console.log(111);
    //     setTimeout(() => {
    //         console.log(222);
    //         setTimeout(() => {
    //             console.log(333);
    //         }, 3000)
    //     }, 2000)
    // }, 1000);
    // 这样也可以实现，但是这样代码的问题是代码太复杂，并且缩进太多,c出现了回调地狱，而且不便于调试

    // 使用生成器实现
    function one() {
        setTimeout(() => {
            console.log(111);
            iterator.next();
        },1000);
    }

    function two() {
        setTimeout(() => {
            console.log(222);
            iterator.next();
        },2000);
    }

    function three() {
        setTimeout(() => {
            console.log(333);
            iterator.next();
        },3000);
    }

    function * gen() {
        yield one();
        yield two();
        yield three();
    }
    let iterator = gen();
    iterator.next();
~~~

### 按照先后顺序获取用户数据，订单数据，商品数据

~~~ es6
    // 按照先后顺序获取用户数据、订单数据、商品数据。
    function getUsers() {
        setTimeout(() => {
            let data = '用户数据';
            iterator.next(data);
        }, 1000); 
    }
    function getOrders() {
        setTimeout(() => {
            let data = '订单数据';
            iterator.next(data);
        }, 1000); 
    }
    function getGoods() {
        setTimeout(() => {
            let data = '商品数据';
            iterator.next(data);
        }, 1000); 
    }
    function * gen() {
        let users = yield getUsers();
        console.log(users);
        let orders = yield getOrders();
        console.log(orders);
        let goods = yield getGoods();
        console.log(goods);
    }
    let iterator = gen(); // 必须要使用iteartor变量接受gen()执行后的迭代器，因为前面的代码中使用了这个变量调用了next()方法传参。
    iterator.next();
~~~

# Promise函数

## 定义

Promise是ES6引入的异步编程的新的解决方案。语法上Promise是一个构造函数，用来封装异步操作并可以获取其成功或失败的结果。
1. Promise构造函数：Promise(excutor) {}
2. Promise.prototype.then() 方法
3. Promise.prototype.catch() 方法

``` es6
    // Promise创建的方式是一个构造函数的方式，其中的入参为一个函数对象，函数中有两个入参，使用入参调用方法可以调用then中的方法
    const p = new Promise(
        function(resolve, reject) {
            let flag = false;
            let data = '获取用户数据';
            if(flag) {
                resolve(data);
            } else {
                reject(data);
            }
        }
    );
    
    // 调用Promise中的then方法，其中传入两个参数，两个参数都为函数，第一个为Promise定义函数中的第一个传入的形参调用的方法，第二个就为第二个调用的方法。
    p.then(function(value) {
        console.log(value);
    }, function(reason) {
        console.error(reason);
    });
```

## Promise.phototype.then方法

~~~ es6
Promise.phototype.then方法的返回值也是一个Promise对象，其中只有以下几种情况返回的Promise状态为reject
1.在then方法中使用了reject()方法
如p.then((value) => {
	reject('aaa');
}, (reason) => {
	reject('aaa');
})
其中不管是在resolve方法还是reject方法中调用reject方法，then方法返回的Promise对象的状态都是rejected
2.在then方法中使用throw抛出数据
如p.then((value) => {
	throw 'aaa';
}, (reason) => {
	throw 'aaa';
})
只要抛出了，状态也都是rejected
~~~

## Promise.phototype.catch方法

相当于then方法中省略了成功的回调方法，可以只写出错了的回调方法

~~~ es6
const p = new Promise((resolve, reject) => {
	reject('出错了');
});
p.then((value) => {}, (reason) => {
	console.warn('reason');
});
// 这个then方法也可以写为catch方法
p.catch((reason) => {
	console.warn('reason');
});
~~~

## 使用Promise案例

### 读取文件案例

~~~ es6
// 1.引入fs模块
const fs = require('fs');

// 2.调用方法读取文件
// fs.readFile("./resource/为学.md", (err, data) => {
//     if(err) {
//         console.log('读取失败');
//     }
//     console.log(data.toString());
// })

// 3.使用Promise封装
const p = new Promise((resolve, reject) => {
    fs.readFile('./resource/为学.md',(err, data) => {
        if (err) reject('读取失败');
        resolve(data);
    })
});
p.then((data)=>{
    console.log(data.toString());
}, (err) => {
    console.error(err);
})
~~~

### 使用Promise封装AJAX请求

~~~ es6
    const p = new Promise((resolve, reject) => {
        // 1. 创建XMLHttpRequest()对象
        let xhr = new XMLHttpRequest();
        // 2. 初始化
        xhr.open('get','http://www.baidu.com');
        // 3.发送请求
        xhr.send();
        // 4.绑定事件，处理响应结果
        if (xhr.readyState == 4) {
            if (xhr.status >=200 && xhr < 300) {
                resolve(xhr.response);
            } else {
                reject(xhr.status);
            }
        }
    });
    p.then((value) => {
        console.log(value);
    }, (reason) => {
        console.log(reason);
    })
~~~

### 使用Promise逐步请求文件内容

~~~ es6
const fs = require('fs');

// 使用回调函数操作
/*fs.readFile('./resource/为学.md', (err, data1) => {
    fs.readFile('./resource/插秧诗.md', (err, data2) => {
        fs.readFile('./resource/观书有感.md', (err, data3) => {
            console.log(data1.toString() + '\r\n' + data2.toString() + '\r\n' + data3.toString());
        });
    });
});*/

// 使用Promise函数对象操作
const p = new Promise((resolve) => {
    fs.readFile('./resource/为学.md', (err, data) => {
        resolve(data);
    });
});

p.then(value => {
    return new Promise((resolve) => {
        fs.readFile('./resource/插秧诗.md', (err, data) => {
            resolve([value, data]);
        });
    })
}).then(value => {
    return new Promise(resolve => {
        fs.readFile('./resource/观书有感.md', (err, data) => {
            let result = value.push(data);
            resolve(value);
        });
    })
}).then(value => {
    console.log(value.join('\r\n'));
})
~~~

# 集合

## Set

### 定义

ES6提供的新的数据类型，它类似于数组，但成员的值都是唯一的，集合实现了iterator接口，所以可以使用扩展运算符和【for...of】进行遍历

### 属性和方法

1. size		属性，返回集合的元素个数
2. add        方法，增加一个新的元素，返回当前集合
3. delete    方法，删除元素，返回boolean值
4. has         方法，检测集合中是否包含某个元素，返回boolean值
5. clear       方法，清空集合，返回undefined

### 创建集合

~~~ es6
// 创建一个空集合
let s = new Set();
// 创建一个非空集合
let s1 = new Set([1,2,3,4]); // 其中为数组

// 集合属性值
// 返回集合属性值
console.log(s1.size);
// 添加新元素
console.log(s1.add(4));
// 删除元素
console.log(s1.delete(1));
// 检测是否存在某个值
console.log(s1.has(2));
// 清空集合
console.log(s1.clear());
~~~

### Set集合使用案例

~~~ es6
let arr = [1, 2, 3, 4, 5, 4, 3, 2, 1];
let arr1 = [4, 5, 6, 5, 4];
// 1.数组去重
let arr2 = [...new Set(arr)];
console.log(result);
// 2.交集
let arr3 = [...new Set(arr)].filter(item => new Set(arr1).has(item));
// 3.并集
let arr3 = [...new Set([...arr, ...arr1])];
// 4.差集
let arr4 = [...new Set(arr)].filter(item => !new Set(arr1).has(item));
~~~

## Map

### 定义

ES6提供了Map数据接口。它类似于对象，也是键值对的集合。但是“键”的范围不局限于字符串，各种类型的值（包括对象）都可以当作键。Map也实现了iterator接口。所以可以使用【扩展运算符】和【for...of...】进行遍历。

### 属性和方法

1. size			属性。返回Map的元素个数
2. set             方法。增加一个新元素，返回当前Map
3. get             方法。返回键名对应的键值
4. has            方法。检测Map中是否包含某个元素，返回boolean值。
5. clear          方法。清空集合，返回undefined

~~~ es6
// 声明
//创建一个空 map 
let m = new Map(); 
//创建一个非空 map 
let m2 = new Map([ 
	['name','尚硅谷'], 
	['slogon','不断提高行业标准'] 
]);

//属性和方法 
//获取映射元素的个数 
console.log(m2.size); 
//添加映射值 
console.log(m2.set('age', 6)); 
//获取映射值 
console.log(m2.get('age')); 
//检测是否有该映射 
console.log(m2.has('age')); 
//清除 
console.log(m2.clear());
~~~

# Class类

## 定义

ES6提供了更接近传统语言的写法，引入了Class(类)的概念，作为对象的模板。通过class关键字，可以定义类。基本上，ES6的class可以看作只是一个语法糖，它的绝大部分功能，ES5都可以做到，新的class写法只是让对象原型的写法更加清晰、更加像面向对象的写法而已。

## 知识点

1. class声明类
2. constructor定义构造函数初始化
3. extends继承类
4. super调用父级构造方法
5. static定义静态方法和属性
6. 父类方法可以重写

## 使用class创建对象

~~~ es6
    // es5创建对象并增加构造函数与成员方法
    function Phone(brand, price) {
        this.brand = brand;
        this.price = price;
    }
    // 添加成员方法
    Phone.prototype.call = function() {
        console.log('我能打电话');
    }
    // 实例话对象
    let Dianxin = new Phone('电信', 1000);
    Dianxin.call();

    // es6使用class
    class MobilePhone {
        // 构造方法，名字不能修改
        constructor(brand, price) {
            this.brand = brand;
            this.price = price;
        }

        // 必须使用改语法，不能使用ES5中的对象的形式
        call() {
            console.log('我也可以打电话');
        }
        // 也可以这样写
        /*call = function call() {
            console.log('我也可以打电话');
        }*/
    }
    let Huawei = new MobilePhone('华为', 2000);
    Huawei.call();
~~~

## 静态成员

~~~ es6
    // es5中没有class，所有就有函数对象和实例对象
    // 则函数对象属性为静态属性，实例对象就是成员 -- 自己的理解
    function Phone() {

    }
    Phone.name = '电话';
    Phone.call = function call() {
        console.log('我可以打电话');
    }
    Phone.prototype.size = 'big';

    let nokia = new Phone();
    Phone.call();
    console.log(nokia.name);
    console.log(nokia.size);

    // es6中的静态成员
    class MobeilPhone{
        static name = '手机';
        static call() {
            console.log('我也可以打电话');
        }
        /*static call = function call() {
            console.log('我也可以打电话');
        }*/
    }
    console.log(MobeilPhine.name);
    MobeilPhine.call();
~~~

## 继承

### ES5中构造函数实现继承

~~~ es5
    function Phone(brand, price) {
        this.brand = brand;
        this.price = price;
    }
    
    Phone.prototype.cell = function() {
        console.log('我可以打电话');
    }

    function SmartPhone(brand, price, color, size) {
        Phone.call(this, brand, price);
        this.color = color;
        this.size = size;
    }

    SmartPhone.prototype = new Phone;
    SmartPhone.prototype.constructor = SmartPhone;

    SmartPhone.prototype.photo = function() {
        console.log('手机，我可以拍照');
    }
    SmartPhone.prototype.game = function() {
        console.log('我可以打游戏');
    }

    const Huawei = new SmartPhone('华为', 3999, 'red', 15.51);
    Huawei.cell();
    Huawei.photo();
    Huawei.game();
    console.log(Huawei);

    console.log(Huawei.brand);
~~~

### ES6使用extends关键字继承

~~~ es6
   class Phone{
        constructor(brand, price) {
            this.brand = brand;
            this.price = price;
        }
        call() {
            console.log('我能打电话');
        }
    }

    class SmartPhone extends Phone{
        constructor(brand, price, color, size) {
            super(brand, price); // 调用父类的构造方法
            this.color = color;
            this.size = size;
        }

        photo() {
            console.log('我可以拍照');
        }
        
        // 子类中，可以重写父类的重名方法，但是不可以在子类的成员方法中使用super关键字调用父类的重名方法
        call() {
            console.log('我i可以视频通话');
        }
    }
    const xiaomi = new SmartPhone('小米', 1999, 'black', 15.6);
    console.log(xiaomi);
    xiaomi.call();
    xiaomi.photo();
~~~

## get和set方法

~~~es6
   // es6中只要有了get和set方法，所有与此属性有交集操作都会经过这两个方法
    class Phone{
        get price() {
            console.log('获取price数据');
            return 12;
        }
        set price(price) {
            console.log('设置price数据');
        }
    }

    let ChuiZi = new Phone();
    ChuiZi.price; // 获取price数据
    ChuiZi.price = 'free'; // 设置price数据
~~~
**es6中只要有了get和set方法，所有与此属性有交集操作都会经过这两个方法**

# ES5的类型扩展

## 数值类型扩展

### Number.EPSILON

~~~ es6
	// 0. Number.EPSILON是JavaScript中的最小精度
    // Number.EPSILON属性的值接近于 2.220446049250313e-16
    console.log(Number.EPSILON); 
    function equal(a, b) {
        if(Math.abs(a - b) < Number.EPSILON) {
            return true;
        }
        return false;
    }
    console.log(0.1 + 0.2 === 0.3);
    console.log(equal(0.1 + 0.2, 0.3));
~~~

### 二进制和八进制

~~~es6
    // 1. 二进制和八进制
    // ES6 提供了二进制和八进制数值的新的写法，分别用前缀 0b和 0o表示。
    let a = 0b0101; // 5
    let b = 0o5432; // 2842
    let c = 0xff; // 255
    console.log(a, b, c);
~~~

### Number.isFinite()与Number.isNaN()

这两个方法在ES5中就有，只不过ES6中将这两个方法放入了Number对象中，使用与查阅起来更方便。

~~~es6
    // 2. Number.isFinite检测一个数是否为有限数
    console.log(Number.isFinite(100));
    console.log(Number.isFinite(100/0));
    console.log(Number.isFinite(isFinite));

    // 3. Number.isNaN()用来检测一个值是否为NaN
    console.log(Number.isNaN(123));
~~~

### Number.parseInt()与Number.parseFloat()

~~~ es6
	// 4. ES6 将全局方法 parseInt和 parseFloat，移植到 Number对象上面，使用不变。
    /*console.log(Number.parseInt('123465love'));
    console.log(Number.parseFloat('3.16546asdgasdg'));*/
~~~

### Number.isInteger()

~~~ es6
    // 5.Number.isInteger()用来判断一个数值是否为整数
    console.log(Number.isInteger(56.456));
~~~

### Math.trunc()

~~~ es6
    // 6.Math.trunc()用于去除一个数的小数部分，返回整数部分
    console.log(Math.trunc(3.46461646));
~~~

### Math.sign()

~~~es6
    // 7.Math.sign()判断一个数到底为正数，负数还是零
    // 为正数是为1，负数时为-1，零时为0
    console.log(Math.sign(8)); // 1
    console.log(Math.sign(0)); // 0
    console.log(Math.sign(-4)); // -1
~~~

## 对象类型扩展

### Object.is

判断两个值是否完全相等

~~~es6
    // 1.Object.is()判断传入的两个参数值是否完全相等
    console.log(Object.is(123, 123)); // true
    console.log(Object.is(NaN, NaN)); // true
    console.log(NaN === NaN); // false
~~~

### Object.assign()

对象的合并

~~~es6
    // 2.Object.assign(obj1, obj2)对象的合并
    // 以obj2为主体，将obj1对象并入obj2
    // 当对象的属性值冲突时，以obj2为主体，不冲突就全部加入
    let obj1 = {
        host: 'localhost',
        log: 'obj1'
    }
    let obj2 = {
        port: 3306,
        log: 'obj2'
    }
    console.log(Object.assign(obj1, obj2));
    /*
        host: "localhost"
        log: "obj2"
        port: 3306
    */
~~~

### Object..setPrototypeOf(obj1, obj2)

~~~ es6
   // 3.Object.setPrototypeOf(obj1, obj2)
   // 将obj1的原型对象设置为obj2
   let school = {
       name: 'ggg'
   }
   let cities = {
       xiaoqu: ['上海', '北京', '广州']
   }
   Object.setPrototypeOf(school, cities);
   console.log(school);
   console.log(Object.getPrototypeOf(school));
~~~

### Object.defineProperty()

向某个对象中添加某个属性。入参为对象，属性名，该属性的描述属性，

即为Object.defineProperty(对象名, '属性名', 描述对象)

~~~es6
    let number = 10;
    let person = {
        name: '张三',
        sex: '男'
    }

    Object.defineProperty(person, 'age', {
        // value: number,
        // writable: true, // 控制属性是否可以被修改，默认是false
        // enumerable: true, // 控制属性是否可以枚举，默认是false
        // configurable: true, // 控制属性是否可以被删除，默认是false

        // 且使用了setter和getter方法，就不允许出现属性的描述属性
        // 当有人读取person的age时，get函数(getter)就会被调用，且返回的值就是age的值
        get() {
            console.log('有人获取age的值');
            return number
        },

        // 当有人修改person的age时，set函数(setter)函数就会被调用，且会修改age的具体值
        set(value) {
            console.log('有人要给age赋值');
            number = value
        }
    });
    console.log(person);
~~~

## 数组类型扩展

### `Array.reduce([callback], i);`

`Array.reduce([callback], i);`该方法会遍历数组，然后运行`Array.length`次，其中回调函数可以写为`(pre, current) => {return xxx;}`必须有返回值，如果不写，默认返回`undefined`，第一个参数`pre`为每次回调函数返回的值，初始值为入参`i`，`current`参数为每次遍历的数组中的元素，最终的返回值为回调函数最后运行的返回值。

~~~js
let todoList = [
    {id: '001', title: '喝酒', done: true},
    {id: '002', title: '抽烟', done: false},
    {id: '003', title: '打游戏', done: true},
];
return this.todoList.reduce((pre, current)=> pre + (current.done ? 1 : 0), 0); // 此结果就为 2

let arr = [1, 2, 3]
let result = arr.reduce((pre, current) => {
    console.log(pre, current); // pre和current的值分别为 pre:3,current:1;pre:4,current:2;pre=5,current=3
    return pre + 1;
},3);
console.log(result); // 6
~~~

# ES6的模块化

## 定义

模块化是指将一个大的程序文件，拆分成许多小的文件，然后将小文件组合起来

## 优势

1. 防止命名冲突
2. 代码复用
3. 高维护性

## 语法

模块化主要由两个命名构成：export和import。

### export

export命令用于规定模块的对外接口

~~~es6
// 第一种方式：分别暴露
// 直接在变量或者方法前加export关键字
export let school = '改变学校';

export function change() {
    console.log('我能改变世界');
}

// 第二种方式：统一暴露
// 使用export关键字统一将变量名暴露出去
let school = '改变学校';

let findJob = function() {
    console.log('我能找到工作');
}

export {school, findJob};

// 第三中方式：默认暴露
// 默认暴露中可以为对象，统一暴露中不可以，但是使用import导入过后必须使用加上.default才可以继续.属性
export default{
    school: '改变学校',
    change: function() {
        console.log('我能改变世界');
    }
}
~~~

### import

import命令输入其他模块提供的功能

~~~ es6
// 1.通用导入方式
    // 引入m1.js模块内容
    import * as m1 from './js/m1.js';
    // 引入m2.js模块内容
    import * as m2 from './js/m2.js';
    // 引入m3.js模块内容
    import * as m3 from './js/m3.js';
    console.log(m1.school);
    console.log(m2.school);
    console.log(m3.default.school);
// 2.解构赋值形式
    import {school, teach} from './js/m1.js';
    // 如果已经引入了一个school的变量，再引入一个相同的变量名时，必须使用as设置别名
    import {school as xuexiao, findJob} from './js/m2.js'; 
    // 默认暴露使用解构赋值的形式导入的时候必须使用这种写法，固定写法
    import {default as m3} from './js/m3.js';
    console.log(m3.school);
    console.log(teach);
// 3.简便形式，只针对默认暴露
    import m3 from './js/m3.js';
    console.log(m3.school);
~~~

## 使用babel打包，将ES6代码打包为ES5代码

为了解决部分浏览器的兼容性问题

~~~ npm
1.在项目目录中使用npm i babel-cli babel-preset-env browserify -D 命令将babel-cli(打包工具)、babel-perset-env(打包依赖)、browserify(包管理工具，一般使用webpack)下载到项目的运行时依赖中去
2.使用npx babel src/js -d dist/js --presets=babel-perset-env 命令将src/js中所有文件编译到dist/js目录中去
3.使用npx browserify dist/js/app.js -o dist/bundle.js命令将app.js打包编译为ES5中可以运行的文件、
4.在html文件中引入bundle.js文件使用
~~~
