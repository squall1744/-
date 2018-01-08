### 构造函数(constructor)

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}
Person.prototype.sayName = function() {
  return this.name;
};
let student = new Person('Adam', 30);
```

### 混合模式(mixing)
继承
```js
let Person = function(name, age) {
  this.name = name;
  this.age = age;
};
Person.prototype.sayName = function() {
  console.log(this.name);
};

let Student = function(name, age, score) {
  Person.call(this, name, age);
  this.score = score;
};
Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;

Student.prototype.sayScore = function() {
  console.log(this.score);
};

let student = new Student('Adam', 28, 99);
```

### 模块模式(module)
通过闭包来实现一个模块
```js
let Person = (function() {
  let name = 'Adam';
  function sayName() {
    console.log(name);
  }
  return {
    name: name,
    sayName: sayName
  }
})();
```

### 工厂模式(factory)
就是每次生产一个对象出来, 关键点在于创建一个新的对象引用
```js
function createPerson(name) {
  let person = {
    name: name,
    sayName: function() {
      console.log(this.name)
    }
  };
  return person;
}
createPerson('Adam')
```

### 单例模式(singleton)
对象创建之后就不会变
```js
let People = (function() {
  let instance;
  function init(name) {
    return {
      name: name
    }
  }
  return {
    createPeople: function(name) {
      if(!instance) {
        instance = init(name);
      }
      return instance;
    }
  }
})();
People.createPeople('Adam'); //{name: 'Adam'}
People.createPeople('Eve'); //{name: 'Adam'}
```

### 发布订阅模式(publish/subcribe)
类似于jquery里的on绑定自定义事件, 然后用fire触发自定义事件

```js
let EventCenter = (function() {
  let events = {};
  
  function(evt, handler) {
    events[evt] = events[evt] || [];
    events[evt].push({
      handler: handler
    });
  }
  
  function fire(evt, args) {
    if(!events[evt]) return;
  
    for(let i = 0; i < events[evt].length; i++) {
      events[evt][i].handler(args);
    }
  }
  
  function off(evt) {
    delete events[evt]
  }
  return {
    on: on,
    fire: fire,
    off: off
  }
})();
```
