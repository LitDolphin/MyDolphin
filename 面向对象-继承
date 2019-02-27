<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>继承</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    
</head>
<body>
    
</body>
<script>
/*
ecmascript只支持实现继承，一般通过原型链实现。
*/
console.log('原型链的实验')
/*
原型链是什么：原型和构造函数以及它的实例之间有关系，如果原型a变成了原型b的实例，那么原型a就有了一个指向原型b的指针，以此类推，如果原型b又是原型c的实例，
那么原型b又有了指向c的指针，层层递推，这就是原型链。
原型链有基本的模式，下面是demo
*/
function Test1Type1(){
    this.property=true
}
Test1Type1.prototype.getValue1=function(){
    return this.property
}
function Test1Type2(){
    this.subproperty=false
}
Test1Type2.prototype=new Test1Type1()//继承
Test1Type2.prototype.getValue2=function(){
    return this.subproperty
}
var test1instance1=new Test1Type2()
console.log(test1instance1.getValue1())//true
console.log(test1instance1.constructor==Test1Type1)//true
/*
1.继承的方式本质是重写原型
2.继承后创建的实例的constructor指向最原始的原型的构造函数，因为Test1Type1的原型指向了TestType2的原型，所以调用的是后者的constructor
对第二条有疑惑，暂且记住结论
原型链本质上扩展了原型搜索机制，搜索过程总是一环一环向上。
*/
/*
三个注意点
1.别忘记默认的原型：Object就是原型链的最终端
2.确定原型和实例的关系：可以使用instanceOf()和isPrototypeOf()检测，只要是原型链中出现过的原型，都可以说是该原型链所派生的实例的原型
3.谨慎地定义方法：给原型添加方法一定要放在替换原型的语句也就是继承之后。另外，绝对不能使用对象字面量的方法添加方法，否则，原型链继承无效
下面是demo.
*/
function Test2Type1(){
         this.property=true
}
Test1Type1.prototype.getValue3=function(){
         return this.property
}
function Test2Type2(){
         this.subproperty=false
}
Test2Type2.prototype=new Test2Type1()
var test2instance2=new Test2Type2()
console.log(test2instance2 instanceof(Test2Type1))//true 注意，这里是true
Test2Type2.prototype={
          someOtherFunc:function(){
              return true
          }
}
var test2instance1=new Test2Type2()
try{console.log(test2instance1.getValue3())}
catch(e){
    console.log(e)
    console.log('这是问题')
}
/*
为什么使用对象字面量的方法就会出现报错，因为这相当于打断了原型链，这时候。Test2Type2的原型事实上仅仅和Object相连。
*/
console.log(test2instance1 instanceof(Object),test2instance1 instanceof(Test2Type2),test2instance1 instanceof(Test2Type1))
//true true false 这里原型链已断

console.log('原型链的问题')
function Test3Type1(){
         this.colors=['red','blue']
}
function Test3Type2(){
}
Test3Type2.prototype=new Test3Type1()
var test3instance1=new Test3Type2()
console.log(test3instance1.colors)//['red','blue']
test3instance1.colors.push('black')
var test3instance2=new Test3Type2()
console.log(test3instance2.colors)//['red','blue','black']
test3instance2.colors.shift()
console.log(test3instance2.colors)//['blue','black']
/*
由此可见，当原型链中的一环的一个属性是引用类型，那么继承者也会把这个属性继承下来，导致的结果就是该继承者创造的实例会相互影响
另外，在创建子类型的实例的时候，不能向超类型的构造函数传递参数，因为这会影响其他实例对象。
因此，很少单独使用原型链
*/

console.log('借用构造函数')
/*
借用构造函数的技术又叫经典继承或者伪造对象，这个技术解决了原型中包含引用类型值所带来的问题并且允许子类型构造函数向超类型构造函数传递参数，
但是却降低了函数复用
*/
function Test4Type1(name){
this.name=name;
this.colors=['red','blue']
}
function Test4Type2(){
    Test4Type1.call(this,'Dolphin');//使用call() apply()的结果是：每创建一个实例，就会调用一次超类型构造函数。也就解决了引用对象互相影响的问题
    this.age=19                     //向超类型构造函数传递了name的值
}
var test4instance1=new Test4Type2()
test4instance1.colors.push('black')
console.log(test4instance1.colors)//['red','blue','black']
console.log(test4instance1.age,test4instance1.name)//19 Dolphin
var test4instance2=new Test4Type2()
console.log(test4instance2.colors)//['red','blue']

console.log('组合式继承')
/*
组合式继承是最常用的，又叫伪经典继承，除了解决了直接用原型链的两个问题还提高了函数复用率，必须会用。
它使用原型链继承原型属性和方法，借用构造函数来实现对实例属性的继承
*/
function Test5Type1(name){
    this.name=name
    this.colors=['red','blue']
}
Test5Type1.prototype.sayName=function(){
    return this.name
}
function Test5Type2(name,age){
    Test5Type1.call(this,name)//这里是借用构造函数的技术进行实例属性的继承
    this.age=age
}
Test5Type2.prototype=new Test5Type1()
Test5Type2.prototype.constructor=Test5Type2
Test5Type2.prototype.sayAge=function(){
    return this.age
}
var test5instance1=new Test5Type2('Dolphin','19')
test5instance1.colors.push('black')
console.log(test5instance1.colors,test5instance1.sayAge(),test5instance1.sayName())//['red','blue','black'],19,Dolphin
var test5instance2=new Test5Type2('Dol','3')
test5instance2.colors.pop()
console.log(test5instance2.colors,test5instance2.sayAge(),test5instance2.sayName())//['red'],19,Dol

console.log('原型式继承')
/*
克罗克福德提出可以借助原型创建类似的新对象，不必大动干戈去创建自定义对象,他使用以下这个函数对传入的对象进行了一次浅复制
*/
function object(o){
    function F(){}
    F.prototype=o
    return new F()
}
var test6instance1={
    name:'Dolphin',
    colors:['red','blue']
}
var test6instance2=object(test6instance1)
test6instance2.name='Dol'
test6instance2.colors.push('black')
console.log(test6instance1.name)
console.log(test6instance2.colors)//['red','blue','black']
var test6instance3=object(test6instance1)
console.log(test6instance3.colors)//['red','blue','black']
/*
由此可见，复制的两个副本可以有自己的私有属性（基本数据类型），引用数据类型却是共享的，如果有特殊情况可以使用这个方式
es5规范了这个方法，可以使用Object.create()方法，可以传入两个参数，分别是基础原型和为新对象定义额外属性的对象
下面是demo
*/
var test7instance1={
    name:'Dolphin',
    colors:['red','blue']
}
var test7instance2=Object.create(test7instance1,{
    age:{
        value:19
    }
})
console.log(test7instance2.colors)//['red','blue']

console.log('寄生式继承')
/*
寄生式继承也是克罗克福德推广的，即创建一个用于封装继承过程的函数。该函数在内部以某种方式增强对象，最后再像是真的一样返回对象。
寄生式继承和原型式继承是同一个用途，基于一个对象，创建多个类似的对象
下面是demo
*/
function createAnother(original){
         var clone=object(original)//通过传入对象构建一个新对象
         clone.sayHi=function(){
                return    'hi'
         }
         return clone
}
var test7instance3={
    name:'Dolphin',
    colors:['red','blue']
}
var test7instance4=createAnother(test7instance3)
console.log(test7instance4.sayHi())//hi

console.log('寄生组合式继承')
/*
最后一种继承方式，为了解决使用组合式继承时调用子类型构造函数时总是重写这些属性
它通过借用构造函数来继承属性，原型链的混成形式来继承方法。
下面是组合式继承问题所在
*/
function Test8type1(name){
    this.name=name;
    this.colors=['red','blue']
}
Test8type1.prototype.sayName=function(){
    console.log(this.name)
}
function Test8type2(name,age){
    Test8type1.call(this,name)     //第二次调用Test8type1()
    this.age=age
}
Test8type2.prototype=new Test8type1() //第一次调用Test8type1()
Test8type2.prototype.constructor=Test8type2
Test8type2.prototype.sayAge=function(){
    console.log(this.age)
}
/*
问题出现在两次调用超类型构造函数后的结果,第一次将超类型构造函数中的实例属性传递到子类型构造函数的原型中，
第二次将子类型函数中的实例属性真正实例化，造成的后果就是后者覆盖前者，每创建一个实例，就要覆盖一次。
寄生组合式继承解决了这个问题。
寄生组合式继承实际应用了寄生式继承的思想，通过如下函数来创建一个超类型原型的副本，然后再给子类型原型继承
*/
function inheritPrototype(Test9type2,Test9type1){
        var Test9type3=object(Test9type1.prototype)     //通过传入的超类型构造函数，创建超类型原型的副本
        Test9type3.constructor=Test9type2               //增强对象
        Test9type2.prototype=Test9type3                 //副本继承
}
/*
下面是这个函数的应用，也就是寄生组合式继承的应用
*/
function Test10type1(name){
         this.name=name;
         this.colors=['red','blue']
}
Test10type1.prototype.sayName=function(){
        console.log(this.name)
}
function Test10type2(name,age){
        Test10type1.call(this,name)
        this.age=age
}
inheritPrototype(Test10type2,Test10type1)
Test10type2.prototype.sayAge=function(){
       console.log(this.age)
}
/*
开发人员普遍认为寄生组合式继承是引用类型最理想的继承范式
*/











</script>
</html>
