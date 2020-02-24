# Vue基础

## 一、生命周期函数

每个Vue实例被创建时，都需要经历一个过程，这称之为生命周期。Vue的生命周期见下图：

![clipboard.png](Vue%E5%9F%BA%E7%A1%80/bVEo3w.png)

`beforecreate` : 举个栗子：可以在这加个loading事件
`created` ：在这结束loading，还做一些初始化，实现函数自执行
`mounted` ： 在这发起后端请求，拿回数据

## 二、指令

### 2.1 花括号

格式：

```vue
{{表达式}}
```

此表达式支持JS语法，但需要有返回值。

### 2.2 v-text和v-html

- v-text：将数据输出到元素内部，如果输出的数据有HTML代码，会作为普通文本输出
- v-html：将数据输出到元素内部，如果输出的数据有HTML代码，会被渲染

实例：

```vue
    v-text:<span v-text="hello"></span>
    v-html:<span v-html="hello"></span>
```

### 2.3 v-model

上述v-text和v-html只是实现了从数据到展示的单向绑定。而v-model则是双向绑定。可以实现从展示层到数据的方向绑定。

但v-model有元素的限制，目前v-model可以使用的元素有：

- input
- select
- textarea
- checkbox
- radio
- components（Vue中的自定义组件）

基本都是表单项。

实例：

```vue
<div id="app">
    <input type="checkbox" v-model="language" value="Java" />Java<br/>
    <input type="checkbox" v-model="language" value="PHP" />PHP<br/>
    <input type="checkbox" v-model="language" value="Swift" />Swift<br/>
    <h1>
        你选择了：{{language.join(',')}}
    </h1>
</div>
<script src="./node_modules/vue/dist/vue.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el:"#app",
        data:{
            language: []
        }
    })
</script>
```

- 多个`CheckBox`对应一个model时，model的类型是一个数组，单个checkbox值是boolean类型
- radio对应的值是input的value值
- `input` 和`textarea` 默认对应的model是字符串
- `select`单选对应字符串，多选对应也是数组

### 2.4 v-on

v-on用来给页面元素绑定事件

语法：

```
v-on:事件名="js片段或函数名"
```

示例：

```html
<div id="app">
    <!--事件中直接写js片段-->
    <button v-on:click="num++">增加</button><br/>
    <!--事件指定一个回调函数，必须是Vue实例中定义的函数-->
    <button v-on:click="decrement">减少</button><br/>
    <h1>num: {{num}}</h1>
</div>
<script src="./node_modules/vue/dist/vue.js"></script>
<script type="text/javascript">
    var app = new Vue({
        el:"#app",
        data:{
            num:1
        },
        methods:{
            decrement(){
                this.num--;
            }
        }
    })
</script>
```

`v-on:click='add'`可以简写为`@click='add'`

#### 键盘按键修饰符

```vue
<!-- 缩写语法 -->
<input @keyup.enter="submit">
```

全部的按键别名：

- `.enter`
- `.tab`
- `.delete` (捕获“删除”和“退格”键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

### 2.5 v-for

v-for主要实现数据的遍历。

#### 遍历数组：

> 语法：

```
v-for="item in items"
```

- items：要遍历的数组，需要在vue的data中定义好。
- item：迭代得到的数组元素的别名

#### 数组+角标

> 语法

```
v-for="(item,index) in items"
```

- items：要迭代的数组
- item：迭代得到的数组元素别名
- index：迭代到的当前元素索引，从0开始。

#### 遍历对象

> 语法：

```
v-for="value in object"
v-for="(value,key) in object"
v-for="(value,key,index) in object"
```

- 1个参数时，得到的是对象的值
- 2个参数时，第一个是值，第二个是键
- 3个参数时，第三个是索引，从0开始

在进行数据渲染时，最好添加key。并保证唯一。

```vue
<ul>
    <li v-for="(item,index) in items" :key=index></li>
</ul>
```

### 2.6 v-if和v-show

#### v-if

顾名思义，条件判断。当得到结果为true时，所在的元素才会被渲染。

> 语法：

```
v-if="布尔表达式"
```

#### v-else

你可以使用 `v-else` 指令来表示 `v-if` 的“else 块”：

```vue
<div v-if="Math.random() > 0.5">
  Now you see me
</div>
<div v-else>
  Now you don't
</div>
```

`v-else` 元素必须紧跟在带 `v-if` 或者 `v-else-if` 的元素的后面，否则它将不会被识别。

#### v-show

另一个用于根据条件展示元素的选项是 `v-show` 指令。用法大致一样：

```
<h1 v-show="ok">Hello!</h1>
```

不同的是带有 `v-show` 的元素始终会被渲染并保留在 DOM 中。`v-show` 只是简单地切换元素的 CSS 属性 `display`。

### 2.7 v-bind

v-bind主要用来动态修改页面的属性。

> 数组语法

我们可以借助于`v-bind`指令来实现：

HTML：

```html
<div v-bind:class="isActive"></div>
```

你的data属性：

```js
data:{
    isActive:['active','hasError']
}
```

渲染后的效果：

```
<div class="active hasError"></div>
```

> 对象语法

我们可以传给 `v-bind:class` 一个对象，以动态地切换 class：

```html
<div v-bind:class="{ active: isActive }"></div>
```

上面的语法表示 `active` 这个 class 存在与否将取决于数据属性 `isActive` 的 [truthiness](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy)。

```html
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```

和如下 data：

```js
data: {
  isActive: true,
  hasError: false
}
```

结果渲染为：

```html
<div class="static active"></div>
```

当 `isActive` 或者 `hasError` 变化时，class 列表将相应地更新。例如，如果 `hasError`的值为 `true`，class 列表将变为 `"static active text-danger"`。

#### 简写

`v-bind:class`可以简写为`:class`

### 2.8 计算属性

Vue中提供了计算属性，来替代复杂的表达式：

```js
var vm = new Vue({
    el:"#app",
    data:{
        birthday:1429032123201 // 毫秒值
    },
    computed:{
        birth(){// 计算属性本质是一个方法，但是必须返回结果
            const d = new Date(this.birthday);
            return d.getFullYear() + "-" + d.getMonth() + "-" + d.getDay();
        }
    }
})
```

- 计算属性本质就是方法，但是一定要返回数据。然后页面渲染时，可以把这个方法当成一个变量来使用。

页面使用：

```html
    <div id="app">
       <h1>您的生日是：{{birth}} </h1>
    </div>
```

### 2.9 watch

watch可以让我们监控一个值的变化。从而做出相应的反应。

示例：

```html
<div id="app">
    <input type="text" v-model="message">
</div>
<script src="./node_modules/vue/dist/vue.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el:"#app",
        data:{
            message:""
        },
        watch:{
            message(newVal, oldVal){
                console.log(newVal, oldVal);
            }
        }
    })
</script>
```

## 三、组件

### 指定属性传递验证规则：

```vue
export default{
	data(){},
	props:{
		'title':String,
		'run':Object
	}
}
```

也可为其指定默认值：

```
export default{
	data(){},
	props:{ // 通过props来接收父组件传递来的属性
        items:{// 这里定义items属性
            type:Array,// 要求必须是Array类型
            default:[] // 如果父组件没有传，那么给定默认值是[]
        }
	}
}

```

验证规则有：

Number,

String

Boolean

Object

Array

...

### 组件通信

#### 父子组件通信：

##### 父组件给子组件通信

###### 父给子组件传值

1.父组件调用组件时，绑定动态属性	

```
<v-header :title="title"></v-header>
```

2.在子组件里面通过props接收父组件传过来的数据

```vue
export default{
	data(){},
	props:["title","msg"]
}
```

###### 父给子组件传递函数

1.父给子通过v-on指令将父组件函数，绑定到子组件上

```vue
<v-header :inc="incremengt"></v-header>
```

2.子组件调用

```vue
methods:{
                plus(){
                    this.$emit("inc");
                },
                reduce(){
                    this.$emit("dec");
                }
}
```

###### 父组件主动获取子组件的数据和方法

1.调用子组件的时候，定义一个ref

```vue
<v-header ref="hreader"></v-header>
```

2.在父组件取值

```vue
this.$refs.header.属性|方法
```

###### 子组件主动获取父组件的数据和方法

```vue
this.$parent.属性
this.$parent.方法
```

#### 非父子通信

1.新建一个js，引入vue实例化，然后暴露这个实例

2.要广播的地方引入上边的实例

3.通过VueEmit.$emit("名称","数据")

4.在接收数据的地方通过$on接收广播的数据：

```vue
VueEmit.$on("名称",function(){

})
```



存储：

localStroage

```vue
var storage={
	set(key,value){
		localStorage.setItem(key,JSON.stringify(value));
	},
	get(key){
		return JSON.parse(localStorage.getItem(key));
	},
	remove(key){
		localStorage.removeItem(key);
	}
}
```

cookie