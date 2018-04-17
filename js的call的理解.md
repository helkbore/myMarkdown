# js的call()的理解

之前也总是碰到, 然后每次都百度一下, 结果每次都是看似懂了然后之后又忘了. 觉得是一个难以越过的高山一样?

这次看了基础的教程.

个人理解就是对象A的方法, 用的参数是A的属性, 现在想用这个方法, 但是用的参数是别的对象的. 
```javascript
var person = {
        firstName : "John",
        lastName : "Doe",
        fullName : function() {
            return this.firstName + "  " + this.lastName;
        }
    }

    var myObject = {
        firstName : "Mary",
        lastName: "Doe"
    }

    console.log(person.fullName.call(myObject));

    console.log(person.fullName());
```