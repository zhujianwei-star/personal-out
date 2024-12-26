
# 针对对象的Rest参数和spread扩展运算符

Rest参数与 spread扩展运算符在 ES6中已经引入，不过 ES6中只针对于数组，在 ES9中为对象提供了像数组一样的 rest参数和扩展运算符

~~~es9
    // Rest参数与 spread扩展运算符在 ES6中已经引入，不过 ES6中只针对于数组，
    // 在 ES9中为对象提供了像数组一样的 rest参数和扩展运算符
    // Rest参数
    function connection({host, port, ...user}) {
        console.log(host); // localhost
        console.log(port); // 3306
        console.log(user);  // {user: 'root', password: 'root'}
    }

    connection({
        host: 'localhost',
        port: '3306',
        user: 'root',
        password: 'root'
    });

    // spread扩展运算符
    const skillOne = {
        q: '天音波'
    }
    const skillTwo = {
        w: '金钟罩'
    }
    const skillThree = {
        e: '天雷破'
    }
    const skillFour = {
        r: '猛龙摆尾'
    }
    let mangsen = {...skillOne, ...skillTwo, ...skillThree, ...skillFour};
    console.log(mangsen); // {q: '天音波', w: '金钟罩', e: '天雷破', r: '猛龙摆尾'}
~~~

# 正则表达式扩展

## 命名捕获分组

ES9允许命名捕获组使用符号 『 `?<name>`』 ,这样获取捕获结果可读性更强

~~~ es9
   // 声明一个字符串
    let str = '<a href="http://www.baodu.com">百度</a>'
    // const reg = /<a href="(.*)">(.*)<\/a>/;
    // const result = reg.exec(str);
    // console.log(result);
    // /*Array(3)
    //     0: "<a href=\"http://www.baodu.com\">百度</a>"
    //     1: "http://www.baodu.com"
    //     2: "百度"
    //     groups: undefined
    //     index: 0
    //     input: "<a href=\"http://www.baodu.com\">百度</a>"
    //     length: 3
    //     [[Prototype]]: Array(0)
    // */
    // // 这个是使用ES9正则扩展之前的写法，也是可以获取到数据的，但是没法groups属性为undefined
    // console.log(result[1]);
    // console.log(result[2]);
    const reg = /<a href="(?<url>.*)">(?<text>.*)<\/a>/
    const result = reg.exec(str);
    console.log(result);
    /*
        Array(3)
        0: "<a href=\"http://www.baodu.com\">百度</a>"
        1: "http://www.baodu.com"
        2: "百度"
        groups: {url: 'http://www.baodu.com', text: '百度'}
        index: 0
        input: "<a href=\"http://www.baodu.com\">百度</a>"
        length: 3
        [[Prototype]]: Array(0)
    */
    // 其中groups中就有了url和text为属性名的对象
    console.log(result.groups.url);
    console.log(result.groups.text);
~~~

## 反向断言

ES9支持反向断言，通过对匹配结果前面的内容进行判断，对匹配进行筛选。

~~~ es9
    // 声明字符串
    let str = 'JS5201314你知道吗555啦啦啦';
    // 正向断言 (?=啦) 获取啦前面的内容
    let reg = /\d+(?=啦)/;
    let result = reg.exec(str);
    console.log(result);

    // 反向断言 (?<=吗)获取吗后面的内容
    reg = /(?<=吗)\d+/
    result = reg.exec(str);
    console.log(result);
~~~

## dotAll模式

正则表达式中点『.』匹配除回车外的任何单字符，标记 『 s』 改变这种行为，允许行终止符出现

~~~es9
    // dot『.』元字符，除换行符以外的单个字符
    let str = ` 
    <ul> 
        <li> 
            <a>肖生克的救赎</a> 
            <p>上映日期: 1994-09-10</p> 
            
        </li> 
        <li> 
            <a>阿甘正传</a> 
            <p>上映日期: 1994-07-06</p> 
        </li> 
    </ul>`;

    // 声明正则
    let reg = /<li>\s+<a>(?<title>.*?)<\/a>\s+<p>(?<time>.*?)<\/p>\s+<\/li>/ // 本来是需要写/s+来匹配换行符的
    // let result = reg.exec(str);
    // console.log(result);
    // 在最后增加一个s就可以让『.』表示换行符，加上g表示全局
    reg = /<li>.*?<a>(?<title>.*?)<\/a>\s+<p>(?<time>.*?)<\/p>.*?<\/li>/gs
    let result;
    let data = [];
    while((result = reg.exec(str))) {
        console.log(result);
        data.push({...result.groups})
    }
    console.log(data);
~~~

