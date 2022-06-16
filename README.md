# Classes & Prototypes

### What is class

`Class` is a schema.

### What is object

`Object` is a 'real' instance of schema.

`Object in JS` is an unordered dynamic collection of properties (key-value pairs).

**Everything** is an object

```
var obj = { prop0: 0 };
obj.prop1 = 1;

var func = function() {};
func.prop1 = 1;

var arr = [1, 2, 4];
arr.prop1 = 1;
```

#### key

`key` - unique (in scope of object) string

#### value

`value` - primitive, object, function

### Inheritance



#### classical

Под классическим понимается наследование в стиле ООП. В чистом JavaScript классического наследования нет и отсутствует понятие классов. И хотя современная спецификация EcmaScript добавляет синтаксические конструкции для работы с классами, это не меняет того факта, что на самом деле в нём используются функции-конструкторы и прототипирование. Поэтому данная техника нередко называется «псевдоклассическим» наследованием.

![image](https://user-images.githubusercontent.com/61269026/174101051-2e3d39f7-7cfb-4646-88a8-fca01d5bc42f.png)

В классическом ООП есть два типа абстракции: классы и объекты. Объект является сущностью реального мира, ну а класс является абстракцией объекта или другого класса.

Объекты в классических объектно-ориентированных языках программирования могут быть созданы только путем создания экземпляров классов:

```
class Human {
    // ...
}

class Man extends Human {
    // ...
}

Man johnDoe = new Man();
```

#### prototypal

Prototypal object-oriented programming languages are much simpler than classical object-oriented programming languages because in prototypal object-oriented programming we only have one type of abstraction (i.e. objects).

Objects in prototypal object-oriented programming languages may be created either ex-nihilo (i.e. out of nothing) or from another object (which becomes the prototype of the newly created object):

```
var human = {};
var man = Object.create(human);
var johnDoe = Object.create(man);
```


### Classes in JS
#### Introduction to classes
  
#### Defining a class

- object literals (not really classes)
- functions (more like classes but still not)
- ES2015 classes

#### Differences between functions and classes

При объявлении через class есть и ряд отличий:

- Класс нельзя вызывать без new, будет ошибка.
- Объявление класса с точки зрения области видимости ведёт себя как let. В частности, оно видно только в текущем блоке и только в коде, который находится ниже объявления (Function Declaration видно и до объявления).


Методы, объявленные внутри class, также имеют ряд особенностей:

- Методы имеют доступ к super.
- Все методы класса работают в строгом режиме use strict, даже если он не указан.
- Все методы класса не перечислимы. То есть в цикле for..in по объекту их не будет.


  #### Hoisting explained with classes
  #### Strict mode with classes explained

  Функция constructor запускается при создании new User, остальные методы записываются в User.prototype.

```
'use strict';

class User {

  constructor(name) {
    this.name = name;
  }

  sayHi() {
    alert(this.name);
  }

}

let user = new User("Вася");
user.sayHi(); // Вася

```

Это объявление примерно аналогично такому:

```
function User(name) {
  this.name = name;
}

User.prototype.sayHi = function() {
  alert(this.name);
};
```
В обоих случаях new User будет создавать объекты. Метод sayHi также в обоих случаях находится в прототипе.

  #### Static methods and usage
  #### Prototype methods explained

- technically - a regular JS object
- property of **every** function
- created by JS environment

### Classes in JS

#### Introduction to classes

- technically - a regular object
- property of every object
- accessible via __ proto __

```
function LegoMan(name) {
    this.name = name;
}

LegoMan.prototype.say = function(message) {
    console.log(this.name + ': "' + message + '"');
}

var alex = new LegoMan('Alex');
```

![Prototypes](https://kirilknysh.github.io/js-classes-talk/img/protolinks-0.png)

### Classes in JS ES6 classes

```
class LegoMan {
    constructor(name) {
        this.name = name;
    }

    say(message) {
        console.log(this.name + ': "' + message + '"');
    }
}
```

```
const alex = new LegoMan('Alex');
alex.say('Hello, Kattie!'); // Alex: "Hello, Kattie!"

const kattie = new LegoMan('Kattie');
kattie.say('Hello!'); // Kattie: "Hello!"
```
---

```
// TypeError: Class constructor LegoMan cannot be invoked without 'new'
const alex = LegoMan('Alex');
```

```
// SyntaxError: A class may only have one constructor
class LegoMan {
    constructor(name) { }

    constructor(name) { }
}
```
---

```
// ReferenceError: LegoMan is not defined
const alex = new LegoMan('Alex');
class LegoMan {
    constructor(name) {
        this.name = name;
    }
}
						
```

```
LegoMan.prototype.constructor === LegoMan; // true
```
---

```
class LegoMan {
    constructor(name) {
        this.name = name;
        this.age = 0;
    }

    set newAge(value) {
        this.age = value;
    }

    get represent() {
        return `My name is ${this.name}. I am ${this.age} years old.`;
    }
}

```

```
const alex = new LegoMan('Alex');
alex.represent // My name is Alex. I am 0 years old.
alex.newAge = 18;
alex.represent // My name is Alex. I am 18 years old.
```
---

```
class LegoMan {
    constructor(name) {
        this.name = name;
    }

    static getInfo(man) {
        return `This is ${man.name}.`;
    }
}
```
```
const alex = new LegoMan('Alex');
LegoMan.getInfo(alex); // This is Alex
alex.getInfo // undefined
```

## Всё это фигня залупная, в тесте не поможет, learnjavascript в помощь. Спасибо 🙂🔫