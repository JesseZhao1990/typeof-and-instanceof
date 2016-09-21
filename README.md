## typeof 和 instanceof ##
js中typeof和instanceof一般用来判断变量类型。由于js这门语言本身设计的缺陷。导致有很多令人困惑和感觉起来不合理的地方。比如说js声称自己有五种基本数据类型undefined, null, boolean, number and string。但是扯淡的是。。。当你用typeof判断这些基本类型时。奇葩的事情就出现了。比如说typeof null竟然是object。那是不是null是引用数据类型呢？其实不是……它真的是基本数据类型。这是一个bug。有人提议把这个bug修改掉。但是被残忍的拒绝了…………

想要搞清楚下面的问题。首先要清楚基本数据类型和引用数据类型

**基本数据类型**

基本数据类型包含五种：undefined, null, boolean, number 和 string

**引用数据类型**

所有的非基本数据类型都是引用数据类型。也就是对象。你可以给引用数据类型添加属性，但是不能给基本数据类型添加属性


### typeof ###
首先先看标准怎么说的。
[标准的链接](http://www.ecma-international.org/ecma-262/5.1/#sec-11.4.3)

翻译过来是这样描述的

typeof操作符的用法

typeof val. 执行一元表达式得到一个值。下面是一些值和结果的对照关系：

```

undefined	---> “undeifined"

null ---> “object"

Boolean ---> “boolean"

Number ---> “number"

String ---> “string"

Function ---> “function"

其他值 ---> “object"

```
typeof null返回object是一个bug，但是不能修复，因为它会破坏现在存在的代码。注意，function也是一个对象，但是typeof做了区分，Array也是一个对象。但是typeof没有作区分。这就是js的设计者做的很多不完美的地方。留了很多缺陷。有一篇文章详细剖析了type of 返回object 这一个bug的具体信息。可以点击[这里](http://www.2ality.com/2013/10/typeof-null.html)查看

### instanceof ###
instanceof检测值是不是一个构造函数的实例。但这里需要注意的是基本数据类型使用instanceof都会返回false。

```
"aaa" instanceof String  //false

var a = new String("aaa") 
a instanceof String  //true

```
思考一下以上两种方式为何会出现不同的结果? 为什么"aaa".length和"aaa".subString()这些String对象上的方法可以被基本数据类型使用。其实可以这样去理解，基本数据类型自身不是对象。但是在运行环境中。js会自动帮他们生产虚拟的对象。运行结束之后，这些虚拟对象被销毁。

那接下来思考一下几个问题：

1.如何判断一个变量是string类型？

```
function isString(s){
	if(typeof s == "string"){
		return true;
	}else if(typeof s =="object" && s instanceof== "String"){
		return true;
	}else{
		return false;
	}
}

```

2.如何判断一个变量是数组呢？

最简单的方法是如下写法。

```
function isArray(value){
	return value instanceof Array;	
}

```
这样写一般也不会遇到啥大问题。但是[在不同 iframe 中创建的 Array 并不共享 prototype](http://perfectionkills.com/instanceof-considered-harmful-or-how-to-write-a-robust-isarray/)

所以想要严谨一些。兼容iframe这种情况的话。可以用如下的方法。

```

var isArray = function(obj) { 
    return Object.prototype.toString.call(obj) === '[object Array]'; 
}

```

3.写一个通用的判断各种类型的方法如何？

```
var is = function (obj,type) { 
	return (type === "Null" && obj === null) || 
			(type === "Undefined" && obj === void 0 ) || 
			(type === "Number" && isFinite(obj)) || 
			Object.prototype.toString.call(obj).slice(8,-1) === type; 
}

```

