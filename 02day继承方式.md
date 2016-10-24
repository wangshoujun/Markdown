## 继承
> 一个对象可以使用另一个对象的东西，就叫继承。
> 一个对象可以使用本不属于自己的东西，就叫继承。
> js中的原型就是对继承特性的实现。
> 继承的本质就是代码的复用。
> js中的原型就是为了让代码复用，所以原型就是js对继承这个特性的实现。

## 继承的规律

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


### 继承方式1 - 默认的原型继承( 很常用 ) 
```
function Fn() {}
Fn.prototype.value = 100;
var fn = new Fn();
```

### 继承方式2 - 覆写构造函数的显式原型( 很常用 ) 
```
function Fn() {}
Fn.prototype = {
     value: 100
 }
var fn = new Fn();
```

### 继承方式3 - 给显式原型混入属性( 很常用 ) 
```
function extend(o1, o2) {
    for ( var key in o2 ) {
        o1[key] = o2[key];
    }  
}
var obj = { add: function (a,b) { console.log(a+b) } } 
function Fn() {}
extend(Fn.prototype, obj);
extend(Fn.prototype, {
    value: 100
});
var fn = new Fn();
```

### 继承方式4 - Object.create( 很少用 ) 
```
var obj = { value: 100 }
var newObj = Object.create(obj);
```

### 继承方式5 - 借用Object.create方法覆写显式原型( 很少用 ) 
```
var obj = { value: 100 }
function Fn() {}
Fn.prototype = Object.create(obj);
var fn = new Fn();0
```

### 继承方式6 - 复合式原型继承( 很少用 ) 
```
function PrFn() {}
PrFn.prototype.value = 100;
function Fn() {}
Fn.prototype = new PrFn()
var fn = new Fn();
```

***

### 枚举：

>可被遍历的，就叫枚举。
>内置的属性不可枚举，浏览器内置的属性无法使用 for in 遍历出来

#### extend方法：

>extend方法会把第二个对象的属性copy到第一个对象中.
>原理：
``` 
    function extend(obj1, obj2) {
        for ( var key in obj2 ) {
            obj1[key] = obj2[key];
        }
    }
```

>extend可以实现多继承，本质上就是把多个对象的属性依次copy到原型对象中。

例：
```
    var obj = { value: 888 };

    function Dog(age) {
        this.age = age;
    }
    // 分别把obj和{ }的属性copy到原型对象中，这样实例就可以使用了
    extend( Dog.prototype, obj );
    extend( Dog.prototype, {
        run: function () {
            console.log('跑');
        },
    });

    var dog = new Dog(3);
    console.log(dog.value); //888
    dog.run(); // 跑
```

#### Object.create方法：
>Object是内置的一个构造函数，在Object自身上有很多方法，其中有一个create方法，它可以实现继承。

* 语法：Object.create( 被继承的对象 );
* 返回值：返回一个新对象，新对象继承 传入 到create方法的对象。
* 作用：就是创建一个新对象，并且指定新对象继承的对象。

例：
```
    var obj = { val: 100 };

    // 指定newObj继承obj，即newObj.__proto__ === obj
    var newObj = Object.create(obj);
    console.log(newObj.val); //100
    console.log(newObj.__proto__ === obj); //true
```

例题：Person的实例可以使用obj里面的方法
```
    var obj = {
        fn: function () {
            // 谁调用fn，this指向谁
            console.log(this.name);
        }
    };
    function Person(name, age) {
        this.name = name;
        this.age = age;
    }


    方法1：可以把obj对象里面的方法copy到Person的显式原型中，达到实例共享的目的,完成需求
    for ( var key in obj ) {
        Person.prototype[key] = obj[key];
    }
    var guoJing = new Person('郭靖', 38);
    guoJing.fn();

    
    方法2：实例继承Person.prototype，Person.prototype继承obj，最终实例 间接 继承了obj
    Person.prototype = Object.create(obj);
    var guoJing = new Person('郭靖', 38);
    guoJing.fn();
```

### 实例成员与静态成员
* 添加给实例的属性或方法，就叫实例成员。
* 添加给类自己的属性或方法，就叫类成员(静态成员)

```
    // 这是构造函数，也是类
    function Person(name, age) {
        // 这里的name和age属性，因为将来要添加到实例身上，所以也称之为实例成员(实例属性)
        this.name = name;
        this.age = age;
    }

    // 直接添加到类上的属性与方法，叫静态成员，或者类成员。
    Person.maxAge = 200;
```