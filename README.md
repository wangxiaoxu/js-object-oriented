javascript 面向对象编程
=========
## 数据类型
**Primitive:**
- number
- string
- boolean
- undefined  

**Object:**  
- everything else

## 对象的定义

对象是无序属性的集合，其属性可以包含基本值、对象或者函数。

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
- 将构造函数的作用域赋值给新对象；
- 执行构造函数的代码；
- 返回新对象； 

缺点：每个方法都要在每个实例上面重新创建一遍。  
>一个解决方法是定义一个getName的全局方法






