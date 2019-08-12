## 入口函数

- 箭头函数没有独立作用域，是由上层决定的

  `xxx(name,value)=>{...this.element...}`中的this是windows的。

- 对象方法不加function是ES6提供的对象方法的简写形式。
- 

## extend方法

- extend方法参数不确定，可以不指定形参，通过函数内置对象arguments操作

  `jQuery.extend = function(...args){}`

- .语法 - 对象；没有. - 变量

- 构造函数内部的this指向函数的实例

  `const elements = document.querySelectorAll(传入的参数)`<-ES5中的函数，返回所有的元素。

## each方法

- 对象使用for...in...循环，数组使用for循环。

  ```js
  for(let i = 0;i < obj.length; i++)
  for(let i in obj)
  ```

  伪数组 : `{ 0:"a",length:3 }`也是用for循环。由于存在数组和伪数组2种情况，**一般来说**，伪数组都存在length属性，且值>=0，用这个特性来区分两种数组。

  ```js
  if( length in obj && obj.length>=0 ){
  	//是伪数组，可用for循环
  }
  ```

  判断是对象还是数组可以用isArray（）。

  

## 获取对象所有属性



- 输出属性

```js
function P(){//自身属性/自有属性
	this.name="XXX",
	this.age=18;
}
P.prototype.length=100;//原型属性
var p1= new P();

//获取自身属性和原型属性
for(let key in p1){}
//获取对象自身属性
Object.keys(p1).forEach((value,index)=>{
	console.log(value,index);
})
```

## CSS方法

- 获取样式 `$("div").css("color")`只能获取到第一个div 的样式。
- `$("div").css("color","red")`设置每一个div的颜色。

```js
jQuery.fn.extend({
    css(...args){
          //设置多个样式	参数个数：1
          //设置单个样式	参数个数：2	step1：遍历每一个DOM；step2：给DOM添加样式
        this.each(function(){//这边this表示一个jQuery对象
           //这个this表示一个DOM元素
            this.style[arg1]=arg2;
        })
    }
})
```

 

## this

```js
$.each([1,2,3],function(){
    //this指向1
})
$.each($("div"),function(){
    //this指向div元素
})
$("div").each(function(){
    //this指向div元素，与上条等价
})
```

## 链式调用

- 如果功能必须要有返回值，那么该功能不能实现链式。

  ex.获取样式、属性、内容

- 如果没有返回值

  ex.设置样式、内容

- 。。。

