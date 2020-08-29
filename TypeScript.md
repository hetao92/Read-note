[TOC]

# TypeScript

## 基础类型

### 数字

浮点数，支持二进制、八进制、十进制和十六进制

`let decLiteral: number = 6`

### 字符串

`let name: string = 'bob'`

### 布尔值

`let isDone: boolean = false`

### 数组

在元素类型后接上`[]` ： `let list: number[] = [1, 2, 3]`

使用数组泛型 `Array<元素类型T>` `let list : Array<number> = [1, 2, 3]`



TypeScript 具有 `ReadonlyArray<T>`类型，与 `Array<T>`相似，但把所有可变方法去掉了，可以确保数组创建后不能被修改

```ts
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error! 
//即使是把整个ReadonlyArray赋值到一个普通数组也是不可以的，但可以用类型断言重写
a = ro as number[]
```



### 元组

允许一个已知元素数量和类型的数组，各元素的类型不必相同)

```typescript
let x: [string, number];
x = ['hello', 10]		//right
x = [10, 'hello']		//wrong
//同时对于访问已知索引的元素也会判断其类型
console.log(x[1].substr(1)) //Error, 'number' does not have 'substr'
// 当访问一个越界的元素，会使用联合类型替代
x[3] = 'world' //ok, 字符串可以赋值给（string | number）类型
console.log(x[5].toString()) //OK, string 和 number 类型都有 toString
x[6] = true //Error, 布尔不是(string | number) 类型
```

### 枚举 enum

枚举是对 JavaScript 标准数据类型的一个补充，可以为一组数组赋予友好的名字

```ts
enum Color {Red, Green, Blue} 
let c: Color = Color.Green
//默认情况下，从 0 开始为元素编号，但可以手动指定
enum Color {Red = 1, Green = 2, Blue = 4}

let colorName: string = Color[2];
console.log(colorName);  // 显示'Green'因为上面代码里它的值是2
```



### Any

类型检查器将不对 `any ` 类型值检查而是直接通过编译阶段的检查，`Object` 类型有相似的作用，但它的变量只允许赋任意值，但却不能在上面调用任意的方法，即使真的有这个方法。



### Void

与`any`相反，它表示没有任何类型，通常一个函数没有返回值时可以定义

```ts
function warnUser(): void {
  console.log('this is my warning message')
}
//定义 void 类型的变量没有意义，因为只能赋予它 null 或者 undefined
```





### Null 和 undefined

`null` 和 `undefined` 是所有类型的子类型，比如可以把它们赋值给 `number`类型的变量

当指定了`--strictNullChecks`标记，`null`和`undefined`只能赋值给`void`和它们各自。 

### never

never 类型是指永不存在的值的类型。比如总是会抛出异常或根本不会有返回值的函数表达式或箭头函数表达式的返回值类型，变量也可以是 never 类型，当它们被永不为真的类型保护所约束时。

never 类型是任何类型的子类型，也可以赋值给任何类型。但没有类型是 never 的子类型或赋值给 never 类型，即使 any 也不可以赋值给 never

```typescript
// 返回 never 的函数必须存在无法到达的终点
function error(message: string): never {
  throw new Error(message)
}

//推断的返回值类型为 never
function fail() {
  return error('Something failed')
}
 
```

### object

object表示非原始类型，即除`number`,`string`,`boolean`,`symbol`,`null`或`undefined`之外的类型

```tsx
declare function create(o: object | null): void;
create({ prop: 0 }) //OK
create(null)				//OK
//以下皆为 Error
create(42)	
create('string')	
create(false)
create(undefined)
```



### 类型断言

类型断言相当于其他语言里的类型转换，但不进行特殊的数据检查和解构。它只是在编译阶段起作用，没有运行时的影响

两种形式

+ 尖括号语法

  ```typescript
  let someValue: any = 'this is a string'
  let strLength: number = (<string>someValue).length
  ```

+ as 语法

  ```typescript
  let someValue: any = 'this is a string'
  let strLength: number = (someValue as string).length
  ```

两种形式等价，不过在使用 JSX 时只有 as 语法是被允许的



**声明结构 **

```ts
let {a, b}: {a: string, b: number} = o;
function f([first, second]: [number, number]) {}
```



## 接口

```ts
interface SquareConfig {
  color: string;
  width?: number;		//属性定义后加 ？表示可选
  readonly x: number;	//readonly 指定只读属性
}
```

### 可选属性可能出现的问题

#### 索引签名

对于可选属性， 对象字面量会被 typescript 特殊对待，做额外的属性检查，当将它们赋值给变量或作为参数传递的时候，若对象字面量存在任何目标类型不包含的属性时将会报错

```ts
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
    // ...
}

let mySquare = createSquare({ colour: "red", width: 100 });
// error: 'colour' not expected in type 'SquareConfig'
```

**解决办法**

1. 可以通过使用类型断言来绕开检查

   ```ts
   let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig)
   ```

   #### 

2. 将对象赋值给另一个变量

```ts
let squareOptions = { colour: "red", width: 100 };
let mySquare = createSquare(squareOptions);
//因为 squareOptions不会经过额外属性检查，所以编译器不会报错。
```

3. 若能够确定对象具有一些作为特殊使用的额外属性，最佳方式是添加**字符串索引签名**

#### 索引签名

```ts
interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any; //任意数量的随意类型的属性
}
```



### 函数类型

使用接口表示函数类型，我们需要给接口定义一个**调用签名**。 它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。

```ts
interface SearchFunc {
  (source: string, subString: string): boolean;
}
let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}
```



在对函数类型做检查时，函数的参数名并不一定要和接口内定义的名字相匹配，他会要求对应位置上的参数类型是兼容的

```ts
mySearch = function(src: string, sub: string): boolean {	}	//right
```



或者对参数不想指定类型，typescript 也会推断出参数类型，包括返回值类型也可以根据其返回值推断出来

```ts
mySearch = function(src, sub) {
    let result = src.search(sub);
    return result > -1;
}	//若是返回字符串数字等，也会发出警告
```



### 可索引的类型

ts 也可以描述一些能 “通过索引得到” 的类型，比如`a[10]` `a['b']`。这种类型具有一个**索引签名**，描述对象索引的类型，以及相应的索引返回值类型

```ts
interface StringArray {
  [index: number]: string;	//当用 number去索引 StringArray 时会得到 string 类型的返回值。 
}

let myArray: StringArray;
myArray = ["Bob", "Fred"];

let myStr: string = myArray[0];
```

Ts 支持两种索引签名，字符串和数字，可以同时使用两种类型的索引，但数字索引的返回值必须是字符串索引返回值类型的子类型，因为当用 number 索引时，JavaScript 会将它转换成 string 再去索引对象。

字符串索引签名能够很好的描述`dictionary`模式，并且它们也会确保所有属性与其返回值类型相匹配。 因为字符串索引声明了 `obj.property`和`obj["property"]`两种形式都可以。 下面的例子里， `name`的类型与字符串索引类型不匹配，所以类型检查器给出一个错误提示：

```ts
interface NumberDictionary {
  [index: string]: number;
  length: number;    // 可以，length是number类型
  name: string       // 错误，`name`的类型与索引类型返回值的类型不匹配
}
```



同时可以对索引签名设置为只读，可以防止给索引赋值

```ts
interface ReadonlyStringArray {
    readonly [index: number]: string;
}
let myArray: ReadonlyStringArray = ["Alice", "Bob"];
myArray[2] = "Mallory"; // error!
```



### 类类型

类接口里只描述了类的公共部分，而不包括私有部分

```ts
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date);
}

class Clock implements ClockInterface {
    currentTime: Date;
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
}
```



例如 constructor 是类的构造函数，属于静态部分

```ts
interface ClockConstructor {
    new (hour: number, minute: number): ClockInterface;
}
interface ClockInterface {
    tick();
}

function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
    return new ctor(hour, minute);
}

class DigitalClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {
        console.log("beep beep");
    }
}
class AnalogClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {
        console.log("tick tock");
    }
}

let digital = createClock(DigitalClock, 12, 17);
let analog = createClock(AnalogClock, 7, 32);
//因为createClock的第一个参数是ClockConstructor类型，在createClock(AnalogClock, 7, 32)里，会检查AnalogClock是否符合构造函数签名。
```



### 接口继承

接口可以相互继承，也可以继承多个接口，从而创建出多个接口的合成接口

```ts
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```



### 混合类型

一个对象可以同时作为函数和对象使用，并带有额外的属性

```ts
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}

function getCounter(): Counter {
    let counter = <Counter>function (start: number) { };
    counter.interval = 123;
    counter.reset = function () { };
    return counter;
}

let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```



### 接口继承类

当接口继承一个类类型时，会继承类的成员但不包括其实现。它同样会继承到类的 private 和 protected 成员，所以当创建了一个接口继承了一个拥有私有或受保护的成员的类时，这个接口类型只能被这个类或其子类所实现

```ts
class Control {
    private state: any;
}

interface SelectableControl extends Control {
    select(): void;
}
class Button extends Control implements SelectableControl {
    select() { }
}

class TextBox extends Control {
    select() { }
}

// 错误：“Image”类型缺少“state”属性。
class Image implements SelectableControl {
    select() { }
}

// SelectableControl包含了Control的所有成员，包括私有成员state。 因为 state是私有成员，所以只能够是Control的子类们才能实现SelectableControl接口。 因为只有 Control的子类才能够拥有一个声明于Control的私有成员state，这对私有成员的兼容性是必需的。
// 在Control类内部，是允许通过SelectableControl的实例来访问私有成员state的。 实际上， SelectableControl接口和拥有select方法的Control类是一样的。 Button和TextBox类是SelectableControl的子类（因为它们都继承自Control并有select方法），但Image类并不是这样的
```



## 类

```ts
class Greeter {
	greeting: string
  constructor(message: string) {
    this.greeting = message
  }
  greet() {
    return this.greeting
  }
}
```

### 公共属性 public

`public`可省略



### 私有属性 private

当属性标记为 `private` 时，就不能再类的外部访问。

```ts
class Animal {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}

new Animal("Cat").name; // 错误: 'name' 是私有的.
```

而且当比较带有 `private` 或者 `protected` 的类型时，若其中一个类型有 private 成员，那么只有当另一个类型也存在这样一个 private 成员，且都来自于同一个声明，typescript 才会认为这两个类型是兼容的。通常是父类与他的子类是兼容的，其他类即使定义的一模一样，在比较相等时也会得到错误



### protected属性

`protected`修饰符与 `private`修饰符的行为很相似，但 `protected`成员在派生类中仍然可以访问。

```ts
class Person {
    protected name: string;
    constructor(name: string) { this.name = name; }
}

class Employee extends Person {
    private department: string;

    constructor(name: string, department: string) {
        super(name)
        this.department = department;
    }

    public getElevatorPitch() {
        return `Hello, my name is ${this.name} and I work in ${this.department}.`;
    }
}

let howard = new Employee("Howard", "Sales");
console.log(howard.getElevatorPitch());
console.log(howard.name); // 错误
// 不能在 Person 类外使用 name，但仍可以通过 Employee 类的实例方法访问，因为 Employee 是由 Person 派生而来
```



构造函数也可以被标记为 protected，则这个类不能在包含它的类外被实例化，但能被继承

```ts
class Person {
    protected name: string;
    protected constructor(theName: string) { this.name = theName; }
}

// Employee 能够继承 Person
class Employee extends Person {
    private department: string;

    constructor(name: string, department: string) {
        super(name);
        this.department = department;
    }

    public getElevatorPitch() {
        return `Hello, my name is ${this.name} and I work in ${this.department}.`;
    }
}

let howard = new Employee("Howard", "Sales");
let john = new Person("John"); // 错误: 'Person' 的构造函数是被保护的.
```



### readonly 修饰符

只读属性**必须**在声明时或构造函数里被初始化。

```ts
class Octopus {
    readonly name: string;
    readonly numberOfLegs: number = 8;
    constructor (theName: string) {
        this.name = theName;
    }
}
let dad = new Octopus("Man with the 8 strong legs");
//只读属性 name 会在构造函数里将 theName 赋值给它
```



### 参数属性

上述情况可以用参数属性来**直接定义并初始化**成员

```ts
class Octopus {
    readonly numberOfLegs: number = 8;
    constructor(readonly name: string) {
    }
}
// 直接使用 readonly name: string 来创建和初始化 name 成员
// 参数属性通过给构造函数的参数前面添加一个访问限定符来声明。不一定要使用 readonly
// 使用 private 限定一个参数属性会声明并初始化一个私有成员，同理可得 public 和 protected
```



### 存取器

Ts 支持通过`getters/setters`来截取对象的访问，只带`get`而不带有`set`的存取器自动被推断为`readonly`



### 抽象类

抽象类作为其他派生类的基类使用，一般不会直接被实例化，不同于接口，抽象类可以包含成员的实现细节，`abstract`关键字是用于定义抽象类和在抽象类内部定义抽象方法。

抽象类中的抽象方法不包含具体实现并且必须在派生类中实现，抽象方法的语法与接口方法相似，两者都是定义方法签名但不包含方法体。不过抽象方法必须包含`abstract`关键字并且可以访问修饰符

```ts
abstract class Department {

    constructor(public name: string) {
    }

    printName(): void {
        console.log('Department name: ' + this.name);
    }

    abstract printMeeting(): void; // 必须在派生类中实现
}

class AccountingDepartment extends Department {

    constructor() {
        super('Accounting and Auditing'); // 在派生类的构造函数中必须调用 super()
    }

    printMeeting(): void {
        console.log('The Accounting Department meets each Monday at 10am.');
    }

    generateReports(): void {
        console.log('Generating accounting reports...');
    }
}

let department: Department; // 允许创建一个对抽象类型的引用
department = new Department(); // 错误: 不能创建一个抽象类的实例
department = new AccountingDepartment(); // 允许对一个抽象子类进行实例化和赋值
department.printName();
department.printMeeting();
department.generateReports(); // 错误: 方法在声明的抽象类中不存在
```



### 函数

函数类型包含 参数类型 和 返回值类型

```ts
let myAdd: (x: number, y: number) => number =		// 返回值和函数间用 => 符号
    function(x: number, y: number): number { return x + y; };
```

**上下文归类/推断类型**

如果在赋值语句的一边指定了类型但是另一边没有类型的话，TypeScript编译器会自动识别出类型

Typescript 函数中的每个参数是必须的，且传递的参数个数必须与函数期望的参数个数一致

**可选参数** `?` 必须跟在必须参数后面

在所有必须参数后面的带默认初始化的参数都是可选的，与可选参数一样，在调用函数的时候可以省略。 也就是说可选参数与末尾的默认参数共享参数类型。

但默认值参数若放在必须参数前面，则必须传值，`undefined`

**剩余参数** `...args: string[]`



#### this

typescript 可以通过提供一个显式的 this 参数（假参数），在参数列表的最前面，来绑定 this

```ts
function f(this: void) {	//若是绑定 void 则说明期望这就是个普通函数
    // make sure `this` is unusable in this standalone function
}
```



如果期望调用类函数的外部函数获取 this 为 void，但类函数可以获取到类的一些参数，可以将类函数写成箭头函数，因为箭头函数不会不会捕获 this，所以总是可以把它们传给期望`this: void`的函数

```ts
interface UIElement {
    addClickListener(onclick: (this: void, e: Event) => void): void;
}
class Handler {
    info: string;
    onClickGood = (e: Event) => { this.info = e.message }
}
let h = new Handler();
uiElement.addClickListener(h.onClickGood);
```



### 重载

JavaScript 函数本身可以依据传入不同的参数返回不同类型的数据

TypeScript 可以为同一个函数提供多个函数类型定义来进行函数重载，编译器会依据列表去处理函数的调用

```ts
function pickCard(x: {suit: string; card: number; }[]): number;
function pickCard(x: number): {suit: string; card: number; };
function pickCard(x): any {
  ...
}
// 由于匹配时是按照顺序，因此要把最精确的定义放最前面
// 其中，function xx(x): any 不属于重载列表的一部分
```



## 泛型

 泛型可以用来创建可重用的支持多种类型数据的组件

实现上述功能本可以使用`any`类型来定义函数，但这样做的缺陷是容易丢失部分信息：传入的类型与返回的类型应该是相同的，如果传入数字，可能任何类型的值都有可能被返回

```ts
function identity(arg: any): any {
  return arg
}
```

为避免此类问题，可以使用**类型变量**

### 类型变量

这是一种只表示类型而不是值的特殊变量

```ts
function identity<T>(arg: T): T {
    return arg;
}
// 类型变量 T 帮助捕获用户传入的类型，然后再次使用 T 当做返回值类型，这样参数和返回值的类型就是相同的
```

定义方法：

+ 传入所有参数，包含类型参数

  ```ts
  let output = identity<string>('myString')
  ```

+ 利用**类型推论**，编译器会根据传入的参数自动确定 T 的类型

  ```ts
  let output = identity('myString')
  // 当不使用尖括号<>来明确传入类型时，编译器会查看 `myString` 的值，然后把 T 设置为它的类型。
  //类型推论可以保持代码精简和高可读性
  ```



在使用泛型函数时，编译器会要求在函数体内必须正确使用这个通用的类型

```ts
function loggingIdentity<T>(arg: T): T {
    console.log(arg.length);  // Error: T doesn't have .length
    return arg;
}
//由于没有地方指明 arg 的具体类型，他代表的是任意类型，则不一定有 length 属性
//如果操作 T 类型的数组则可行
function loggingIdentity<T>(arg: T[]): T[] {
    console.log(arg.length);  // Array has a .length, so no more error
    return arg;
}
function loggingIdentity<T>(arg: Array<T>): Array<T> {
    console.log(arg.length);  // Array has a .length, so no more error
    return arg;
}

let myIdentity: <T>(arg: T) => T = loggingIdentity
```



### 泛型接口

```ts
interface GenericIdentityFn {
  <T>(arg: T): T;
}
function identity<T>(arg: T): T {
    return arg;
}
let myIdentity: GenericIdentityFn = identity
//也可以将泛型参数作为接口，这样就能在调用时候知道具体是什么泛型类型，这样接口里的成员就能知道参数的类型
interface GenericIdentityFn<T> {
    (arg: T): T;
}

function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;
```



### 泛型类

```ts
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```

而类是有静态部分和实例部分，泛型类指的是实例部分的类型，静态属性是不能使用泛型类型的



### 泛型约束

有时候会需要限制函数去处理带有某些属性（比如`.length`）的所有类型，只要传入的类型有这个属性，就允许。因此需要列出对 T的约束要求

可以定义一个接口来描述约束条件，比如一个包含`.length`属性的接口，使用这个接口和`extends`关键字来实现约束

```ts
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);  // Now we know it has a .length property, so no more error
    return arg;
}
//loggingIdentity(3);  // Error, number doesn't have a .length property
```



### 在泛型约束中使用类型参数

可以声明类型参数被另一个类型参数所约束。比如想要用属性名从对象里获取这个属性。 并且我们想要确保这个属性存在于对象 `obj`上，因此我们需要在这两个类型之间使用约束。

```ts
function getProperty(obj: T, key: K) {
    return obj[key];
}

let x = { a: 1, b: 2, c: 3, d: 4 };

getProperty(x, "a"); // okay
getProperty(x, "m"); // error: Argument of type 'm' isn't assignable to 'a' | 'b' | 'c' | 'd'.
```

