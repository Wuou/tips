#### NSRegularExpression参照表

正则表达式语法很简洁，经常会出现多种符号结合使用。下面是常用正则表达式符号含义汇总表，可以收藏[本网页](https://github.com/pro648/tips/wiki/iOS%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F%E8%AF%AD%E6%B3%95%E5%85%A8%E9%9B%86)备用。

- 特殊字符

```
* ? + [ ] ( ) { } ^ $ | \ . / 
```

如果需要匹配这些特殊符号，需要在其前面用`\`标志出。

- 元字符

|字符|描述|
|---|----|
|[pattern]|匹配pattern中的任一字符。如：[a-z]匹配a-z的任一字符。|
|.|匹配`\n`之外的任何字符。|
|^|匹配字符的开始位置。|
|$|匹配字符的结束位置。|
|\ |将下一个字符标记为一个特殊字符、或一个原义字符、或一个向后引用。如：`n`匹配字符*n*，`\n`匹配一个换行符。序列`\\`匹配\，`\(`匹配( 。
|\b|匹配单词边界，边界发生在单词(\w)和非单词(\W)字符之间的转换处。如：`ne\b`可以匹配*throne*中的*ne*，但不能匹配*Chinese*中的*ne*，也能匹配`throne!`中的*ne*，因为`!`是非单词。|
|\B|匹配非单词边界。`ne\B`可以匹配*Chinese*中的*ne*，不能匹配*throne*中的*ne*。|
|\cX|匹配control-X字符。如：\cM匹配一个control-M或回车符。X的值必需为a-z或A-Z之一。否则，c将被视为一个原义的`c`字符。|
|\d|匹配一个数字字符，等价于[0-9]。|
|\D|匹配一个非数字字符，等价于[^0-9]。|
|\f|匹配一个换页符。|
|\n|匹配一个换行符。|
|\s|匹配任何空白符，包括空格、制表符、换页符等等。等价于[\t\n\f\r\p{Z}]|
|\S|匹配任何非空字符。|
|\w|匹配包括下划线的任何单词字符。等价于[a-zA-Z0-9]。|
|\W|匹配任何非单词字符。等价于[^a-zA-Z0-9]。|

- 运算符

|字符|描述|
|---|---|
|\|| 或，`A\|B`匹配*A*或*B*|
|*|匹配零次、或多次，尽可能多的匹配，即贪婪模式(greediness)。如：`zo*`能匹配*z*、*zo*、*zoo*等。`*`等价于`{0,}`。|
|+|一次、或多次，尽可能多的匹配。如：`zo+`能匹配*zo*以及*zoo*，但不能匹配*z*，`+`等价于`{1,}`。|
|?|匹配零次、一次，优先匹配一次。`(n)?ever`可以匹配*never*以及*ever*。`?`等价于`{0,1}`。|
|{n}|匹配n次。n为非负整数，大括号内不能有空格。如：`o{2}`不能匹配*word*中的*o*，但能匹配*Google*中的两个*o*。|
|{n,}|至少匹配n次，尽可能多的匹配。n为非负整数。`0{2,}`不能匹配*word*中的*o*，但能匹配*gooooogle*中的所有*o*|
|{n,m}|至少匹配n次，最多匹配m次，尽可能多的匹配。n和m均为非负整数，且n<=m。|
|*?|匹配零次、或多次。尽可能少的匹配，即懒惰模式(laziness)。|
|+?|匹配一次、或多次，尽可能少的匹配。|
|??|匹配零次、或一次，优先匹配零次。|
|{n}?|匹配n次。|
|{n,}?|至少匹配n次，但不超过整体模式匹配所需。|
|{n,m}?|匹配n至m次，尽可能少的匹配，但不少于n次。|
|*+|匹配零次、或多次。第一次遇到时，尽可能多地匹配，即使整体匹配失败，也不回溯，称为*Possessive Match*。点击查看[possessive match和贪婪匹配](https://github.com/pro648/tips/wiki/%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8Fpossessive%E3%80%81greediness%E5%92%8Claziness%E5%8C%BA%E5%88%AB)区别。|
|++|匹配一次、或多次。*Possessive Match*。|
|?+|匹配零次、或一次。*Possessive Match*。|
|{n}+|匹配n次。|
|{n,}+|至少匹配n次。*Possessive Match*。|
|{n,m}+|匹配n至m次，*Possessive Match*。|
|(pattern)|匹配*pattern*，并捕获这一匹配的捕获组，该子字符串用于向后引用。|
|(?:pattern)|匹配*pattern*但不捕获这一匹配的子字符串，也就是说这是一个不捕获匹配，不存储匹配的子字符串用于向后引用，比捕获组高效。|
|(?=pattern)|正向肯定预查（Look-ahead assertion)。在任何匹配pattern的字符串开始处匹配查找字符串。这是一个非获取匹配。如：`Windows(?=7\|8\|8.1\|10)`能匹配*Windows10*中的*Windows*，但不能匹配*Windowsxp*中的*Windows*。预查不消耗字符，也就是在一个匹配发生后，在最后一次匹配之后立即开始下一次匹配的搜索，而不是从包含预查的字符之后开始。|
|(?!pattern)|正向否定预查（ Negative look-ahead assertion)，在任何不匹配pattern的字符串开始处匹配查找字符串。这是一个非获取匹配。如：`Windows(?!7\|8\|8.1\|10)`能匹配*Windowsxp*中的*Windows*，但不能匹配*Windows10*中的*Windows*。预查不消耗字符。|
|(?<=pattern)|反向肯定预查（Look-behind assertion)，与正向肯定预查类似，只是方向相反。如：`(?<=7\|8\|8.1\|10)Windows`能匹配*7Windows*中的*Windows*，但不能匹配*xpWindows*中的*Windows*。|
|(?<!pattern)|反向否定查询（Negative Look-behind assertion），与正向否定预查类似，只是方向相反。如：`(?<!7\|8\|8.1\|10)Windows`能匹配*xpWindows*中的*Windows*，但不能匹配*7Windows*中的*Windows*。|

- 其它

|符号|描述|
|----|---|
|\n|向后引用，匹配第n个捕获组。其中，1<= 正整数n <=捕捉组总数。|
|$n|n为非负整数，向后引用第n个捕捉组，0<= n <=捕捉组总数。$后没有数字时该符号没有任何特殊含义。

- 优先级

在这些运算符同时出现时，按照下面的优先级进行操作。

|优先级|符号|
|-----|---|
|最高| \ |
|高|( )、(?: )、(?= )、[ ]|
|中|*、+、?、{n}、{n,}、{n,m}|
|低|^、$、中介字符|
|最低|\||

> 如果想要系统了解正则表达式，请查看我的另一篇文章[正则表达式NSRegularExpression](https://github.com/pro648/tips/wiki/%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8FNSRegularExpression)。

欢迎更多指正：<https://github.com/pro648/tips/wiki>