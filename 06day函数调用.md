
[TOC]

## 函数调用的几种方法

### 1、函数调用模式
> 函数调用模式  ==>  函数名( )

> 内部的this指向全局对象(window)

例1：
```
    function fn(){
        console.log(this)
    }
    fn();
     // 函数名调用，this指向window
```
例2：
```
    var obj = {
        inFn: function () {
            console.log(this);
        }
    };
    var outFn = obj.inFn;
    outFn();
    // 函数名调用，this指向window
```

### 2、方法调用模式
> 方法调用模式 == >  对象 . 对象名( )   ||   对象\[ 函数名 \]( )

> 内部的this指向方法所属的对象

例1：
```
    var obj = {
        fn: function () {
            console.log(this);
        }
    };
    obj.fn(); 
    // 方法调用模式，this指向obj
```
例2：
```
    function fn() {
        console.log(this);
    }
    var obj={};
    obj.f = fn;
    obj.f();
    // 方法调用模式，this指向obj
```
例3：
```
    var arr = {
        obje: {
            fn: function () {
                console.log(this);
            }
        }
    };
    arr.obje.fn();  
    //方法调用模式，this指向fn所属的obje对象
```
例4：
```
    var ob = {
        111: function () {
            console.log(this);
        }
    };
    ob[111](); 
    //方法调用模式，this指向111所属的ob对象
```
### 3、构造器调用
> 构造器调用模式 == > new 函数名( )  ||  new   对象.函数名( )  ||  new  对象\[ 函数名 \]( )
> 
> 内部的this指向新创建的实例

例1：
```
   
   function Person(age) {
       this.age = age;
       console.log(this);
       console.log(this instanceof Person);
   }
   new Person(16); // 这是构造器模式，内部的this指向新创建的实例 
```
例2：
```
    var obj = {
        fn: function () {
            console.log(this);
            console.log(this instanceof obj.fn);
        }
    };
    new obj.fn();  
    // 这是构造器模式，内部的this指向新创建的实例
```
### 4、间接调用模式
> 间接调用模式1 == >  函数名.call( )  ||  对象.函数名.call( )   ||  new  对象\[ 函数名 ].call( )
> 
> 间接调用模式2 == >  函数名.apply( )  ||  对象.函数名.apply( )  ||  new  对象 \[ 函数名 \].apply( )
> 
> 内部的this指向自定义的对象，如果传入空，this指向全局对象window。

例1：
```
    function Fn() {
        console.log(this);
    }
    Fn.apply(Fn);  
    // 间接调用模式，this指向Fn
```
例2：
```
    var o = {
        f : function () {
            console.log(this);
        },
        2 : function () {
            console.log(this);
        }
    };
    o.f.call([1,2]);  // 间接，this指向[1,2]
    o[2].call([1,2,3,4]); // 间接，this指向[1,2,3,4]
```

***

### call( )、apply( ) 和 bind()  
> 这三个方法都是来自于Function.prototype上，所以所有的函数都可以使用。
> 他们有一个共同点，就是可以指定函数执行时内部this的指向。

#### call、apply、bind的区别：
- call和apply的区别在于参数的方式。
- bind和前两个方法的区别在于，bind不会马上执行函数，而是返回一个函数，供以后调用。 

#### call、apply、bind的语法：
* 语法：函数名.call( 指定函数执行时的this指向 ，实参1, 实参2....);
* 语法：函数名.apply( 指定函数执行时的this指向 ，[实参1, 实参2....] );
* 语法：var fn = 函数名.bind( this指向，要绑定的实参1, 要绑定的实参2.... );

例1：
```
    function fn() {
        console.log(this);
    }

    // 通过fn调用call方法，call方法内部会返过来调用fn，并且指定fn执行时内部的this为数组。
    fn.call([1,2,3,4,5]);
    fn.apply(new Date);
    fn.apply(new RegExp);
```
例2：
```
    var obj = {
        fn: function () {
            console.log(this);
        }
    };

    obj.fn.call(['a']);
    obj.fn.call({val:1});
    obj.fn.apply(new Boolean);
```
例3：
```
    function fn() {
        console.log(this);
    }

    fn.call({val:1}); // 内部的this指向字面量对象

    fn.call(1);  // 内部的this指向1的包装对象
    fn.apply('a'); // 内部的this指向'a'的包装对象

    fn.apply(null); // 内部的this指向window
    fn.call(undefined); // 内部的this指向window
    fn.call(); // 内部的this指向window
```
#### ⑴ call( )用法：

* 语法：函数名.call( 自定义的this指向, 实参1，实参2, 实参3, 实参4.... );
* 注意：第一个参数只是为了指定函数执行时this的指向，并不会作为参数传入进去。

```
    function add(a, b)  {
        console.log(a + b);
    }
    add.call(300, 10, 20);  // 结果为30

```



#### ⑵ apply( )用法：

* 语法1：函数名.apply( 自定义的this指向, [实参1，实参2, 实参3, 实参4....] );
* 语法2：函数名.apply( this指向，{0: 实参1, 1: 实参2, length: 2} )
* 注意：第一个参数只是为了指定函数执行时this的指向，并不会作为参数传入进去,第二个参数要求是数组或伪数组，apply会自动把数组中的内容平铺后传入到函数中。

```
    function add(a,b) {
        console.log(a + b);
    }
    add.apply(300, [50, 50]);  // 结果100
    add.apply(300, { 0:20, 1:20, length:2 });  // 结果40
```

#### ⑶ bind( )用法：
* 语法：var fn = 函数名.bind( this指向，要绑定的实参1, 要绑定的实参2.... );
* 返回值：函数的copy版本

```
      var baseNumber = 0;
      var obj = {baseNumber:100};
      function add(a, b, c) {
          console.log(this.baseNumber + a + b + c);
      }

      var fn = add.bind(obj);

      fn(1,2,3);  //106
      fn(1,2,54);  //157

    // bind绑定实参的使用：
    // 如果前两个参数一直是10，
    // 就可以通过bind绑定死，以后就不用传了
    var fn = add.bind(obj, 10, 10); // 相当于固定形参a为10，形参b为10
    fn(50);  // 调用fn时，只需再传形参c的值即可  170
    fn(30); // 调用fn时，只需再传形参c的值即可   150
```



### toString的使用方法：
* Object.prototype.toString执行时根据内部的this，返回一个这样的字符串:" [object  this的类型]"

```
   console.log(Object.prototype.toString()); 
    // 返回'[object Object]'

   console.log(Object.prototype.toString.call([1,2])); 
   // 因为[1,2]是Array类型的对象，返回'[object Array]'

   console.log(Object.prototype.toString.call(Array)); 
   // 因为Array是Function类型的对象，返回'[object Function]'

   console.log(Object.prototype.toString.call(Math)); 
   // 因为Array是Function类型的对象，返回'[object Function]' 
```

* 内置的9大构造函数，它们的prototype显式原型对象中，都定义了自己的toString方法，所以他们的实例，会优先使用自己的toString。
* 如果想要通过Object原型上的toString获取某个对象的类型，为了防止意外，必须得这样做 ： Object.prototype.toString.call( 要判断的对象 );

```
    // arr会调用优先调用Array.prototype上的toString
    var arr = [1,2,3];
    console.log(arr.toString());  //1,2,3

    // fn会调用优先调用Function.prototype上的toString
    function fn() {}
    console.log(fn.toString());  //function fn() {}

    // new Boolean会调用优先调用Boolean.prototype上的toString
    console.log((new Boolean).toString());    //false

     console.log(Math.toString());  //[object Math]

```

### 方法借用
1. push方法可以给数组添加属性值

```
    var arr = [];
    arr.push(10);
    console.log(arr);
```
2. 借用数组的push方法，给obj对象按照下标添加属性值

```
    var obj = {};
    [].push.call(obj, 10);
    console.log(obj);   
```
3. 借用数组的pop方法，删除伪数组o对象最后一个下标属性值

```
   var arr = {
       0: 10,
       1: 20,
       2: 30,
       length: 3
   };
   [].pop.call(arr);
   console.log(arr); 
```

### 构造函数调用
```
function Person(name, age) {
    this.name = name;
    this.age = age;
}

// 目的：给Student的实例也添加name和age属性
function Student(name, age) {

    /* Student执行时，this指向小红(即小红就是this，this就是小红)，
       那么我想要Person执行时，它里面的this指向小红，
       就可以通过call指定Person里面的this为小红，
       因为小红就是this，所以给call传this。*/

    Person.call( this, name, age );
}

var xiaohong = new Student('小红', 16);
console.log(xiaohong);
```