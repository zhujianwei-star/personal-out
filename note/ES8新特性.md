
# async表达式与await表达式

async和 await两种语法结合可以让异步代码像同步代码一样

## async表达式

1. async函数的返回值为 promise对象，
2. promise对象的结果由 async函数执行的返回值决定

~~~ es8
	// async函数
    async function fun() {
        // 和Promised中执行了then函数的返回类型一样，返回值也是一个Promise对象，其中只有以下几种情况返回的Promise状态为reject
        // 其他的返回的状态都为成功fulfilled
        // return '朱健伟';
        // 其中不管是在resolve方法还是reject方法中调用reject方法，then方法返回的Promise对象的状态都是rejected
        // 2.在then方法中使用throw抛出数据
        // 只要抛出了，状态也都是rejected
        /*return new Promise((resolve, reject) => {
            resolve('aaa');
            // reject('bbb');
        });*/
        return aaa;
    }
    fun().then(value => console.log(value), reason => console.warn(reason));
~~~

## await表达式

1. await必须写在async函数中

2. await右侧表达式一般为promise对象

3. await返回的是promise成功的值

4. await的promise失败了，就会抛出异常，需要通过try...catch捕获处理

~~~es8
    // 1.await必须写在async函数中
    // 2.await右侧表达式一般为promise对象
    // 3.await返回的是promise成功的值
    // 4.await的promise失败了，就会抛出异常，需要通过try...catch捕获处理
    const p = new Promise((resolve, reject) => {
        // resolve('用户数据');
        reject('失败了啊');
    });

    async function fun() {
        try {
            const result = await p;
            console.log(result);
        } catch(e) {
            console.log(e);
        }
    }

    fun();
~~~

## 使用async与await结合一次读取文件

~~~ es8
const fs = require('fs');
function readWeixue() {
    return new Promise((resolve, reject) => {
        fs.readFile('./resource/为学.md', (err, data) => {
            if (err) reject(err);
            resolve(data);
        })
    })
}

function readChayangshi() {
    return new Promise((resolve, reject) => {
        fs.readFile('./resource/插秧诗.md', (err, data) => {
            if (err) reject(err);
            resolve(data);
        })
    })
}

function readGuanshuyougan() {
    return new Promise((resolve, reject) => {
        fs.readFile('./resource/观书有感.md', (err, data) => {
            if (err) reject(err);
            resolve(data);
        })
    })
}

async function main() {
    try{
        const weixue = await readWeixue();
        const Chayangshi = await readChayangshi();
        const Guanshuyougan = await readGuanshuyougan();
        console.log(weixue.toString());
        console.log(Chayangshi.toString());
        console.log(Guanshuyougan.toString());
    } catch(e) {
        console.warn(e);
    }
}

main();
~~~

## 使用async与await结合发送ajax请求

~~~es8
    function sendAjaxRequest(url) {
        return new Promise((resolve,reject) => {
            // 1.获取发送请求对象
            const xhq = new XMLHttpRequest();

            // 2.初始化
            xhq.open('get',url);

            // 3.发送请求
            xhq.send();

            // 4.事件绑定
            xhq.onreadystatechange = function () {
                if (xhq.readyState === 4) {
                    if (xhq.status >= 200 && xhq.status < 300) {
                        resolve(xhq.response);
                    }else {
                        reject(xhq.status);
                    }
                }
            }
        });
    }

    async function main() {
        try {
            // let result = await sendAjaxRequest('https://api.apiopen.top/getJoke');
            // console.log(result);
            // 发送的这两个url路径可以测试发送数据
            const tianqi = await sendAjaxRequest('https://tianqiapi.com/api/?version=v1&city=%E9%95%BF%E6%B2%99&appid=23941491&appsecret=TXoD5e8P'); 
            console.log(tianqi);
        } catch (error) {
            console.warn(error);
        }
        
    }

    main();
~~~

# 对象方法的扩展

1. object.values()方法返回一个给定对象的所有可枚举属性值的数组
2. Object.entries()方法返回一个给定对象自身可遍历属性 [key,value] 的数组
3. Object.getOwnPropertyDescriptors()方法返回指定对象所有自身属性的描述对象

~~~es8
    // 声明对象
    let school = {
        name: 'zjw',
        cities: ['长沙'],
        xueke: ['前端','java']
    };
    // 获取所有键
    console.log(Object.keys(school));
    // 获取所有值
    console.log(Object.values(school));
    // entries
    console.log(Object.entries(school));
    // 创建map
    const map = new Map(Object.entries(school));
    console.log(map.get('cities'));
    // 对象描述属性对象
    console.log(Object.getOwnPropertyDescriptors(school));

    // 创建描述对象
    const obj = Object.create(/*原型对象*/null,/*描述对象*/{
        name: {
            // 设置值，而且值必须为一个对象
            // 设置value
            value: 'zjw',
            // 设置属性特性
            writable: true, // 是否可写
            configurable: true, // 是否可以删除
            enumerable: true // 是否可以枚举
        }
    });
    // 使用getOwnPropertyDescriptors()方法就是获取描述对象，获取了可以进行一些深度对象的克隆等功能
~~~

