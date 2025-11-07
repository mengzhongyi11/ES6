## ES6  class的写法

1. ### 基本定义与实例化

- 用  class  关键字定义类， constructor  是构造方法（实例创建时自动执行）。

- 用  new  关键字创建实例。
  
- ```js
  - class Person {
    constructor(name, age) { // 构造方法：初始化实例属性
      this.name = name;
      this.age = age;
    }
  
    sayHi() { // 实例方法（挂载到原型上）
      console.log(`Hi, ${this.name}`);
    }
    }
  
  const p = new Person("Tom", 20);
  p.sayHi(); // Hi, Tom
  ```
  
  

2. ### 核心特性

- **静态方法：用  static  修饰，属于类本身，不挂载到原型，需通过类调用。**

  ```js
  class Person {
  static create(name) { // 静态方法
    return new Person(name);
  }
  }
  const p = Person.create("Jerry"); // 类直接调用
  ```

  

- **继承：用  extends  实现继承， super()  调用父类构造方法。**

  ```js
  class Student extends Person {
  constructor(name, age, grade) {
    super(name, age); // 必须先调用父类构造器
    this.grade = grade;
  }
  }
  ```

   **getter/setter：拦截属性的读取/赋值。**

  ```js
  class Person {
  get name() { return this._name; }
  set name(val) { this._name = val.trim(); }
  }
  ```

  