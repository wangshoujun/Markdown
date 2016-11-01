[TOC]

# 重点复习
#### 原型链的概念 
> 一个对象继承的所有对象称之为对象的原型链，对象的原型链结构可长可短。原型链是描述对象的继承结构

#### 函数的原型链：
> fn ==> Function.prototype ==> Object.prototype ==> null
> 所以函数.prototype  都继承 Object.prototype

#### 什么类型的对象既有prototype属性又有__proto__属性？
> 函数类型的对象

#### 直接继承Object.prototype的对象有哪几种？
> {}、new Object 、内置构造函数.prototype 、自定义函数.prototype、 Math

#### 直接继承Function.prototype的对象有哪几种？
> 内置的构造函数、字面量定义的函数、new Function创建的函数

***

## 继承的结构：
### 字面量对象的继承结构:
 1、var obj = { } ;

* obj的继承结构:obj ==> Object.prototype ==> null
* new Object 创建的实例，继承Object.prototype;和{}字其实{}就是new Object的简写形式。

 2、var arr = [ ] ;

* arr的继承结构:arr ==> Array.prototype ==> Object.prototype ==> null
* new Array 创建的实例，继承Array.prototype;和[]字面量对象一致，其实[]就是new Array的简写形式。

### 字面量的形式：
1. 数组 [ ]
2. 对象 { }
3. 正则表达式 / abc /
4. 函数 function ( ) { }


### 继承的规律

 1.  对象继承的终点是Object.prototype
 2.  所有函数默认的显示原型（即函数的prototype）都继承 Object.prototype
 3.  谁的实例，这个实例就继承谁的prototype
    3.1 所有的函数，都被看作是Function的实例，所有都继承Function.prototype
    3.2 所有的数组，都被看作是Array的实例，所有都继承Array.prototype
    3.3 所有的正则，都被看作是RegExp的实例，所有都继承RegExp.prototype

### function  fn() {}

 1.  fn 是Function 的实例，所以继承Function.prototype
 2.  fn.prototype 继承 Object.prototype
 3.  new fn()是fn的实例，所以继承fn.prototype

### 函数的原型链结构：
>在js中，函数比较特殊，他们都是Function类型的对象;但是这些函数可以派生出属于自己类型的对象。

例如：
1. Date自身是Function类型,但是通过new Date创建出来的所有实例都是Date类型。
2. 一个自定义的函数，假设名叫fn，fn自身是Function类型，但是通过new fn创建出来的所有实例都是fn类型。

#### ECMAScript 内置的-----函数类型的对象：
>String、Number、 Boolean 、RegExp、 Function 、Object 、Array、 Error、 Date
>上述9大内置构造函数的原型链结构：
9大构造函数 ==> Function.prototype ==> Object.prototype ==> null

#### ECMAScript 内置的-----非函数类型的对象： Math
>Math的原型链结构：Math ==> Object.prototype ==> null


***

### instanceof运算符的运算规则：
> 判断左边的对象的原型链中有没有右边构造函数.prototype指向的对象。
> 语法：对象  instanceof  构造函数
> 返回值：boolean 

例：
```
    function Person() {}
    var xiaofang = new Person();

    //原型链：xiaofang ==> Person.prototype ==> Object.prototype ==> null
    console.log( xiaofang instanceof Person );  // true
    console.log( xiaofang instanceof Object );  // true

```

### for in 语句
- for in用来遍历一个对象可枚举的属性
- 需要注意：该对象继承的属性也能够被遍历出来


### hasOwnProperty和propertyIsEnumerable有什么区别？
- hasOwnProperty用来判断一个对象是否含有某个属性
- propertyIsEnumerable不光用来判断一个对象是否含有某个属性，还要判断这个属性是不是可枚举的

### 函数的length属性和arguments对象的length属性有什么区别？
- 函数的length属性用来获取形参的个数
- arguments对象的length属性用来获取实参的个数

### Function 和 eval 有什么共同特点？
> 都可以把字符串转换为代码执行。

***

## 静态成员与实例成员：
### 静态成员(类成员)
> 添加给构造函数自己的属性与方法，称之为静态成员。

### 实例成员
> 添加给实例或者构造函数原型中的属性与方法，称之为实例成员。

例：
```
    function Person( name, age ) {
        // 这是添加给将来的实例的，所以这是实例成员
        this.name = name;
        this.age = age;
    }

    // 这是添加给类自己的，所以这是类成员
    Person.MAX_AGE = 200;
```

```
    在JQuery中，
    $.ajax ，这里面的ajax方法就是静态成员（类成员）
    $('div').css() ，这里面的css方法就是实例成员
```

### 什么情况下会用到静态成员：
1. 如果一些属性和类的关联性比较大，那么可以考虑作为静态属性存在
2. 如果一些方法具有通用性，那么可以考虑将其作为静态方法存在

***

## Object.prototype上常用的几个方法：

### 1、hasOwnProperty：
* 作用：判断一个对象是否(自己)含有某个属性
* 语法: 对象.hasOwnProperty( 要判断的属性名 )
* 返回值：boolean

```
    var obj = { a: 1 };
    console.log(obj.hasOwnProperty('a'));  // true
    console.log(obj.hasOwnProperty('b'));  // false
    console.log(obj.hasOwnProperty('constructor'));  // false
    console.log(Object.prototype.hasOwnProperty('hasOwnProperty'));  // true
```

### 2、propertyIsEnumerable:
* 作用：判断一个对象是否(自己)含有某个属性，并且还要判断这个属性是不是可枚举的,这个方法是一个双重判断，通常称这个方法为hasOwnProperty的加强版。
* 语法: 对象.propertyIsEnumerable( 要判断的属性名 )
* 返回值：boolean

```
    var obj2 = { a: 1 };
    console.log(obj2.propertyIsEnumerable('a'));  // true
    console.log(obj2.propertyIsEnumerable('b')); // false
    console.log(obj2.propertyIsEnumerable('constructor'));  // false
    console.log(Object.prototype.propertyIsEnumerable('hasOwnProperty'));  // false
```

### 3、isPrototypeOf:
* 作用：判断一个对象是不是另一个对象的原型对象
* 语法：被判断的对象.isPrototypeOf( 对象 )
* 返回值：boolean

```
    console.log( Object.prototype.isPrototypeOf( Object ) );  // true
    console.log( Function.prototype.isPrototypeOf( Function ) ); // true
    console.log( Function.prototype.isPrototypeOf( Math ) );  // false，因为Math只继承Object.prototype
```

### 4、toString：
* 作用：根据方法执行时内部的this指向
* 返回一个类似于这样的字符串：'[object this对象的类型名称]'
* 通过这个字符串，可以得知内置对象的类型。

```
    Math.toStr = Object.prototype.toString;
    console.log(Math.toStr()); // [object Math]

    Date.toStr = Object.prototype.toString;
    console.log(Date.toStr()); // [object Function]

    var date = new Date();
    date.toStr = Object.prototype.toString;
    console.log(date.toStr()); // [object Date]   
```

***

## 函数属性：
1. arguments 代表实参的伪数组对象
2. caller 返回调用该函数的函数
3. name 函数的名字
4. length 形参的个数

```
    function fn(a, b, c) {
        console.log(arguments); 
    }
    
    console.log(fn.name); //fn
    console.log(fn.length); // 3   
    fn(15,20); // [15 , 20]
```

### 函数的length属性作用：
```
    function add(a, b) {
        if ( arguments.length !== add.length ) {
            throw '参数个数不符！';
        }
        console.log(a + b);
    }

    add(10,20);  //30
    add(6); // Uncaught 参数个数不符！
```


## 运算符
### in 运算符
- 作用：用来判断一个对象能否使用某个属性
- 语法：'属性名'  in  对象
- 返回值：boolean
- 需要注意：in运算符单独使用时，与属性是不是可枚举的没有任何瓜葛

```
    var obj = { aa: 11 };
    console.log( 'aa' in obj ); // true
    console.log( 'bb' in obj ); // false
    console.log( 'hasOwnProperty' in obj ); // true
```



### delete 运算符
- 作用：删除对象的属性。
- 语法：delete 对象.属性名

```
    var obj = { aa : 10, bb : 230, 2: 200 };
    delete obj.bb;
    delete obj[2];
    console.dir(obj); // { aa : 10 }
```

### Function创建函数的语法：
- new Function( arg_name1, arg_name2, arg_name3, functionBody )
- 前面可以定义任意数量的形参，最后一个参数代表函数的代码体
- 注意：这些参数必须是字符串的形式。
- 返回值：一个新创建的函数实例。

```
    function add(a, b) {
        console.log(a + b);
    }
    //Function创建函数，会把字符串当做代码执行
    var addObj = new Function('a', 'b', 'console.log(a + b);');
    addObj(10, 60); // 70
```

### eval()
- eval可以直接把字符串当做代码执行。
- 语法：eval(字符串代码)

例：eval('console.log(123223)');

### JSON
- JSON数据格式 ==>   '{ "name": "李四" }'
- 为了方便操作JSON数据，ES5提供了JSON对象，里面有两个方法
1. JSON.parse ： 用来把JSON数据转换为js对象
2. JSON.stringify : 用来把js对象转换为JSON数据

```
    var JSONString = '{ "name": "李四" }';
    console.log(JSON.parse(JSONString)); // Object {name: "李四"}

    var obj = { value: 100, val: 320 };
    console.log(JSON.stringify(obj));  // {"value":100,"val":320}
    console.log(typeof JSON.stringify(obj));  // string
```

