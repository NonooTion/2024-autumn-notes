# TypeScript

TypeScript是JavaScript的超集，提供了JS的所有功能，并且额外的增加了类型系统

与JS相比，TS会检查变量的类型是否发生变化，可以显示标记出代码中的意外行为，从而降低了发生错误的可能性



# TypeScript常用类型

## 类型注解

类型注解为变量添加类型约束，约定了什么类型，就只能给变量赋值该类型的值，否则，就会报错

```ts
var age : number =10
age="100"
//将number类型的变量赋值字符串
//报错
//不能将类型“string”分配给类型“number”。
```

## 常用基础类型概述

### JS已有类型

原始类型：

- number
- string
- boolean
- null
- undefined
- symbol

对象类型：

- object 包括数组，对象，函数...

```ts
var num:number =100
console.log("js已有类型: 数字类型 "+num)

var str:string ="100"
console.log("js已有类型: 字符串类型 "+str)

var isTrue:boolean =true
console.log("js已有类型: 布尔类型 "+isTrue)

var isNull:null =null
console.log("js已有类型: null类型 "+isNull)

var unDefined:undefined =undefined
console.log("js已有类型: undefined类型"+unDefined)

var syb:symbol =Symbol("这是一个symbol变量,表示独一无二的值")
console.log("js已有类型: symbol类型 "+syb.toString())

var nums:Array<number>=[1,2,3,4,5]
console.log("js已有类型: 数组类型 "+nums)

type person={
    name:string
    age:number
}

var p:person={name:"王刚",age:100}
console.log("js已有类型: 对象类型 "+p.name+" "+p.age)

var func = ()=>{
    console.log("js已有类型: 函数类型")
}

func()
```



#### 数组类型

在TS中，数组类型有两种写法

```ts
// 声明数组的两种方式
let num : number[]=[1,2,3]
let str :Array<string>=["1","2","3"]

//如果一个数组中含有多种类型的元素,使用联合类型进行声明
let num_and_str: (number|string)[]=[1,2,"3","4"]
let nums_strs: Array<(number|string)>=[1,2,"3","4"]
```

#### 类型别名

```ts
//使用type可以给类型起别名
type num_and_str = (number|string)
let data: Array<num_and_str> = [1,2,3,"4"] 
```



#### 函数类型

函数类型可以单独指定参数和返回值，也可以同时指定参数和返回值

```ts
//单独指定函数参数、返回值类型
function add1(num1:number,num2:number) :number{
    return num1+num2
}

const add2=(num1:number,num2:number) :number=>{
    return num1+num2
}

//同时指定参数、返回值类型
const add3:(num1:number,num2:number)=>number=(num1,num2)=>{
    return num1+num2
}
// 当函数作为表达式时，可以通过类似箭头函数形式的语法来为函数添加类型

//当函数没有返回值时，返回值类型为void
const hello=():void=> {console.log("hello")}
```

当函数没有返回值时，返回值类型为void

```ts
//当函数没有返回值时，返回值类型为void
const hello=():void=> {console.log("hello")}
```

函数参数可以传，也可以不传，可以使用可选参数

```ts
//可选参数，可选参数后面不能出现必选参数
function mySlice(start?:number,end?:number):void{
    console.log("起始索引:",start,"结束索引:",end)
}
mySlice()
mySlice(1)
mySlice(1,2)

//执行结果
起始索引: undefined 结束索引: undefined
起始索引: 1 结束索引: undefined
起始索引: 1 结束索引: 2
```



### TS新增类型

#### 元组

另一种类型的数组，可以确切的知道包含多少元素

```ts
let position:[number,number]=[10.2,3.5]
```

#### 字面量类型

```ts
const str="Hello TS"
```

因为str是一个常量，str的类型为"Hello TS"

除字符串来说，任意的对象，数字...都可以作为类型使用，叫做JS字面量

**字面量可以和联合类型一起使用**

```ty
function move(direction:('up'|'down'|'left'|'right')):void
{
    console.log("move "+direction)
}

move("up")
```

字面量配合联合类型一起使用，用于表示一组明确的可选值列表

#### 枚举类型

枚举的功能类似于字面量类型+联合类型组合的功能，也可以表示一组明确的可选值

```ts
enum direction {"up","down","left","right"}
function move(direction:direction):void
{
    console.log("move "+direction)
}

move(direction.up)
```

通过"."来访问枚举的成员

枚举成员的值默认是从0开始

上面的代码中 up=0,down=1,...

可以修改值

```ts
enum direction {"up"=4,"down"=7,"left"=9,"right"=12}
function move(direction:direction):void
{
    console.log("move "+direction)
}

move(direction.up)
```

可以将值修改为字符串，必须全部赋值

```ts
enum direction {"up"="up","down"="down","left"="left","right"="right"}
function move(direction:direction):void
{
    console.log("move "+direction)
}

move(direction.up)
```

#### any

any类型的值可以进行任意操作



# TypeSciprt高级类型

## class类

```ts
class Person {
    //实例变量
    age:number
    gender:string
    // 构造函数
    constructor(age:number ,gender:string){
        this.age=age
        this.gender=gender
    }
    //实例方法
    toString():void{
        console.log("age:",this.age,"gender:",this.gender)
    }
}

let p:Person=new Person(18,"沃尔玛购物袋")

p.toString()

```



```ts
//定义接口
interface Speakable{
    speak():void
}
//extends继承父类，implements 实现接口
//s
class Student extends Person implements Speakable{
    name:string
    speak(): void {
        console.log(this.name+"is speaking")
    }
 //super 调用父类方法
    constructor(name:string,age:number,gender:string){
        super(age,gender)
        this.name=name
    }
    study(){
        console.log("name:",this.name,"is studying")
    }
}
let p:Person=new Person(18,"沃尔玛购物袋")

p.toString()

let s:Student=new Student("陈木华",18,"男宝")
s.toString()
s.study()
s.speak()
```

## 类型兼容性

TS采用结构化类型系统，类型检查关注的是值所具有的形状

两个对象具有相同的"形状"，则认为它们属于同一类型

```ts
class Print{
    x:number
    y:number
    constructor(x:number,y:number){this.x=x,this.y=y}
}
class Print2{
    x:number
    y:number
    constructor(x:number,y:number){this.x=x,this.y=y}
}

const p:Print=new Print2(1,2)

console.log(p.x,p.y)
```



### 对象之间的兼容性

对于对象类型来说，y的成员至少与x相同，则x兼容y(成员多的兼容成员少的)

```ts
class Print2D{
    x:number
    y:number
    constructor(x:number,y:number){this.x=x,this.y=y}
}
class Print3D{
    x:number
    y:number
    z:number
    constructor(x:number,y:number,z:number){this.x=x,this.y=y,this.z=z}
}

const p:Print2D=new Print3D(1,2,3)

console.log(p.x,p.y)
```



### 函数之间的兼容性

参数个数：参数多的兼容参数少的

参数类型：相同同位置的参数类型要相同或兼容

返回值类型：返回值类型相同或兼容

```ts
type F1=(a:number)=> void
type F2=(a:number,b:number)=> void

let f1:F1=(a:number)=>{} 
let f2:F2

```



## 交叉类型

交叉类型(&)：功能类似于接口继承，用于这多个类型为一个类型

```ts
interface Person {name:string}
interface Contact{phone:string}

//Person和Contact联合类型
type PersonDetail=Person & Contact 
let obj: PersonDetail={
    name: 'jack',
    phone: '133...'
}
```



## 泛型

在保证类型安全的前提下，让函数、接口、类等与多种类型一起工作，从而实现复用

```ts
//创建泛型函数
function returnValue<Type>(t:Type):Type{
    return t
}

console.log(returnValue<number>(123))
console.log(returnValue<string>("12341231243"))
```

### 泛型约束

默认情况下，泛型函数的类型变量Type可以代表多个类型，这导致无法访问任何属性

通过添加泛型约束收缩类型，可以解决这个问题，主要有以下两种方式

1. 指定更加具体的类型

```ts
function id<Type>(value:Type[]):Type[]{
	console.log(value.length)
	return value
}
```

2. 添加约束

```ts
interface Ilength{length:number}
function id<Type extends ILength>(value:Type[]):Type[]{
	console.log(value.length)
	return value
}
```



### 多个泛型变量

泛型中的类型变量可以有多个，并且类型变量之间还可以约束

```ts
//获取对象属性的方法
//keyof关键字获取一个对象键名称的联合类型
function getProp<Type , Key extends keyof Type>(type:Type,key:Key){
    return type[key]
}

class Person{
    name:string
    age:number
    constructor(name:string,age:number){
        this.name=name
        this.age=age
    }
}

let p=new Person("陈慕华",18)
console.log(getProp(p,"name"),getProp(p,"age"))

```



### 泛型接口

接口也可以配合泛型使用，增加其灵活性和复用性

```ts
//建立一个泛型接口
interface value<Type>{
    returnValue:(value:Type) => Type
}
//Person类实现接口
class Person implements value<string>{
    returnValue(value:string){return value}
}

let p =new Person()
console.log(p.returnValue("瞠目话"))
```

