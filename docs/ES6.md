# ES6知识点总结

## 一、let和const命令

### 1.1 let命令

let是变量声明的一种类型，使用方式类似于`var`，你补`var`在变量作用域方面的不足。使用let声明的变量，只有在let命令所在的代码块中有效。

```javascript
//j虽然在for循环内部声明，但在循环外部仍旧可以获取j变量的值
//使用let变量时，j只作用于循环内部，防止全局变量污染
for(var i = 0;i<10;i++){
	var j = 10;
}
```

### 1.2 const命令

`const`命令用来生命常量，不能进行修改。

```javascript
const a = 100;
```

## 二、字符串扩展

字符串模板：

​	ES6采用`进行字符串的模板声明。

​		不管是换行还是其他任何脚本，只要在两个`之间，都被看成一个字符串。

​		同时也可在`声明的字符串内部，使用${}进行变量的拼接。

ES6中对字符串新增了几个拓展API。

​	`includes()`:是否包含字符串内容，返回布尔值

​	`startWith()`:是否以某字符串开头

​	`endWith()`:是否以某字符串结尾

例子：

```javascript
let str = "hello world";

//是否包含hello字符串
str.includes("hello");	//true
//是否以h开头
str.startWith("h");		//true
//是否以d结尾
str.endWith("d")

```

## 三、解构表达式

解构表达式是为了方便在对象或者数组中快速取值而产生的。他的具体用法如下：

```javascript
const [x,y,z] = [1,2,3];//x y z会把数组中对应位置的值取出来赋值在对应变量上

const {x,y,z} = {x:1,y:2,z:3}//在对象中，会自动取得对应key的值。
```

如果想要赋值给其他变量：

```javascript
let i;

const {x:i,y,z} = {x:1,y:2,z:3}//会把x的值赋值给i变量
```

## 四、对函数的优化

### 4.1 默认值

可以直接在参数上设置默认值，当此值不传或者为空时就赋给默认值

```javascript
function add(a , b = 1) {
    return a + b;
}
// 传一个参数
console.log(add(10));
```

### 4.2 箭头函数

上述函数可以用箭头函数来简写

单参数单条代码：

```javascript
let f = a=>console.log(a);
```

多参数单条代码 函数体只有一条代码时可以简写，默认返回此语句的值。

```javascript
let f = (a,b)=>a+b;
```

多参数多条代码

```javascript
let f = (a,b)=>{
		let j = a+b;
		return j;
}
```

### 4.3 对象属性的简写

```javascript
let person = {
    name: "jack",
    // 以前：
    eat: function (food) {
        console.log(this.name + "在吃" + food);
    },
    // 箭头函数版：
    eat2: food => console.log(person.name + "在吃" + food),// 这里拿不到this
    // 简写版：
    eat3(food){
        console.log(this.name + "在吃" + food);
    }
}
```

### 4.4 箭头函数结合解构表达式

```javascript
const person = {
    name:"jack",
    age:21,
    language: ['java','js','css']
}

function hello(person) {
    console.log("hello," + person.name)
}

var hi = ({name}) =>  console.log("hello," + name);
```

## 五、数组扩展

### 5.1 map方法

`map()`：接收一个函数，将原数组中的所有元素用这个函数处理后放入新数组返回。

```javascript
//循环arr数组转换为整数，再赋值给arr，   实现了元素数组的整数转换
let arr = ['1','20','-5','3'];
arr = arr.map(s => parseInt(s));

```

### 5.2 reduce

`reduce()`：接收一个函数（必须）和一个初始值（可选），该函数接收两个参数：

- 第一个参数是上一次reduce处理的结果
- 第二个参数是数组中要处理的下一个元素

`reduce()`会从左到右依次把数组中的元素用reduce处理，并把处理的结果作为下次reduce的第一个参数。如果是第一次，会把前两个元素作为计算参数，或者把用户指定的初始值作为起始参数

理解起来比较难，下面试reduce的几个实例：

先定一个原始数组：

```javascript
var arr = [3,9,4,3,6,0,9];
```

#### 求数组的和

```javascript
var sum = arr.reduce(function (prev, cur) {
    return prev + cur;
},0);
```

由于传入了初始值0，所以开始时prev的值为0，cur的值为数组第一项3，相加之后返回值为3作为下一轮回调的prev值，然后再继续与下一个数组项相加，以此类推，直至完成所有数组项的和并返回。

#### 求最大值

```javascript
var max = arr.reduce(function (prev, cur) {
    return Math.max(prev,cur);
});
```

由于未传入初始值，所以开始时prev的值为数组第一项3，cur的值为数组第二项9，取两值最大值后继续进入下一轮回调。

#### 数组去重

```javascript
var newArr = arr.reduce(function (prev, cur) {
    prev.indexOf(cur) === -1 && prev.push(cur);
    return prev;
},[]);
```

① 初始化一个空数组
② 将需要去重处理的数组中的第1项在**初始化数组**中查找，如果找不到（空数组中肯定找不到），就将该项添加到**初始化数组**中
③ 将需要去重处理的数组中的第2项在**初始化数组**中查找，如果找不到，就将该项继续添加到**初始化数组**中
④ ……
⑤ 将需要去重处理的数组中的第n项在**初始化数组**中查找，如果找不到，就将该项继续添加到**初始化数组**中
⑥ 将这个**初始化数组**返回

## 六、Promise

所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

我们可以通过Promise的构造函数来创建Promise对象，并在内部封装一个异步执行的结果。

语法：

```js
const promise = new Promise(function(resolve, reject) {
  // ... 执行异步操作

  if (/* 异步操作成功 */){
    resolve(value);// 调用resolve，代表Promise将返回成功的结果
  } else {
    reject(error);// 调用reject，代表Promise会返回失败结果
  }
});
```

这样，在promise中就封装了一段异步执行的结果。



如果我们想要等待异步执行完成，做一些事情，我们可以通过promise的then方法来实现,语法：

```js
promise.then(function(value){
    // 异步执行成功后的回调
});
```

如果想要处理promise异步执行失败的事件，还可以跟上catch：

```javascript
promise.then(function(value){
    // 异步执行成功后的回调
}).catch(function(error){
    // 异步执行失败后的回调
})
```

示例：

```javascript
const p = new Promise(function (resolve, reject) {
    // 这里我们用定时任务模拟异步
    setTimeout(() => {
        const num = Math.random();
        // 随机返回成功或失败
        if (num < 0.5) {
            resolve("成功！num:" + num)
        } else {
            reject("出错了！num:" + num)
        }
    }, 300)
})

// 调用promise
p.then(function (msg) {
    console.log(msg);
}).catch(function (msg) {
    console.log(msg);
})
```

