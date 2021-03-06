---
layout: post
title: JavaScript
categories: HTML
description: JavaScript
keywords: JavaScript
---



Js的概念和声明:
- 问题:在网页的发展历程中,发现网页不能对用户的数据进行自动校验,和提供一些特效造成用户体验极差
- 解决:使用JavaScript
- 作用:
    - 可以让网页和用户之间进行直接简单的交互.
    - 可以给网页制作特效和动画
- 注意:
    - js是由浏览器解析执行的.
    - js需要在HTML文档中进行声明
- 使用:
    - 声明Js代码域
        - 在head标签中使用script声明js代码域
        - 在head标签中使用script引入外部声明的js文件



js变量

变量声明
- var
    - var的作用域是函数作用域，在一个函数内利用var声明一个变量，则这个变量只在这个函数内有效
    - 没有块的概念，可以跨块访问, 不能跨函数访问
    - 存在变量声明提前,可声明同名变量覆盖,允许出现同名变量，但是后面的会将前面的覆盖。
    - js中的代码可以不适用分号结尾，但是为了提升代码的阅读性，建议使用分号。
- let
    - 作用域是块级作用域（在ES6之前，js只存在函数作用域以及全局作用域）
    - 不存在变量声明提前
    - 不能重复定义
    - 存在暂时性死区：只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量
- const
    -用来定义常量，使用时必须初始化(即必须赋值)，只能在块作用域里访问，而且不能修改。存在暂时性死区，不存在变量声明提前，不允许重复定义


数据类型判断 typeof
- number:数值类型
- string:字符类型,注意：在js中字符可以使用单引号也可以使用双引号
- boolean:布尔类型
- object:对象类型

特殊值
- null：object
- undefined undefined
- NaN： number

数据作用:变量是用来存储数据的,方便程序进行操作.

数据转换
··· -> number
- string 数字：对应number
- string 非数字：NaN(number类型)
- boolean true： 1
- boolean false：0
- object 有具体： 1
- object null： 0

··· -> boolean
- number 非0： true
- number 0：   false
- string  非空字符串 :       true
- string  空字符串 "":       false
- object  具体的对象  :    true
- object  null    :       false
- 声明不赋值的变量  :       false


运算符和逻辑结构
- 作用：变量是存储要处理的数据的，运算符和逻辑结构就是用来处理数据的，结合变量进行数据处理
- 算术运算符（+ - * / %）
    - 在算术运算中如果两边的数据类型不是number的话，会使用Number()强转后在进行运算.(在字符串中“+”符合代表的是字符串的连接符，不会参与运算)
- 逻辑运算符(! & | && ||)
    - & | 非短路 位运算
    - && || 短路 逻辑运算
- 关系运算符(!= >= <= > < === ==)
    - 等值运算符==  先判断类型，类型一致则直接比较。类型不一致，则先使用Number()进行强转后再进行比较。
    - 等同运算符=== 先判断类型，类型一致则再比较内容，内容一致则返回true，内容不一致则返回false。类型不一致则直接false
    - null和undefined在做==判断时候返回true
- 自增运算符(++ -- += -=)


逻辑结构
- if结构
```
    单分支结构：
        if(判断条件){执行体}
    双分支：
        if(判断条件){执行体}else{执行体}
    多分支：
        if(判断条件){执行体}else if(判断条件){执行体}……else{执行体}
```
- swicth选择结构：
```
    switch (a){
        case "1":
            alert("第一项选择");
            break;
        case "2":
            alert("第二项选择");
            break;
        default:
            alert("没有对应的选项");
            break;
        }
    注意：
        判断的变量可以是number类型也可以是string类型，但是不要混用。
```
- 循环结构：
    - for(变量;条件;迭代条件){循环体}循环
    - while(循环条件){循环体}
    - do{循环体}while(循环条件)
- 注意：js中变量是没有类型的，但是数据是有类型的，在进行数据处理的时候要注意数据的类型。

数组
- 问题：使用变量存储数据，如果数据量比较大的时候，会造成需要声明大量的变量去存储数据，代码整洁度及阅读性极差，数据的完整性得不到保证。
- 作用：存储数据，保证数据的完整性，操作方便。
- 数组声明
    - var arr=new Array();//声明一个空数组对象
    - var arr=new Array(length)//声明一个指定长度的数组
    - var arr=[元素]//声明数组(最常用);<br/>
    注意：js中的数组声明不用指定长度，js的数组长度是不固定的，会随着元素的数量改变而改变。
- 数组的赋值和取值
    - 数组可以存储任意类型的数据：数组名[角标]=值;//角标可以是任意的正整数或者是0
    - 数组的取出：数组名[角标]//返回当前角标对应存储的值；如果角标不存在，返回undefined;
- 数组特点
    - js中的数组可以存储任意类型的数据。
    - js的数组可以通过length属性动态的改变长度。可以增加，也可以缩短。
    - 注意：
        - 如果是增加，则使用逗号进行占位
        - 如果是缩减则从后往前减少存储的数据。


- 数组的length属性
    - 作用1：数组名.length//返回当前数组的长度。
    - 作用2：数组名.length=新的值//动态的改变数组的长度
    - 注意：length>原有长度，则使用空进行填充。length<原有长度，则从后面进行截取，最后的数据会被删除。
- 数组的遍历
    - 普通for循环： for(var i=0;i < arr.length; i++)
    - for-in: for(var i in arr)
- 数组的操作学习
    - 数组的合并：arr.concat(b,c);//数组的合并
    - 数组指定间隔符转换字符串:var b=arr.join("-");
    - 数组移除最后一个元素并返回:var ele=arr.pop();
    - 数组的追加，返回新的长度：var ln=arr.push("lol");//追加的元素可以是一个数组，但是为作为一个角标值存在
    - 数组的移除第一个元素:var ele=arr.shift();
    - 数组的在开始位置插入指定元素:var a=arr.unshift("又是周五了");
    - 数组删除指定位置元素：var arr2=arr.splice(1,3,"a");


函数
- 问题：其实开发就是对现实生活中的问题使用代码进行解决，同类型的问题非常多，这样就需要每次都将代码重新声明一遍，造成代码过于冗余。
- 解决：封装成函数，不用重复声明，调用即可。
- 作用:封装功能代码，降低代码冗余
- 函数的声明：
    - 方式一:function 函数名(形参名1,形参名2……){执行体}
    - 方式二:var 函数名=new Function("形参1","形参2"……,"函数执行体");注意：此声明表明在js中函数是作为对象存在的。"形参"是作为实参存在
    - 方式三:var 变量名=function(形参名1,形参名2……){函数执行体}    
- 函数的形参
    - js中的函数在调用时，形参可以不赋值，不会报错，但是默认为undefined
    - js中的函数在调用时，形参赋值可以不全部赋值，不会报错，但是实参会依次进行赋值。
    - 注意：js中没有函数重载，只有函数覆盖。
- 函数的返回值
    - 在js中如果函数有返回值则直接返回，没有返回值默认返回undefined
- js的代码声明区域和执行区域是一起的，都是在js代码的代码域中。
- 函数的调用:
    - 在加上代码域中直接调用(主要进行页面资源初始化)
    - 使用事件机制(主要实现和用户之间的互动)
    - 作为实参传递(主要是动态的调用函数)
    - 注意：小括号为函数的执行符，函数名()才会被执行，直接函数名则作为对象使用。
- 注意：
    - js的代码区域只有一个,包括引入的js代码，浏览器会将引入的js文件和内部声明的js代码解析成一个文件执行。
    - js代码的调用和声明都在一个区域内。


类
- 类的声明
```js
function 类名(形参1，形参2……){
    this.属性名1=形参1;
    this.属性名2=形参2;
    ……
    this.属性名=fn
}
```
- 类的使用
    - var 对象名=new 类名(实参1，实参2……);
    - 注意：js中类的内容只是对象的公共部分，每个对象还可以自定义的进行扩充。
- 类的继承
    - 通过prototype关键字实现了不同对象之间的数据共享。
    - 作用:实现某个类的所有子对象的方法区对象的共享，节省内存
- 自定义对象
    - 对象的作用：用来存储整体数据。
    - 原因：很多时候我们没有办法预先知道一个对象应该有哪些属性，所以只能临时的创建一个对象来自定义属性存储数据。来保证数据的完成性
    - 应用：Ajax中会使用。
    - 使用：
        - 创建自定义对象
        ```js
            var 对象名=new Object();
                对象名.属性名1=属性值1;
                对象名.属性名2=属性值2;
                对象名.属性名3=属性值3;
                ……
            
            var 对象名={};
                对象名.属性名1=属性值1;
                对象名.属性名2=属性值2;
                对象名.属性名3=属性值3;
                ……
        ```
        - 一般用来存储数据，不会在自定义对象中存储函数对象。
        - js中的对象属性和内容是可以自定义的扩充的，不是依赖于类的声明的，类只是对象公共部分的一种声明，是为了节省代码的冗余的。

常用对象和方法
- String：需要new
    - 使用：字符串.函数名即可 
    - 大小写转换： toUpperCase() 转换大写 toLowerCase() 转换小写 
    - 字符串截取 
        - substr(0,1) 从指定开始位置截取指定长度的子字符串
        - substring(0,1) 从指定位置开始到指定的 结束位置的子字符串（含头不含尾）
        - 查找字符位置 
            - indexOf 返回指定字符第一次出现的位置。
            - lastIndexOf 返回指定字符最后一次出现的位置。
- Date：需要new
    - 使用： var 变量名=new Date(); 
    - 注意：获取的是客户端的时间，不能作为系统功能校验的时间的。
- Math：在使用的时候不能new，使用Math.方法名调用即可
- Global:该对象从不直接使用并且不能new，也是就直接写方法名调用即可
    - eval()：将字符串转换为可执行的js代码

事件机制
- 解释：当我们的行为动作满足了一定的条件后，会触发某类事务的执行。
- 作用：主要是结合js的函数来使用。
- 内容:
    1. 单双击事件
        - 单击：onclick          当鼠标单击的时候会触发
        - 双击：ondblclick       当鼠标双击的时候会被触发
    2. 鼠标事件
        - onmouseover         当鼠标悬停在某个HTML元素上的时候触发
        - onmousemove         当鼠标在某个HTML元素上移动的时候触发
        - onmouseout          当鼠标在某个HTML元素上移出的时候触发
    3. 键盘事件
        - onkeyup             当键盘在某个HTML元素上弹起的时候触发
        - onkeydown           当键盘在某个HTML元素上下压的时候触发
    4. 焦点事件
        - onfocus             当某个HTML元素获取焦点的时候触发
        - onblur              当某个HTML元素失去焦点的时候触发
    5. 页面加载事件            
        - onload              当页面加载成功后触发。
- 注意:
    - js中添加事件的第一种方式：在HTML上直接使用事件属性进行添加，属性值为所监听执行的函数。
    - js中的事件只有在当前HTML元素上有效。
    - 一个HTML元素可以添加多个不同的事件。
    - 一个事件可以监听触发多个函数的执行,但是不同的函数要使用分号间隔
- 给合适的HTML标签添加合适的事件
    - onchange----select下拉框
    - onload------body标签
    - 单双击-------用户会进行点击动作的HTML元素
    - 鼠标事件------用户会进行鼠标移动操作的。
    - 键盘事件------用户会进行键盘操作的HTML元素。
- 事件之间的冲突
- 事件的阻断
    - 当事件所监听的函数的将返回值返回给事件时：
        - false：则会阻断当前事件所在的HTML标签的功能
        - true:则继续执行当前事件所在的HTML标签的功能


window对象
- BOM浏览器对象模型：是规范浏览器对js语言的支持(js调用浏览器本身的功能)。BOM的具体实现是window对象  
- 对象使用
    - window对象不用new，直接进行使用即可，window关键字可以省略不写
    - 框体方法
        - alert:警告框 提示一个警告信息，没有返回
        - confirm:确认框  提示用户选择一项操作（确定/取消）
            - 点击确定 返回true
            - 点击取消  返回false
        - prompt:提示框， 提示用某个信息的录入或者说收集
            - 点击确定，返回当前用书录入的数据，默认返回空字符串
            - 点击取消,返回null
    - 定时和间隔执行方法
        - setTimeout:指定的时间后执行指定的函数：参数1：函数对象，参数2：时间，单位毫秒，返回值：返回当前定时器的id
        - setInterval:每间隔指定的时间执行指定的函数：参数1：函数对象，参数2：时间，单位毫秒，返回值：返回当前间隔器的id
        - clearTimeout:用来停止指定的定时器：参数：定时器的id，clearInterval:用来停止指定的间隔器，参数：间隔器的id
    - 子窗口方法
        - window.open('子页面的资源(相对路径)','打卡方式','配置');注意:关闭子页面的方法window.close(),但是此方法只能关闭open方法打开的子页面。
        ```
            示例：window.open('son.html','newwindow','height=400, width=600, top=100px,left=320px, toolbar=yes, menubar=yes, scrollbars=yes, resizable=yes,location=no, status=yes');
        ```
        - 子页面调用父页面的函数: window.opener.父页面的函数
    - 对象常用属性
        - 地址栏属性:location
            - window.location.href="新的资源路径（相对路径/URL）"
            - window.location.reload()重新加载页面资源
        - 历史记录属性
            - window.history.forward() 页面资源前进，历史记录的前进。
            - window.history.back()    页面资源后退，历史记录后退
            - window.history.go(index) 跳转到指定的历史记录资源
            - 注意window.history.go(0)相当于刷新。
        - 屏幕属性
            - window.srceen.width;//获取屏幕的宽度分辨率
            - window.screen.height;//获取屏幕的高度分辨率
        - 浏览器配置属性
        - 主体面板属性(document)

document对象
- document对象的概念：浏览器对外提供的支持js的用来操作HTML文档的一个对象，此对象封存的HTML文档的所有信息。通过此对象可以让我们灵活的操作HTML文档，达到修改，完善HTML文档
的目的。
- 使用document
    - 获取HTML元素对象
        - 直接获取方式：
            - 通过id：var 变量名=document.getElementById("uname");//返回指定的HTML元素对象；
                - 注意：此对象中封存了HTML元素标签的所有信息。
            - 通过name属性值：var 变量名=document.getElementsByName("name属性值");
                - 注意：获取的是相同name属性的HTML元素对象的数组。
            - 通过标签名：var 变量名=document.getElementsByTagName("标签名");
                - 注意：返回的是存储了该网页中指定的标签的所有对象的数组。
            - 通过class属性值
        - 间接获取方式：
            - 父子关系：var 变量名=父节点对象.childNodes;
                - 注意：子节点数组中会包含文本节点，可以使用nodeType属性
            - 子父关系：先获取子元素对象(参照直接方式)通过子元素对象获取父元素对象 var 变量名=子元素对象.parentNode
            - 兄弟关系：先获取当前元素，根据兄弟关系选择对应的获取方式
                - var 变量名=元素对象.previousSibling; //兄
                - var 变量名=元素对象.nextSibling; //弟

    - 操作HTML元素属性
        - 获取：
            - 元素对象名.属性名//返回当前属性的属性值。----固有
            - 元素对象名.getAttribute("属性名");//返回自定义属性的值-----自定义
        - 修改
            - 元素对象名.属性名=属性值
            - 元素对象名.setAttribute("属性名","属性值");//修改自定义属性的值----自定义
        - 注意：
            - 尽量的不要去修改元素的id值和name属性值。
            - 使用自定义方式获取固有属性内容，value的值获取的是默认值，不能够获取到实时的用户数据
    - 获取元素内容
        - 获取
            - 元素对象名.innerHTML//返回当前元素对象的所有内容，包括HTML标签
            - 元素对象名.innerText//返回当前元素对象的文本内容，不包括HTML标签
        - 修改
            - 元素对象名.innerHTML="新的值"//会将原有内容覆盖，并HTML标签会被解析
            - 元素对象名.innerHTML=元素对象名.innerHTML+"新的值"//追加效果
            - 元素对象名.innerText="新的值"//会将原有内容覆盖，但HTML标签不会被解析，会作为普通文本显示。    

    - 操作元素样式：
        - 通过style属性
            - 元素对象名.style.样式名="样式值"//添加或者修改
            - 元素对象名.style.样式名=""//删除样式
            - 注意:以上操作，操作的是HTML的style属性声明中的样式。而不是其他css代码域中的样式。
        - 通过className
            - 元素对象名.className="新的值"//添加类选择器样式或者修改类选择器样式
            - 元素对象名.className=""//删除类样式。
    - 操作HTML文档结构
        - 文档结构:指的是HTML文档的树形结构。
        - 节点：HTML文档树中的基本元素单位称为一个节点。
        - 新建节点：var obj=document.createElement("标签名");元素对象名.appendChild(obj);
        - 添加节点：节点对象.appendChild(节点对象);
        - 删除节点：节点对象.removeChild(节点对象);
    - 操作form
        - 获取form表单对象
            - 使用id:var fm=document.getElementById("fm");
            - 使用name属性:var frm=document.frm;
        - 获取form下的所有表单元素对象集合
            - fm.elements
        - form表单的常用方法
            - 表单对象.submit();//提交表单数据。
        - form的属性操作：
            - 表单对象名.action="新的值"//动态的改变数据的提交路径
            - 表单对象名.method="新的值"//动态的改变提交方式
        - 表单元素的通用属性
            - 只读模式：readonly="readonly"//不可以更改，但是数据可以提交
            - 关闭模式：disabled="disabled"//不可以进行任何的操作，数据不会提交


event对象