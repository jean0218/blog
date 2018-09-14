javaScript中，数据的存储位置会对代码整体性能产生重大影响。有下面四种基本的数据存取位置：

    字面量：字符串、数字、布尔值、对象、数组、函数、正则表达 式，null、undefined

    本地变量：let,const定义的数据存储单元

    数组元素

    对象成员

它们有着各自的性能特点：

访问字面量和局部变量的速度最快，相反，访问数组元素和对象成员相对较慢。

由于局部变量存在于作用域链的起始位置，因为访问局部变量比访问跨作用域变量更快。变量在作用域链中的位置越深，访问所需要的时间就越长。由于全局变量总是处于作用域链的最末端，因此访问速度也是最慢的。

避免使用with语句，因为它会 改变 执行环境用域链。同样，try-catch语句中的catch也有同样的影响，因此也要小心使用。

嵌套的对象成员明显影响性能，尽量少用。

属性或方法在原型链中的位置越深，访问它的速度也越慢。

通常来说，你可以把常用的对象成员、数组元素、跨域变量保存在局部变量中来改善JS性能，因为局部变量访问速度最快。

管理作用域

每一个js函数都是表示为一个对象，是Function对象的实例。

内部属性[[Scope]]包含了一个函数被创建的作用域中对象的集合，这个集合被称为函数的作用域链。



标识符解析的性能

在执行环境的作用域链中，一个标识符所在的位置越深，它的读写速度也就越慢。

函数中读写局部变量总是最快的，而读写全局变量通常是最慢的。

法则：如果某个跨作用域的值 在函数中被引用一次以上，那么 就把它存储到局部变量里。

优化前：

function initUI(){
    var bd = document.body,
        links = document.getElementByTagName("a"),
        i = 0,
        len = links.length;
    while(i < len){
        update(links[i++]);
    }
    document.geElementById('go-btn').onclick = function(){
        start();
    };
    bd.className = "active";
}
该函数调用了三次document，搜索该变量必须遍历整个作用域链，直到最后在全局变量对象中找到。

优化后：

function initUI(){
    var doc = document,
        bd = doc.body,
        links = doc.getElementByTagName("a"),
        i = 0,
        len = links.length;
    while(i < len){
        update(links[i++]);
    }
    doc.geElementById('go-btn').onclick = function(){
        start();
    };
    bd.className = "active";
} 
改变作用域链

with可以在执行时临时改变作用域链，尽量避免！

try-catch语句中的catch子句也能改变执行环境作用域链。如果准确使用try-catch，请确保了解可能会出现的错误。

尽量简化代码使用catch子句对性能的影响最小，推荐做法，将错误 委托给一个函数来处理。如：

try{
    methodThatMigthCauseAnError();
}catch(ex){
    handleError(ex);//委托给错误错误函数
}
动态作用域链（避免）

with，try-catch，包含eval()的函数，都被认为是动态作用域。

闭包、作用域和内存

对象成员

js中的对象是基于原型的。对象有两种成员类型：实例成员和原型成员。

hasOwnProperty() 判断对象是否包含实例成员，不会搜索原型

in 判断对象是否包含特定的属性，搜索原型

原型链

对象在原型链中存在的位置越深，找到它越慢。

搜索实例成员比从字面量或局部变量中读取数据代价更高。

嵌套成员

window.location.href,每次遇到点操作符，嵌套成员会导航js引擎搜索所有对象成员。

对象成员嵌套越深，读取速度越慢。

缓存对象成员值

尽可能避免使用对象成员。如果在同一个函数中多次读取同一个对象，将值保存在局部变量中减少查找。

但这种优化方法，并不推荐用于对象的成员方法。因为许多对象方法使用this来判断执行环境，把一个对象方法保存在局部变量会导致this绑定到window.



第4章 算法和流程控制

循环的类型：

for循环

for(var i = 0; i < 10; i++){

}


while循环

var i = 0;
while(i < 10){
    i++;
}
do-while循环 是js中唯一一种后测循环

var i = 0;
do{

}while(i++ < 10);
for-in循环，可以枚举任何对象的属性名。比其它几种要慢

for(var prop in object){} 

除非明确需要迭代一个属性数量未知的对象，应避免使用for-in循环。如果 需要 遍历一个数据有限的已知属性列表。建议使用以下模式：

var props = ['prop1','prop2'],
    i = 0;
while(i < props.length){
    process(object[props[i++]]);
}
这段代码创建了一个由属性名构成的数组

优化循环的第一步是减少对象成员及数组项的查找次数

通过颠倒数组的顺序来提高循环性能

减少迭代次数

优化if-else最简单的优化方法是，确保最可能出现的条件 放在首位


js dom编程

性能优化是个大课题，涉及的地方方面面。
dom编程

dom操作为什么会慢？
浏览器中通常会把DOM和javaScript独立实现，这意味着两个相互独立的功能只要通过接口彼此连接，就会产生消耗。
好比两个独立的岛屿，ECMAScript每次访问DOM都要经过这座桥，访问次数越多，消耗越多。
所以，要减少访问DOM的次数。
通用法则：减少访问DOM的次数，把运算留在JS这一端处理。

innerHTML会比document.createElement()更快
建议使用数组来合并大量字符串，这样会让innerHTML效率更高。