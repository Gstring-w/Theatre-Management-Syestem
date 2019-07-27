# Theatre-Management-Syestem

此为 TS 练习项目，使用到 TS+React

# 开发顺序

1. 服务器端
   使用 postman 测试
2. 客户端开发

# 模块解析

1. 使用 classic （以前版本，基本很少）
2. 使用 node 解析策略（现在版本基本都使用）

> 在配置文件 tsconfig.js 中加入 `{moduleReslution : "node" }`

# 接口和类型兼容性

> 接口属于扩展类型的一种

# 接口：interface

## 扩展类型：

- 类型别名 （存在一定的问题，后文会讲到）
- 枚举
- 接口
- 类

TypeScript 中接口的定义：用于约束函数、类、对象的契约（标准）。

### 契约（标准）的形式：

- API 文档，弱标准，人为开发时可能出现错误
- 代码约束，强标准
-

### 接口代码的书写

> 和类型别名一样，不会出现在编译结果里面

1. 接口约束对象

```TypeScript

interface User {
    name:string
    age:number
}

let u:User{
    name:"ssss",
    age:12
}

```

> 和类型别名基本没什么区别，但是后文约束类，只能使用接口约束，而不能使用类型别名约束

2. 接口约束函数

```

interface User{
    sayHello :()=>void // 表示sayHello是一个函数，函数没有参数，函数返回void
    sayHello():void
}
let u:User{
    sayHello(){
        consoloe.log("hello")
    }
}


// 普通函数


function sum (n:number[],callback:(n:number)=>boolean){
    n.filter
}

// 将约束函数提取出来

// 类型别名
type Confition = (n:number)=>boolean

// 接口
interface Confition {
    (n:number):boolean
}
// 类型别名也可以使用这种书写形式
type Confition{
      (n:number):boolean
}
// {}表示一个定界符，不表示对象
```

### 接口可以继承

> 继承可以节约代码，可以表示一定逻辑语义

```
// React 中的继承
class App extends React.Component{

}
```

- 接口继承代码书写

```

interface A1{
    T1:sting
}
interface A2 extends A1 {
    T2:number
}

let b: A2{
    T1 = '123',
    T2 = 123
}


// 可以继承多个
interface A1{
    T1:sting
}
interface A2 {
    T2:number
}

interface A3 extends A1,A2{
    T3:boolean
}


// 类型别名表示组合

type A {
    T1:sting
}

type B {
    T2:sting
}

type  C{
    T3:sting
} & A & B
```

> 使用类型别名实现类型的组合效果，可以通过`&`（交叉类型）

### 接口和类型别名的差异

- 子接口中不能覆盖父接口的成员
- 交叉类型如果重写类型的成员，不会覆盖，将会合并该成员，导致没法赋值

**readyOnly**

只读修饰符，修饰的目标是只读的

## 类型兼容性

> B->A 如果能完成赋值，则 B 和 A 类型兼容

**使用鸭子辨型法（子结构辩型法）**

> 目标类型需要某一些特征，赋值的类型只有能满足该特征即可

- 基本类型：完全匹配
- 对象类型：鸭子辩型法
  > 当直接使用对象字面量的时候，会执行更加严格的匹配方式

```
interface Duck{
    sound:"嘎嘎嘎" // 这是自变量，而不是字符串值
    swin():void
}


let person = {
    name:'伪装成鸭子的人',
    age:11,
    sound:"嘎嘎嘎" as "嘎嘎嘎" // 类型断言
    swin(){
        console.log("swimining")
    }
}

let duck:Duck = person; // 可以完成赋值


// 假设有个函数，用于得到服务器的某个接口的返回结果，是一个用户对象

interface ResponseUser{
    loginId:string,
    nickName:string,
    gender:"男"|"女"
}
```

- 函数类型：一切无比自然

**函数参数**：传递给目标函数的参数可以少，但不可以多。<br/>
**返回值**: 返回值必须类型匹配，要求需要返回值，必须要返回值；不要求返回，你随意

```

interface Condititon{
    (n:number,i:number):boolean
}
function sum (n:number,callback:Condition){
    let s = 0;
    n.forEach((item,index)=>{
        if(callback(item,index)){
            s +=item;
        }
    })
    return s;
}
const result = sum([1,2,3,4,5], n => n % 2 !== 0); // 注：第二个参数中没有传入第二个参数i，此时ts不会报错
```

# TS 中的类

> 面向对象思想

**属性**：使用属性列表来描述属性

在 tsconfig.js 中添加一个`"strictPropertyInitialization":"true"` 添加一个更加严格的属性检查

```
class User {
    name ?:string   // 可以没有这个属性
    age:number = 18 // 添加一个默认值
    private gender:"男"|"女"  // 可选属性 私有属性
    readonly id:number = 12   // 只读属性
    constructor(name:string,age:number){
        this.name= name;
        this.age = age;
    }
}

```

## 访问器

> 控制属性的读取和赋值（js 本身在 ES6 版本存在的语法糖）

```
class Test{
   private _id:number
    constructor(){

    }
    get id(){
        return this._id
    }
    set id(value:number){
        this._id = value
    }
}
// 不要在get id中存在id赋值，会出现无限递归执行set和get

```

# 泛型

#### 泛型可以解决的问题

例：

```
// js代码
// 从arr数组中取回前n项
function take(arr,n){
    if(n >= arr.length) return arr;

    const newArr = [];
    for(let i = 0; i < n,i++){
        newArr.push(arr[i]);
    }
    return newArr;
}
const newArr = take( [ 1, 23, 4, 5], 2 );

// take函数中第一个参数arr，无法保证这个一个number数组，或者是一个string数组
// 不能预先知道arr到底是什么 就不得不写any[]
```

有时书写某个函数是，会丢失一些类型信息（多个位置的类型应该保持一致或有关联的信息）

泛型：是指附属于函数、类、接口、类型别名之上的类型

## 在函数中使用泛型

在函数名之后写上`<泛型名称>`

> 泛型相当于是一个类型变量，在定义时，无法预知具体的类型是，可以用该变量来代替，只有到调用时，才能确定它的类型；很多时候，TS 会更具传入的参数，推导出类型；如果没有完成推导，默认会是一个空对象

```
// 改写之前的代码
function take<T = number>(arr:T[],n:number) T[]{ // 设置默认值
    if(n >= arr.length) return arr;

    const newArr:T[] = [];
    for(let i = 0; i < n,i++){
        newArr.push(arr[i]);
    }
    return newArr;
}
const newArr = take<string>( [ "1", "23", "4", "5"], 2 );
```

## 如何在类型别名，接口，类中使用泛型

```
// 回调函数，判断数组中中某一项是否满足条件
type CallBack = (n:number,i:number) => boolean;


type CallBack<T> = (n:T,i:number) => boolean;

```

## 多泛型

```
// 此时Array类中的泛型都是 一开始new Aarry(arr)中arr数组决定
class Array<T,K>{

    constructor(public arr:T[]){
        this.arr = arr;
    }
    pop():T{
        return this.arr.pop();
    }
}


// 此时泛型T 为传入arr的数组
class Array{

    constructor(public arr:T[]){
        this.arr = arr;
    }
    pop():T{
        return this.arr.pop();
    }
}
```

# 深入类和接口

## 类的继承

### 继承的作用

1. 描述类与类之间的关系
   > 如果 A 和 B 都是类，如果可以描述为 A 是 B，则 A 和 B 可以是一种继承关系

- B 是父类， A 是子类
- B 派生 A， A 继承 B
- B 是 A 的基类，A 是 B 的派生类

2. 避免书写重复代码

```
// 用于抽象成员，才能成为抽象类
class Tank {
    abstruct readonly name:string
}

```

## 静态成员(`static`)

## 单例模式

```
class Board{
    width:number = 500;
    height:number = 500;
    private constructor{}
    private _borad?:Board;
    createBoard(){
        if(this._board){
            return this._board
        }else {
            this._board = new Board()
            return this._board
        }
    }
}
```

## 索引器

`对象[值]`就是一个索引器，以前叫做成员表达式

**在 TS 中,默认情况下，不对成员表达式作严格的类型检查**，因为不能确定表达式是什么

> 开启配置 `noImpicitAny：true` 开启对隐式 any 类型的检查

```JavaScript
const onj = {
    a:'12',
    b:12,
    "id-name":'123' // 不规范的命名 可以使用索引器读取
}

for(var key in obj){
    console.log(obj[key])
}
```

```
const methodName = "sayHello"
class User{


    // 限定了所有成员
    [prop:string]:any
    constructor(
        public name:string
    ){

    }
    // sayHello(){
    //
    // }
    [methodName](){} // 使用 const u = new User("aas"); u[methodName]();
}
```

使用索引器，对象可以任意添加任何属性。<br />
在索引器中，键的类型可以是数字和字符串

```
class MyArry {
    [prop: number]:any // 书写的位置，必须在所有成员的最顶端
    0 = 1
    1 = 334
    3 = "name"
}
```

TS 中索引器的作用：

1. 在严格的检查下，可以使用为类动态增加成员
2. 可以实现动态操作类成员

```

class Array {
    [props:number]:string
    [props:string]:string

    [props:number]:string
    [props:string]:number
}
// 可以使用2个索引器
a[0] = 'str';
a["str"] = "str"
```

TS 中，如果出现 2 个索引器，要求索引器的值类型必须匹配

在 JS 中所有的成员名，都是字符串，如果使用数字为成员名，会转化成字符串

```
const arr = [];
arr[0] = 1;
arr["0"] = 3;
console.log(arr[0])
```

## TS 中 this 指向约束

对象字面量中 this 是 any <br>
类中的 this 是用户对象

```
interface IUser{
    name:string
    age:number
    sayHello(this:IUser):void
}

const u:IUser{
    name:"str",
    age:12,
    sayHello(){
        console.log(this,this.name,this.age)
    }
}

const say = u.sayHello;
say() // 不予许



// 在类中约定this的指向，this不是真正的参数，不会出现在编译结果里
class User{
    name:sting = "sre"
    age:number = 12
    sayHello(this:User){
        console.log(this,this.name,this.age)
    }
}
```

# 装饰器

> 分离关注点

eg:属性验证场景

```
class User{
    loginId:string
    loginpwd:string
}
// 在书写属性时，验证规则；而不是写完之后再验证规则
```

- 关注点问题
  > 在定义某个属性时，最清楚该属性的情况
- 重复代码

### 类装饰器

装饰器的本质是函数，该函数接受一个参数，表示类（构造函数）本身

在 TS 如何约束一个变量为类

```
function test(target: new () => objcet){ // Function太宽泛
     console.log(target)
}
@test
```

装饰器函数的运行时间

在类定义时，直接运行

类装饰器可以具有的返回值

- void 仅运行函数
- 返回一个新的类，将新的类替换目标类

```

// 对构造函数中参数的处理
function test(target:new (...args:any[])=> object){

}
@test
class A {
    prop:string
    constructor(prop:string){

    }
}

@test("zhe")

 function test(str:string){
       return function (target:new (...args:any[])=> object){}
}
```

### 多个装饰器运行顺序（从下往上）

## 成员装饰器

- 属性
- 方法

### 属性装饰器

> 也是一个函数

该函数需要 2 个参数：

1.  如果是静态属性，是==类本身==；如果是实例属性，则为==类的原型==
2.  固定为一个字符串，表示属性名

```
function d(target:any,key:string){
    console.log(target,key)
}

class A{
    @d
    prop1:string

    @d
    static prop2:string
}
```

### 方法装饰器

> 也是一个函数

该函数需要 3 个参数：

1.  如果是静态方法，是==类本身==；如果是实例方法，则为==类的原型==
2.  固定为一个字符串，表示方法名
3.  属性描述对象(可以修改)

可以有多个装饰器

```
function d(){
    return function(target:any,key:string,dec:PrototypeDescriptor){
    console.log(target,key,dec)
}
}

class A{
   @d()
    method1(){

    }
}

```

## 声明文件

1.  什么是声明文件？

以`.d.ts`结尾的文件

2.  声明文件有什么作用？

为 JS 代码提供类型声明

3.  声明文件的位置

- 放置到 tsconfig.json 配置中包含的目录中`include:['./src']`
- 放置到 node_modules/@types 文件夹中
- 手动配置，在 tsconfig.json 中配置`typeRoots:[]`(权重较高，设置以后，上面 2 个配置均失效)
- 与 JS 代码所在目录相同，文件名也相同的文件 （最好的方式）用 TS 代码书写的工程发布之后的格式

### 编写声明文件 （可以使用打包工具，自动生成声明文件）

#### 自动生成

> - 工程是使用 TS 开发，书写而成的，发布（编译）之后，发布的是 JS 文件，如果发布的文件，需要其他开发者使用；可以使用声明文件来描述发布结果中的类型<br/>
> - 配置`tsconfig.json`中的`declaration:true`，为发布的结果自动配置声明文件

#### 手动编写

使用场景

1. 对已有的库，他是使用 JS 书写，并且更改该库代码为 TS 成本较高，可以手动编写声明文件；
2. 对第三方库，他们使用 JS 书写而成，并且这些第三方库没有提供声明文件，可以手动编写声明文件。

手动编写样例：
`global.d.ts`

````TypeScript
// 全局声明
declare var console: {
    log (message?: any):void
}
// 注：只能是声明，不能使用赋值


// namespace：表示命名空间，可以将其看成一个对象，访问是只能使用```.```来访问
declare namespace console{
    function log(message?:any):void
    function error(message?:any):void
}

declare function setTimeout(fun:()=>void,time:number):number

// 提取公共函数类型
type timeHandle = ()=>void
declare function setTimeout(fun:timeHandle,time:number):number




// 模块声明

// lodash模块

import _ from "lodash";
const newArr = _.chunk([1,2,3,7,4,5,6],2);

// lodash.d.ts 文件
declare module "lodash"{
    export function chunk<T>(a:T[],n:number): T[][]
}

````

#### 三斜线

```
/// <reference path ='../...d.ts>

```

### 发布

1. 当前工程使用 TS 开发
   > 直接打包，会根据配置，打包出声明文件，将编译结果所在的文件夹直接发布到 npm 上即可
2. 为其他第三方库开发的声明文件
   > 发布到@types/\*\*中

- 进入 github 开源项目： https://github.com/DefinitelyTyped/DefinitelyTyped
- fork 到自己的开源库
- 从自己的开源库中 clone 到本地
- 本地新建分支（例如：myloash4.3）在新分支中进行新分支文件的开发
- 在 types 目录中新建文件夹，在新的文件夹中开发声明，
- push 分支到自己的库中
- 新建一个 pull request
- 等待官方审核 （大概一天的时间）
- 审核通过之后，会发布到 npm， 之后使用命令`npm install @types/你发布的库名`
