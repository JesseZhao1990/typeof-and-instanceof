## typeof 和 instanceof ##
js中typeof和instanceof一般用来判断变量类型。由于js这门语言本身设计的缺陷。导致有很多令人困惑和感觉起来不合理的地方。比如说js声称自己有五种基本数据类型undefined, null, boolean, number and string。但是扯淡的是。。。当你用typeof判断这些基本类型时。奇葩的事情就出现了。比如说typeof null竟然是object。那是不是null是引用数据类型呢？其实不是……它真的是基本数据类型。这是一个bug。有人提议把这个bug修改掉。但是被残忍的拒绝了…………

### typeof ###
首先先看标准怎么说的。
[标准的链接](http://www.ecma-international.org/ecma-262/5.1/#sec-11.4.3)

翻译过来是这样描述的

typeof操作符的用法

typeof val

typeof 操作符的判断标准是这样的:

1.首先判断val的值是基本数据类型还是引用数据类型
2.如果是引用数据类型
