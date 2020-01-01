---
title: 初识TypeScript
date: 2019-07-13 14:26:07
tags: typescript
categories: 前端
---
## 基础类型
### 布尔值
```ts
let isBoolean:boolean = false;
```
### 数字
```ts
let isNumber:number = 6;
```
### 字符串
```ts
let name:string = 'hello';
```
### 数组
TypeScript像JavaScript一样可以操作数组元素。 有两种方式可以定义数组。
```ts
//第一种，可以在元素类型后面接上 []，表示由此类型元素组成的一个数组：
let list:number[] = [1,2,3];
//第二种方式是使用数组泛型，Array<元素类型>：
let list:Array<number> = [1,2,3];
```
### 元组(Tuple)
元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为 string和number类型的元组。
```ts
let x: [string, number];
x = ['hello', 10]; // OK
x = [10, 'hello']; // Error
```
### 枚举
enum类型是对JavaScript标准数据类型的一个补充。 像C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。
```ts
enum Color {Red, Green, Blue};
let c:Color = Color[1]; //输出：Green
//默认情况下，从0开始为元素编号。 你也可以手动的指定成员的数值。 例如，我们将上面的例子改成从 1开始编号：
enum Color {Red = 1, Green, Blue}
let c:Color = Color[1]; //输出：Red
//或者，全部都采用手动赋值：
enum Color {Red = 1, Green = 2, Blue = 4}
let c:Color = Color[2]; //输出：Green
let p:Color = Color.Blue //输出：4 (下标)
```
### Any
有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 any类型来标记这些变量：
```ts
let notSure:any = 4;
notSure = "maybe a string instead";
notSure = false;
```
### Void
某种程度上来说，void类型像是与any类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 void:
```ts
function warnUser():void {
    console.log("This is my warning message");
}
//声明一个void类型的变量没有什么大用，因为你只能为它赋予undefined和null：
let unusable: void = undefined;
```
### Null 和 Undefined
TypeScript里，undefined和null两者各自有自己的类型分别叫做undefined和null。 和 void相似，它们的本身的类型用处不是很大：
```ts
let u: undefined = undefined;
let n: null = null;
```
### Never
never类型表示的是那些永不存在的值的类型。 例如， never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 never类型，当它们被永不为真的类型保护所约束时。
```ts
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```
### Object
object表示非原始类型，也就是除number，string，boolean，symbol，null或undefined之外的类型。
使用object类型，就可以更好的表示像Object.create这样的API。例如：
```ts
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```
### 类型断言
有时候你会遇到这样的情况，你会比TypeScript更了解某个值的详细信息。 通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。
通过类型断言这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 **类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。** TypeScript会假设你，程序员，已经进行了必须的检查。
```ts
//方法一： 尖括号
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;

//方法二： as语法
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;

```
### 关于let
你可能已经注意到了，我们使用**let关键字来代替大家所熟悉的JavaScript关键字var。 let关键字是JavaScript的一个新概念，TypeScript实现了它**。 我们会在以后详细介绍它，很多常见的问题都可以通过使用 let来解决，所以尽可能地使用let来代替var吧。

## 函数
### 函数定义类型
```ts
//方式1
function add(x:number,y:number):number{
  return x+y;
}

//方式2
let add = function(x:number,y:number):number{
  return x+y;
}
```
我们可以给每个参数添加类型之后再为函数本身添加返回值类型。 TypeScript能够根据返回语句自动推断出返回值类型，因此我们通常省略它。

### 书写完整函数类型
现在我们已经为函数指定了类型，下面让我们写出函数的完整类型。
```ts
let myAdd: (x: number, y: number) => number =
    function(x: number, y: number): number { return x + y; };
```
函数类型包含两部分：参数类型和返回值类型。 当写出完整函数类型的时候，这两部分都是需要的。 我们以参数列表的形式写出参数类型，为每个参数指定一个名字和类型。 这个名字只是为了增加可读性。 我们也可以这么写：
```ts
let myAdd: (baseValue: number, increment: number) => number =
    function(x: number, y: number): number { return x + y; };
```
只要参数类型是匹配的，那么就认为它是有效的函数类型，而不在乎参数名是否正确。
第二部分是返回值类型。 对于返回值，我们在函数和返回值类型之前使用( =>)符号，使之清晰明了。 如之前提到的，返回值类型是函数类型的必要部分，如果函数没有返回任何值，你也必须指定返回值类型为 void而不能留空。
函数的类型只是由参数类型和返回值组成的。 函数中使用的捕获变量不会体现在类型里。 实际上，这些变量是函数的隐藏状态并不是组成API的一部分。

### 推断类型
尝试这个例子的时候，你会发现如果你在赋值语句的一边指定了类型但是另一边没有类型的话，TypeScript编译器会自动识别出类型
```ts
// myAdd has the full function type
let myAdd = function(x: number, y: number): number { return x + y; };

// The parameters `x` and `y` have the type number
let myAdd: (baseValue: number, increment: number) => number =
    function(x, y) { return x + y; };

```
### 可选参数和默认参数
**TypeScript里的每个函数参数都是必须的**。 这不是指不能传递 null或undefined作为参数，而是说编译器检查用户是否为每个参数都传入了值。 编译器还会假设只有这些参数会被传递进函数。 简短地说，传递给一个函数的参数个数必须与函数期望的参数个数一致。
```ts
function buildName(firstName: string, lastName: string) {
    return firstName + " " + lastName;
}

let result1 = buildName("Bob");                  // error, too few parameters
let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result3 = buildName("Bob", "Adams");         // ah, just right
```

**可选参数**
使用 **?** 实现可选参数的功能。 比如，我们想让last name是可选的：
```ts
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}

let result1 = buildName("Bob");  // works correctly now
let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result3 = buildName("Bob", "Adams");  // ah, just right
```
可选参数必须跟在必须参数后面。 如果上例我们想让first name是可选的，那么就必须调整它们的位置，把first name放在后面。

 **默认初始化值的参数** 
 让我们修改上例，把last name的默认值设置为"Smith"。
```ts
function buildName(firstName: string, lastName = "Smith") {
    return firstName + " " + lastName;
}

let result1 = buildName("Bob");                  // works correctly now, returns "Bob Smith"
let result2 = buildName("Bob", undefined);       // still works, also returns "Bob Smith"
let result3 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result4 = buildName("Bob", "Adams");         // ah, just right
```
**在所有必须参数后面的带默认初始化的参数都是可选的，与可选参数一样，在调用函数的时候可以省略。也就是说可选参数与末尾的默认参数共享参数类型。**
```ts
function buildName(firstName: string, lastName?: string) {
    // ...
}
function buildName(firstName: string, lastName = "Smith") {
    // ...
}
```
共享同样的类型(firstName: string, lastName?: string) => string。 默认参数的默认值消失了，只保留了它是一个可选参数的信息。
与普通可选参数不同的是，带默认值的参数不需要放在必须参数的后面。 **如果带默认值的参数出现在必须参数前面，用户必须明确的传入 undefined值来获得默认值**。 例如，我们重写最后一个例子，让 firstName是带默认值的参数：
```ts
function buildName(firstName = "Will", lastName: string) {
    return firstName + " " + lastName;
}

let result1 = buildName("Bob");                  // error, too few parameters
let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result3 = buildName("Bob", "Adams");         // okay and returns "Bob Adams"
let result4 = buildName(undefined, "Adams");     // okay and returns "Will Adams"
```
### 剩余参数
必要参数，默认参数和可选参数有个共同点：它们表示某一个参数。 有时，你想同时操作多个参数，或者你并不知道会有多少参数传递进来。 在JavaScript里，你可以使用 arguments来访问所有传入的参数。
在TypeScript里，你可以把所有参数收集到一个变量里：
```ts
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}

let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
```
剩余参数会被当做个数不限的可选参数。 可以一个都没有，同样也可以有任意个。 编译器创建参数数组，名字是你在省略号 **...** 后面给定的名字，你可以在函数体内使用这个数组。
### this和箭头函数
**JavaScript里，this的值在函数被调用的时候才会指定。**
下面的函数，独立调用myName函数，因为谁调this就指向谁，所以它的this指向了window。
```ts
let people = {
    name:['hello','world','if','for'],
    getName:function(){
        return function(){
            let i = Math.floor(Math.random()*4);
            return {
                n:this.name[i]
            }
        }
    }
};
let myName = people.getName();
console.log('name: '+myName().n);
//输出： name：undefined
```
**箭头函数能保存函数创建时的 this值，而不是调用时的值：**
```ts
let people = {
    name:['hello','world','if','for'],
    getName:function(){
        return ()=>{
            let i = Math.floor(Math.random()*4);
            return {
                n:this.name[i]
            }
        }
    }
};
let myName = people.getName();
console.log('name: '+myName().n);
//this指向 people下的name
```
### 函数重载
JavaScript本身是个动态语言。 JavaScript里函数根据传入不同的参数而返回不同类型的数据是很常见的。
**之前实现重载的方式：**
```ts
function attr(nameorage){
    if(nameorage && typeof nameorage === 'string'){
        console.log('name');
    }else{
        console.log('age');
    }
}
```
**ts实现重载的方法**
```ts
function attr(name:string):string;
function attr(age:number):number;
function attr(nameorage:any):any{
    if(nameorage && typeof nameorage === 'string'){
        console.log('name');
    }else{
        console.log('age');
    }
}
```

## 类
类的简单例子
```ts
class Greeter{
    greeting: string;
    constructor(message:string) {
        this.greeting = message;
    }
    greet(){
        return "Hello, " + this.greeting;
    }
}
let greeter = new Greeter("world");
```

### 继承
在TypeScript里，我们可以使用常用的面向对象模式。 基于类的程序设计中一种最基本的模式是允许使用继承来扩展现有的类。
```ts
class Animal {
    move(distanceInMeters: number = 0) {
        console.log(`Animal moved ${distanceInMeters}m.`);
    }
}

class Dog extends Animal {
    bark() {
        console.log('Woof! Woof!');
    }
}

const dog = new Dog();
dog.bark();
dog.move(10);
dog.bark();
```
这个例子展示了最基本的继承：类从基类中继承了属性和方法。 这里， Dog是一个 派生类，它派生自 Animal 基类，通过 extends关键字。 派生类通常被称作 子类，基类通常被称作 超类。

因为 Dog继承了 Animal的功能，因此我们可以创建一个 Dog的实例，它能够 bark()和 move()。

下面我们来看个更加复杂的例子:
```ts
class Animal {
    name: string;
    constructor(theName: string) { this.name = theName; }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

class Snake extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters);
    }
}

class Horse extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 45) {
        console.log("Galloping...");
        super.move(distanceInMeters);
    }
}

let sam = new Snake("Sammy the Python");
let tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);
```
这个例子展示了一些上面没有提到的特性。 这一次，我们使用 extends关键字创建了 Animal的两个子类： Horse和 Snake。

与前一个例子的不同点是，派生类包含了一个构造函数，它 必须调用 super()，它会执行基类的构造函数。 而且，在构造函数里访问 this的属性之前，我们 一定要调用 super()。 这个是TypeScript强制执行的一条重要规则。

这个例子演示了如何在子类里可以重写父类的方法。 Snake类和 Horse类都创建了 move方法，它们重写了从 Animal继承来的 move方法，使得 move方法根据不同的类而具有不同的功能。 注意，即使 tom被声明为 Animal类型，但因为它的值是 Horse，调用 tom.move(34)时，它会调用 Horse里重写的方法：

```console
Slithering...
Sammy the Python moved 5m.
Galloping...
Tommy the Palomino moved 34m.
```

### 公共，私有与受保护的修饰符
**默认为public**
也可以明确的将一个成员标记成 public。 我们可以用下面的方式来重写 Animal类：
```ts
class Animal {
    public name: string;
    public constructor(theName: string) { this.name = theName; }
    public move(distanceInMeters: number) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}
```

### 私有修饰符 private
当成员被标记成 private时，**它就不能在声明它的类的外部访问**。比如：
```ts
class Animal {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}

new Animal("Cat").name; // 错误: 'name' 是私有的.
```
### protectd
protected修饰符与 private修饰符的行为很相似，但有一点不同， protected成员在派生类中仍然可以访问。**protected修饰的只能在它的Person类和它的派生类中访问**例如
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
```
### 静态方法
不用 new 就可以用类名直接访问,它的派生类也可以直接访问。
```ts
class People{
    static print(){
        console.log('hello')
    }
}
class Student extends People{

}
People.print();
Student.print();
//输出
// hello
// hello
```

### readonly修饰符
可以使用**readonly**关键字将属性设置为只读。**只读属性必须在声明时或者构造函数里被初始化。**
```ts
class People {
  readonly name: string;
  constructor(thename: string) {
    this.name = thename;
  }
}

let s = new People('kk');
s.name = '123'; //错误！ name 是只读的
```

### 存取器（getters,setters）
Typescript支持同构getters/setters来截取对对象成员的访问，它能帮助你有效的控制对对象成员的访问。
```ts
class People {
  private _name: string;

  get name(): string {
    return this._name;
  }

  set name(newName: string) {
    if (!newName) {
      console.error('NewName is undefined!');
    } else {
      this._name = newName;
    }
  }
}
var s = new People();
console.log(s.name); // 输出: undefined
s.name = '123';
console.log(s.name); // 输出：123
s.name = '';
console.log(s.name); // 输出报错： NewName is undefined

```
### 抽象类
抽象类作为其他派生类的基类使用。他们一般不会直接被实例化。不同于接口，抽象类可以包含成员的实现细节。**abstract** 关键字是用于定义抽象类和在抽象类在抽象类内部定义抽象方法。
```ts
abstract class People{
  constructor(public name: string) {} 
  abstract print():void;// 必须在派生类中实现
}


class Student extends People{
  constructor(){
      super('zs'); // 在派生类的构造函数中必须调用 super()
  }
  print():string{ 
      return 'student'
  }
}
```

## 接口（interfaces）
在TypeScript文档中知道了，**TypeScript的核心原则之一是对值所具有的结构进行类型检查**，它有时被称做“鸭式辩型法”或“结构性子类型化”。在TypeScript里，**接口的作用就是为这些类型命名和为你的代码或者第三方代码定义契约。**

```ts
interface LabelledValue {
  label: string;
}

function printLabel(labelledObj: LabelledValue) {
  console.log(labelledObj.label);
}

let myObj = { size: 10, label: 'hello' };
printLabel(myObj);
```
### 可选属性
接口里的属性不全都是必须的。有些是只在某些条件下存在，或者根本不存在。可选属性在应用“option bags”模式时很常用，即给函数传入的参数对象中只有部分属性赋值，
```ts
interface SquareConfig{
    color?:string;
    width?:number;
}
```
带有可选属性的接口于普通的接口定义差不多，只是在可选属性名字定义的后面加一个 **?** 符号。

### 只读属性
**一些对象属性只能在对象刚刚创建的时候修改其值**。你可以在属性名前用 **readonly** 来指定只读属性：
```ts
interface Point{
    readonly x:number;
    readonly x:number;
}

// 赋值后， x和y再也不能被改变了。
let p1:Point = {x:10,y:20};
p1.x = 5; // error
```
TypeScript具有 **ReadonlyArray<T>** 类型，它于 **Array<T>** 相识，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能修改：
```ts
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; //error
ro.push(5); //error
ro.length = 100; //error
a = ro; //error
```
上面代码最后一行，可以看到就算把整个**ReadonlyArray**赋值到一个普通数组也是不可以的。但是可以用类型断言重写：
```ts
a = ro as number[];
```

### readonly vs const
最简单的判断该用 readonly 还是 const 的方法是看要把它做为变量使用还是做为一个属性。作为变量使用的话用 const ，若做为属性则使用 readonly。

### 额外的属性检查
```ts
interface SquareConfig {
  color?: string;
  width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
  // ...
  return config;
}

let mySquare = createSquare({ colour: "red", width: 100 });
```
注意传入 createSquare 的参数拼写为 colour 而不是color。在javaScript里，这会默默地失败。
因为TypeScript会认为这段代码可能存在bug。对象字面量会被特殊对待而且会经过 **额外属性检查**，当将它们赋值给变量或者作为参数传递的时候。如果一个对象字面量存在“目标类型”不包含的属性时，你会得到一个错误。
```ts
// error: 'colour' not expected in type 'SquareConfig'
let mySquare = createSquare({ colour: "red", width: 100 });
```
1. **绕开这些检查非常简单。最简便的方法是使用类型断言：**
```ts
let mySquare = createSquare({ colour: "red", width: 100 } as SquareConfig);
```
2. 最佳的方式是能够添加一个字符串索引签名，前提是你能够确定这个对象可能具有默写作为特殊用途使用的额外使用。如果 SquareConfig 带有上面定义的类型的 color 和 width属性，并且还会带有任意数量的其他属性，我们可以这样定义它：
```ts
interface SquareConfig{
    color?: String;
    width?: number;
    [propName: string]:any;
}
```
3. 直接将这个对象赋值给另一个变量，下面代码的 squareOptions 不会经过额外属性检查，所以编译器不会报错。
```ts
let squareOptions = {colour: 'red', width: 100};
let mySquare = createSquare(squareOptions);
```

### 函数类型
接口能够描述JavaScript中对象拥有的各种各样的外形。除了描述带有属性得到普通对象外，接口也可以描述函数类型。
```ts
interface SearchFunc{
    (sourece: string, subString: string):boolean;
}
```
上面这样定义后，我们可以使用其他接口一样使用这个函数类型的接口。下例展示来如何创建一个函数类型的变量，并将一个筒类型的函数赋值给这个变量。
```ts
let mySearch: SearchFunc;
mySearch = function(source:string, subString:string){
    let result = source.search(subString);
    return resule > -1;
}
```
**对于函数类型的检查类型来说，函数的参数名不需要于接口里定义的名字相匹配。**比如，我们下面使用的代码重写上面的例子：
```ts
let mySearch: SearchFunc;
mySearch = function(src: string, sub:string): boolean{
    let result = src.search(sub);
    return result > -1;
}
```
函数的参数会逐个进行检查，要求对于位置上的参数类型是兼容的。如果你不想指定类型，TypeScript的类型系统会推断参数类型，因为函数直接复制给了 SearchFunc 类型变量。函数的返回值类型是通过其返回值推断出来的（此例 false 和 true)。如果让这个函数返回的梳子或者字符串，类型检查器会警告我们的函数返回值类型与 SearchFunc 接口中的定义不匹配。
```ts
let mySearch: SearchFunc;
mySearch = function(src,sub){
    let result = src.search(sub);
    return result > -1;
}
```

### 可索引的类型
与使用接口描述函数差不多，我也可以描述那些能够“通过索引得到”的类型，比如a[10]或ageMap['daniel']。可索引类型具有一个索引签名，它描述来对象索引的类型，还有相应的索引返回值类型。且看下面例子：
```ts
interface StringArray{
    [index: number]: string;
}

let myArray: StringArray;
myArray = ['Bob', 'Fred'];
let myStr: string = myArray[0];
```
上面例子，我们定义了 StringArray 接口，它具有索引签名。这个索引签名表示了当用 number 去索引 StringArray 时会得到 string 类型返回值。

TypeScript支持两种索引签名：字符串和数字。可以同时使用两种类型的索引，**但是数字索引的返回值必须是字符串索引返回值类型的子类型。**因为当使用 number 来索引是，JavaScript 会将它转换成 string 然后再去索引对象。也就是说用 100（一个number）去索引等同于使用 "100" （一个string）去索引，因此两者需要保持一致。
```ts
class Animal{
    name: string;
}
class Dog extends Animal{
    breed: string;
}
// 错误：使用数值型的字符串索引，有时会得到完全不同的Animal
interface NotOkay{
    [x: number]:Animal;
    [x: string]:Dog;
}
```
字符串索引签名能够很好的描述 **dictionary** 模式，并且它们也会确保所有属性与其返回值类型相匹配。因为字符串索引声明来 obj.property 和 obj['property']两种形式都可以。下面例子里，name 的类型与字符串索引类型不匹配，所以类型检查器给出一个错误提示：
```ts
interface NumberDictionary{
    [index: string]:number;
    lenght: number //可以，lenght是number类型
    name: string //错误，name的类型与索引类型返回值的类型不匹配
}
```
最后，你可以将索引签名设置为只读，这样就防止了给索引赋值：
```ts
interface ReadonlyStringArray{
    readonly [index:number]:string;
}

let myArray: ReadonlyStringArray = ['Alice','Bob'];
myArray[2] = 'Mallory'; //error
```
你不能设置 myArray[2],因为索引签名是只读的。

### 类类型
```ts
interface ClockInterface{
    currentTime: Date;
}
class Clock implements ClockInterface{
    currentTime: Date;
    constructor(h: number,m: number){}
}
```

### 继承接口
和类一样，接口也可以相互继承。
```ts
interface Shape{
    color: string;
}

interface Square extends Shape{
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
```
一个接口可以继承多个接口:
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
当接口继承了一个类类型时，它会继承类的成员但不包括其实现。
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

class Location {

}
```
在上面的例子里，SelectableControl包含了Control的所有成员，包括私有成员state。 因为 state是私有成员，所以只能够是Control的子类们才能实现SelectableControl接口。 因为只有 Control的子类才能够拥有一个声明于Control的私有成员state，这对私有成员的兼容性是必需的。

在Control类内部，是允许通过SelectableControl的实例来访问私有成员state的。 实际上， SelectableControl接口和拥有select方法的Control类是一样的。 Button和TextBox类是SelectableControl的子类（因为它们都继承自Control并有select方法），但Image和Location类并不是这样的。

## 泛型
在软件工程中，我们不仅要创建一致的定义良好的API，同时也要考虑可重用性。组件不仅能够支持当前的数据类型，同时也能支持未来数据类型，这在创建大型系统时为你提供了一个十分灵活的功能。

下面来创建第一个使用泛型的例子：identity函数。 这个函数会返回任何传入它的值。 你可以把这个函数当成是 echo命令。

不用泛型的话，这个函数可能是下面这样：
```ts
function identity(arg: number): number {
    return arg;
}
// 或者下面这样
function identity(arg: any): any {
    return arg;
}
```
使用any类型会导致这个函数可以接收任何类型的arg参数，这样就丢失了一些信息：传入的类型与返回的类型应该是相同的。如果我们传入一个数字，我们只知道任何类型的值都有可能被返回。

因此，我们需要一种方法使返回值的类型与传入参数的类型是相同的。 这里，我们使用了 类型变量，它是一种特殊的变量，只用于表示类型而不是值。
```ts
function identity<T>(arg: T): T {
    return arg;
}
```
我们给identity添加了类型变量T。 T帮助我们捕获用户传入的类型（比如：number），之后我们就可以使用这个类型。 之后我们再次使用了 T当做返回值类型。现在我们可以知道参数类型与返回值类型是相同的了。 这允许我们跟踪函数里使用的类型的信息。
```ts
//第一种是，传入所有的参数，包含类型参数
//这里我们明确的指定了T是string类型，并做为一个参数传给函数，使用了<>括起来而不是()。
let output = identity<string>("myString");  // type of output will be 'string'

//第二种。利用了类型推论 -- 即编译器会根据传入的参数自动地帮助我们确定T的类型：
let output = identity("myString");  // type of output will be 'string

```
### 使用泛型变量
如果我们想同时打印出arg的长度。 我们很可能会这样做：
```ts
function loggingIdentity<T>(arg: T): T {
    console.log(arg.length);  // Error: T doesn't have .length
    return arg;
}
```
如果这么做，编译器会报错说我们使用了arg的.length属性，但是没有地方指明arg具有这个属性。 记住，这些类型变量代表的是任意类型，所以使用这个函数的人可能传入的是个数字，而数字是没有 .length属性的。

现在假设我们想操作T类型的数组而不直接是T。由于我们操作的是数组，所以.length属性是应该存在的。 我们可以像创建其它数组一样创建这个数组：
```ts
function loggingIdentity<T>(arg: T[]): T[] {
    console.log(arg.length);  // Array has a .length, so no more error
    return arg;
}
```
你可以这样理解loggingIdentity的类型：泛型函数loggingIdentity，接收类型参数T和参数arg，它是个元素类型是T的数组，并返回元素类型是T的数组。 如果我们传入数字数组，将返回一个数字数组，因为此时 T的的类型为number。 这可以让我们把泛型变量T当做类型的一部分使用，而不是整个类型，增加了灵活性。
我们也可以这样实现上面的例子:
```ts
function loggingIdentity<T>(arg: Array<T>): Array<T> {
    console.log(arg.length);  // Array has a .length, so no more error
    return arg;
}
```
### 泛型类型
泛型类型的实际使用：
方式一：
```ts
function Hello<T>(arg:T):T{
    return arg;
}
var myHello:<K>(arg:K)=>K = Hello;
console.log(myHello('hello'))
```
方式二：
```ts
function Hello<T>(arg:T):T{
    return arg;
}
var myHello:{<T>(arg:T):T} = Hello;
console.log(myHello('hello'))
```
方式三：
```ts
interface Hello{
    <T>(arg:T):T;
}
function myHello<T>(arg:T):T{
    return arg;
}
var MH:Hello = myHello;
console.log(MH('hello'));
```
方式四：
```ts
interface Hello<T>{
    (arg:T):T;
}
function myHello<T>(arg:T):T{
    return arg;
}
var MH:Hello<string> = myHello;
console.log(MH('hello'));
```

### 泛型类
代码中使用：
```ts
class HelloNumber<T>{
  Ten: T;
  add: (x: T, y: T) => T;
}

var myHelloNumber = new HelloNumber<number>();
myHelloNumber.Ten = 10;
myHelloNumber.add = function (x, y) {
  return x + y;
}
console.log(myHelloNumber.Ten);
console.log(myHelloNumber.add(5,3));
```

## TS模块

### 泛型类型
```ts
module Validation{
  export interface StringValidator{
    isAcceptable(s: string):boolean;
  }
  var lettersRegexp = /^[A-Za-z]+$]/;
  var numberRegexp = /^[0-9]+$]/;
  export class LettersOnluValidator implements StringValidator{
    isAcceptable(s:string){
      return lettersRegexp.test(s);
    }
  }
  export class ZipCodeValidator implements StringValidator{
    isAcceptable(s:string){
      return s.length === 5 && numberRegexp.test(s);
    }
  }
}
```

## 命名空间
未使用命名空间：
```ts
interface StringValidator {
    isAcceptable(s: string): boolean;
}

let lettersRegexp = /^[A-Za-z]+$/;
let numberRegexp = /^[0-9]+$/;

class LettersOnlyValidator implements StringValidator {
    isAcceptable(s: string) {
        return lettersRegexp.test(s);
    }
}

class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {
        return s.length === 5 && numberRegexp.test(s);
    }
}

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: StringValidator; } = {};
validators["ZIP code"] = new ZipCodeValidator();
validators["Letters only"] = new LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        let isMatch = validators[name].isAcceptable(s);
        console.log(`'${ s }' ${ isMatch ? "matches" : "does not match" } '${ name }'.`);
    }
}
```

使用命名空间：
```ts
namespace Validation {
    export interface StringValidator {
        isAcceptable(s: string): boolean;
    }

    const lettersRegexp = /^[A-Za-z]+$/;
    const numberRegexp = /^[0-9]+$/;

    export class LettersOnlyValidator implements StringValidator {
        isAcceptable(s: string) {
            return lettersRegexp.test(s);
        }
    }

    export class ZipCodeValidator implements StringValidator {
        isAcceptable(s: string) {
            return s.length === 5 && numberRegexp.test(s);
        }
    }
}

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: Validation.StringValidator; } = {};
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        console.log(`"${ s }" - ${ validators[name].isAcceptable(s) ? "matches" : "does not match" } ${ name }`);
    }
}
```

## 装饰器
装饰器是一种特殊类型的声明，它能够被附加到 **类声明，方法，访问符，属性或参数上**，装饰器使用@expression 这种形式，expression求值后必须为一个函数，它会在运行的时候被调用，被装饰的声明信息作为参数传入。
编译装饰器的方法：
1. 命令行：
```cmd
tsc --target ES5 --experimentalDecorators
```
2. tsconfig.json
```json
{
    "compilerOptions":{
        "target": "ES5",
        "experimentalDecorators": true
    }
}
```

### 装饰器工厂
如果我们要定制一个修饰器如何应用到一个声明上，我们得写一个装饰器工厂函数。装饰器工厂就是一个简单的函数，它返回一个表达式，以供装饰器在运行时调用。
装饰器工厂函数:
```ts
function color(value: string){  //这是一个装饰器工厂
    return function(target){    //这是装饰器

    }
}
```

## Mixins
除了传统的面向对象继承方式，还流行一种通过可重用组件创建类的方式，就是联合另一个简单类的代码。

### 混入示例
```ts

class Disposable {
  isDisposed: boolean;
  dispose() {
    this.isDisposed = true;
  }
}

class Activatable {
  isActive: boolean;
  activate() {
    this.isActive = true;
  }
  deactivate() {
    this.isActive = false;
  }
}
//首先应该注意到的是，没使用extends而是使用implements。 把类当成了接口，仅使用Disposable和Activatable的类型而非其实现。 这意味着我们需要在类里面实现接口。 但是这是我们在用mixin时想避免的。
class SmartObject implements Disposable, Activatable {
  constructor() {
    setInterval(() => {
      console.log(this.isActive + ":" + this.isDisposed)
    }, 500);
  }

  interact() {
    this.activate();
  }
  //我们可以这么做来达到目的，为将要mixin进来的属性方法创建出占位属性。 这告诉编译器这些成员在运行时是可用的。 这样就能使用mixin带来的便利，虽说需要提前定义一些占位属性。
  isDisposed: boolean = false;
  dispose: () => void;
  isActive: boolean = false;
  activate: () => void;
  deactivate: () => void;
}

applyMixins(SmartObject, [Disposable, Activatable]);

let smartObj = new SmartObject();
setTimeout(() => smartObj.interact(), 1000);

function applyMixins(derivedCtor: any, baseCtors: any[]) {
  baseCtors.forEach(baseCtor => {
    Object.getOwnPropertyNames(baseCtor.prototype).forEach(name => {
      derivedCtor.prototype[name] = baseCtor.prototype[name];
    });
  });
}
```
其中applyMixins方法，帮我们进行混入操作。它会遍历mixins上的所有属性，并复制到目标上去，把之前的占位属性替换成真正的代码。

## 三斜线指令
三斜线指令是包含单个XML标签的单行注释。注释的内容会作为编译指令使用。

三斜线指令仅可放在包含它的**文件最顶端**。一个三斜线指令前面只能出现单行注释和多行注释，这包括其他的三斜线指令。如果它们出现在一个语句或声明之后，那么它们会被当成普通的单行注释，并不具有特殊的含义。
```ts
/// <reference path="...">
```

它用于声明文件间的 **依赖**。三斜线引用告诉编译器在编译过程中要引入的额外的文件。

