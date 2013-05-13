javascript 面向对象编程
=========
## 数据类型
**原始值类型:**
- number
- string
- boolean
- undefined  

**Object类型:**  
- everything else

## 对象的定义

对象是无序属性的集合，其属性可以包含基本值、对象或者函数。
**动态性**
执行时可以动态的添加，删除，修改
>有些属性不能被修改——（只读属性、已删除属性或不可配置的属性）  

在ES5规范中静态对象不能扩展新的属性，并且它的属性也不能被修改或者删除。他们是所谓的冻结对象，可以通过freeze(o)得到。
```javascritp
    var foo = {x: 10};
 
    // 冻结对象
    Object.freeze(foo);
    console.log(Object.isFrozen(foo)); // true
 
    // 不能修改
    foo.x = 100;
     
    // 不能扩展
    foo.y = 200;
     
    // 不能删除
    delete foo.x;
 
    console.log(foo); // {x: 10}
```
在ES5规范里，也使用Object.preventExtensions(o)方法防止扩展，或者使用Object.defineProperty(o)方法来定义属性。
```javascript
    var foo = {x : 10};
 
    Object.defineProperty(foo, "y", {
      value: 20,
      writable: false, // 只读
      configurable: false // 不可配置
    });
     
    // 不能修改
    foo.y = 200;
     
    // 不能删除
    delete foo.y; // false
     
    // 防治扩展
    Object.preventExtensions(foo);
    console.log(Object.isExtensible(foo)); // false
     
    // 不能添加新属性
    foo.z = 30;
     
    console.log(foo); {x: 10, y: 20}
```

## 创建一个对象

```javascript
    var person = new Object();
    person.name = 'zzc';
    person.sayName = function() {
        alert(this.name);
    }
    person.sayName();
```
缺点：使用同一个接口创建很多个对象，会产生大量的重复性代码。  

**工厂模式**  

```javascript
    function createPerson(name) {
        var o = new Object();
        o.name = name;
        o.getName = function() {
            return this.name;
        }
        retrun o;
    };
    
    var person1 = createPerson('zzc');
    var person2 = createPerson('tonycoolzhu');
```
缺点：没有解决对象识别问题。  

**构造函数模式**

```javascript
    function Person(name) {
        this.name = name;
        this.getName = function() {
            return this.name;
        }
    };
    
    var person1 = new Person('zzc');
    var person2 = new Person('tonycoolzhu');
```
调用new操作符经历以下几个步骤：
- 创建一个新对象；
- 将构造函数的作用域链赋值给新对象；
- 执行构造函数的代码；
- 返回新对象； 

缺点：每个方法都要在每个实例上面重新创建一遍。  
>一个解决方法是定义一个getName的全局方法 

```javascript
    function Person(name) {
        this.name = name;
        this.getName = getName;
    };
    
    function getName() {
        return this.name;
    };
    
    var person1 = new Person('zzc');
    var person2 = new Person('tonycoolzhu');
```

**原型模式**
```javascript
    function Person() {
        
    }
    
    Person.prototype = {
        constructor: Person,
        name: 'zzc',
        getName = function() {
            return this.name;
        }
    }
    
    var person1 = new Person();
    var person2 = new Person();
```

**组合使用构造函数模式和原型模式**
```javascript
    function Person(name) {
        this.name = name;
    }
    
    Person.prototype = {
        constructor: Person,
        name: 'zzc',
        getName = function() {
            return this.name;
        }
    }
    
    var person1 = new Person('zzc');
    var person2 = new Person('tonycoolzhu');
```
**动态原型模式**
```javascript
    function Person(name) {
        this.name = name;
        // methods
        if(typeof this.getName != "function") {
            Person.prototype.sayName = function() {
                return this.name;
            }
        }
    }
    
    var person = new Person('zzc')
    person.getName();
```
**稳妥构造函数模式**
>javascript中的稳妥对象指的是没有公共属性，而且其方法也不引用this的对象。 

```javascript
    function Person(name) {
        var o = new Object()
        o.getName = function() {
            return name;
        }
    }
    
    var person = Person('zzc');
    person.getName();
```
## 继承 
```javascript
    function Animal() {
　　　　this.species = "动物";
　　}
　　
　　function Cat(name,color) {
　　　　this.name = name;
　　　　this.color = color;
　　}
```
**构造函数继承**
```javascript
    function Cat(name, color){
　　　　Animal.apply(this, arguments);
　　　　this.name = name;
　　　　this.color = color;
　　}
　　
　　var cat1 = new Cat("大毛","黄色");
　　alert(cat1.species); // 动物
```
**prototype模式继承**
```javascript
    Cat.prototype = new Animal();
　　Cat.prototype.constructor = Cat;
　　var cat1 = new Cat("大毛","黄色");
　　alert(cat1.species); // 动物    
```
缺点：prototype如果包含引用类型的数据会造成问题。
**组合继承**
```javascript
    function SuperType(name) {
        this.name = name;
        this.colors = ['red', 'blue', 'green'];
    }
    
    SuperType.prototype.getName = function() {
        return this.name;
    }
    
    function SubType(name, age) {
        SuperType.call(this, name);
        this.age = age;
    }
    
    SubType.prototype = new SuperType();
    
    var instance = new SubType('name', 20);
```
缺点：无论在什么情况下，都会调用两次超类型构造函数。
**寄生组合式继承(YUI使用的继承方式)**
```javascript
    function object(o) {
        function F() {};
        F.prototype = o;
        return new F();
    }
    
    function inheritProperty(subType, superType) {
        var prototype = object(superType.prototype);
        prototype.constructor = subType;
        subType.prototype = prototype;
    }
    
    function SuperType(name) {
        this.name = name;
        this.colors = ['red', 'blue', 'green'];
    }
    
    SuperType.prototype.getName = function() {
        return this.name;
    }
    
    function SubType(name, age) {
        SuperType.call(this, name);
        this.age = age;
    }
    
    inheritPrototype(SubType, SuperType);
    
```

**拷贝继承(jquery使用的继承模式)**
'''javascript
    function objectCompositon(target, source) {
        var desc  = Object.getOwnPropertyDescriptor;
        var prop  = Object.getOwnPropertyNames;
        var def_prop = Object.defineProperty;
     
        prop(source).forEach(
            function(key) {
                def_prop(target, key, desc(source, key))
            }
        )
        return target;
    }
'''


