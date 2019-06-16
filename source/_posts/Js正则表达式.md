---
title: Js正则表达式
date: 2019-05-10 17:21:10
tags: javascript
---
[doc]

----

# JavaScript 正则表达式

![](http://www.phpxs.com/uploads/201808/08/15336929641.JPG)

---

**正则表达式**（Regular Expression），在代码中常简写为**regex**、**regexp**或**RE**），使用单个字符串来描述、匹配一系列符合某个句法规则的字符串搜索模式。可用于所有<u>文本搜索</u>和<u>文本替换</u>的操作。应用领域广泛，包含数据验证（表单验证），数据的删除替换等

### 基础

- ####  创建一个正则表达式


```javascript
// 使用一个正则表达式字面量，其由包含在斜杠之间的模式组成，
const regex = patt=/pattern/modifiers;

// 调用RegExp对象的构造函数
var regex = new RegExp(pattern,modifiers);

- pattern（模式） 描述了表达式的模式
- modifiers(修饰符) 用于指定全局匹配、区分大小写的匹配和多行匹配
```

- #### 基础语法

  1. **正则表达式中的特殊字符**

     - 边界匹配 —— ^ and $

     | 表达式 | 描述                                    |
     | ------ | --------------------------------------- |
     | n$     | 匹配任何结尾为 n 的字符串。             |
     | ^n     | 匹配任何开头为 n 的字符串。             |
     | ^n m$  | 匹配任何开头为 n 结尾为 m 的的 字符串。 |

     - 方括号——查找某个范围内的字符

     | 表达式             | 描述                               | 例                                                           |
     | :----------------- | :--------------------------------- | ------------------------------------------------------------ |
     | [abc]              | 查找方括号之间的任何字符。         | [例子](https://www.runoob.com/jsref/jsref-regexp-charset.html) |
     | [^abc]             | 查找任何不在方括号之间的字符。     |                                                              |
     | [0-9]              | 查找任何从 0 至 9 的数字。         |                                                              |
     | [a-z]              | 查找任何从小写 a 到小写 z 的字符。 |                                                              |
     | [A-Z]              | 查找任何从大写 A 到大写 Z 的字符。 |                                                              |
     | [A-z]              | 查找任何从大写 A 到小写 z 的字符。 |                                                              |
     | (red\|blue\|green) | 查找任何指定的选项。               |                                                              |

     -  量词——* + ? and {}

     | 量词  | **描述**         |              |
     | ----- | ---------------- | ------------ |
     | *     | 重复零次或更多次 | 等价于`{0,}` |
     | +     | 重复一次或更多次 | 等价于`{1,}` |
     | ?     | 重复零次或一次   | 等价于{0,1}  |
     | {n}   | 重复n次          |              |
     | {n,}  | 重复n次或更多次  |              |
     | {n,m} | 重复n到m次       |              |

     - 常用元字符（Metacharacter）

     | 元字符                                                       | 描述                                                         |
     | :----------------------------------------------------------- | :----------------------------------------------------------- |
     | [.](https://www.runoob.com/jsref/jsref-regexp-dot.html)      | 查找单个字符，除了换行和行结束符。                           |
     | [\w](https://www.runoob.com/jsref/jsref-regexp-wordchar.html) | 查找单词字符。（a-z、A-Z、0-9，以及下划线, 包含 _ (下划线) 字符。） |
     | \s                                                           | 匹配任意的空白符(包括tabs、换行)                             |
     | \d                                                           | 匹配数字                                                     |
     | [\b](https://www.runoob.com/jsref/jsref-regexp-begin.html)   | 匹配单词边界。                                               |
     | [\n](https://www.runoob.com/jsref/jsref-regexp-newline.html) | 查找换行符。                                                 |
     | \t                                                           | 查找制表符。                                                 |
     | \r                                                           | 查找回车符。                                                 |

  2. | 代码/语法 | 说明(代表反义)                             |
     | --------- | ------------------------------------------ |
     | \W        | 匹配任意不是字母，数字，下划线，汉字的字符 |
     | \S        | 匹配任意不是空白符的字符                   |
     | \D        | 匹配任意非数字的字符                       |
     | \B        | 匹配不是单词开头或结束的位置               |
     | [^x]      | 匹配除了x以外的任意字符                    |
     | [^aeiou]  | 匹配除了aeiou这几个字母以外的任意字符      |

     **修正符**（modifiers）

     | **修饰符** | **描述**                                                 |      |
     | ---------- | -------------------------------------------------------- | ---- |
     | i          | 执行对大小写不敏感的匹配。                               |      |
     | g          | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。 |      |
     | m          | 执行多行匹配。                                           |      |

  3. **正则表达式中的方法**

     - **test()**：在字符串中查找符合正则的内容(**布尔值**)，若查**找到返回true**,反之返回false.

     ```javascript
     var str = '123456789';
     var re = /\d/;      //  \d代表数字
     if( re.test(str) ){   // 返回true,代表在字符串中找到了非数字。
         alert('不全是数字');
     }else{
         alert('全是数字');
     }
     ```

     -  **search()**：在字符串搜索符合正则的内容，搜索到就返回**出现的位置**（从0开始，如果匹配的不只是一个字母，那只会返回第一个字母的位置）， 如果搜索失败就返回 -1

     ```javascript
     var str="Visit Runooob";
     var patt1=/\bvi/gi; // 全局匹配 vi开头的单词 不区分大小写
     document.write(str.search(patt1)); // 0

     var patt2=/\bru/gi; // 全局匹配 ru开头的单词 不区分大小写
     document.write(str.search(patt2)); // 6

     var patt3=/\b/gi; // 全局匹配 单词 不区分大小写
     document.write(str.search(patt3)); // 0

     var patt4=/\bi/gi; // 全局匹配 i开头的单词 不区分大小写
     document.write(str.search(patt4)); // -1
     ```

     - **match()**： 在字符串中搜索复合规则的内容，搜索成功就返回内容，格式为**数组**，失败就返回null。

     ```javascript
     var str = 'asdfw723sdk54hask33dasdwd889';
     var regex1 =  /\d/g
     document.write(str.match(regex1)); // 7,2,3,5,4,3,3,8,8,9

     var str = 'asdfw723sdk54hask33dasdwd889';
     var regex2 =  /\d+/g
     document.write(str.match(regex2)); // 723,54,33,889

     var str = 'asdfw723sdk54hask33dasdwd889';
     var regex3 =  /\d{1,2}/g
     document.write(str.match(regex3)); // 72,3,54,33,88,9

     var str = 'asdfw723sdk54hask33dasdwd889';
     var regex4 =  /\d{2}/g
     document.write(str.match(regex4)); // 72,54,33,88

     // 贪婪匹配和惰性匹配

     // 贪婪匹配
     var regex = /\d{2,5}/g; // 数字连续出现2到5次。会匹配2位、3位、4位、5位连续数字
     var string = "123 1234 12345 123456";
     document.write( string.match(regex) );
     // => ["123", "1234", "12345", "12345"]  尽可能多的匹配 需要2至5个字符 ，’123‘ 字符只有3个 匹配3个； ’123456‘字符有6个 我最多只需要5个，只匹配5个， 只要在2-5范围内，越多越好

     // 惰性匹配，就是尽可能少的匹配 （量词后面加个问号）

     var regex = /\d{2,5}?/g;
     var string = "123 1234 12345 123456";
     console.log( string.match(regex) );
     // => ["12", "12", "34", "12", "34", "12", "34", "56"] 需要的范围在2-5内，惰性匹配 尽可能少的匹配，满足2个字符的条件，就不再往下匹配 '123'大于两个字符 匹配12 之后就不匹配123


     ```

     - **replace()** :查找符合正则的字符串，就**替换成对应的字符串**。返回替换后的内容。

       **可以用于敏感词过滤**

     ```javascript
     var string = "fuck，sasdfasf,shit";
     var regex = /fuck|shit/g; // 用|（管道符）分隔，表示其中任何之一。
     document.write( string.replace(regex,'***') ); // ***，sasdfasf,***

     // 实现几个字对应几个*，我们可以用回调函数实现
     var string = "fuck，sasdfasf,shit";
     var regex = /fuck|shit/g; // 用|（管道符）分隔，表示其中任何之一。
     document.write( string.replace(regex,function(str){zx
       return "*".repeat(str.length)
     }) );
     ```

     - **exec()方法**：和match方法一样，搜索符合规则的内容，并返回返回一个结果数组或null

       > 正则表达式的方法，而不是字符串的方法，它的参数才是字符串

       > 执行exec方法的正则表达式没有**分组**（没有括号括起来的内容），那么如果有匹配，他将返回一个只有一个元素的数组，这个数组唯一的元素就是该正则表达式匹配的第一个串;如果没有匹配则返回null。

     ```javascript
     var str= "cat2,hat8" ;
     var p=/at\d/g; //注意g属性
     // 没有分组 exec永远只返回第一个匹配
     document.write(p.exec(str)) // at2
     //match在正则指定了g属性的时候，会返回所有匹配。
     document.write(str.match(p)) // at2,at8

     // 使用（） 对at 分组
     var str= "cat2,hat8" ;
     var p=/(at)\d/;
     document.write(p.exec(str)) // at2,at  将匹配第一个可以匹配的字串“at2”,正则表达式还包含一个(at)的分组 ，返回的数组【at2,at】

     var str= "cat2,hat8" ;
     var p=/(at)(\d)/;
     document.write(p.exec(str)) // at2,at  将匹配第一个可以匹配的字串“at2”,正则表达式还包含(at)(\d)的分组 ，返回的数组【at2,at,2】

     var someText="web2.0 .net2.0";
     var pattern=/(\w+)(\d)\.(\d)/g;
     var outCome_exec=pattern.exec(someText);
     var outCome_matc=someText.match(pattern);

     document.write(outCome_exec); // web2.0,web,2,0

     document.write(outCome_matc); // web2.0,net2.0
     ```

     > match是返回所有匹配的字符串合成的数组，但是正则表达式必须**指定全局g属性**才能**返回所有匹配**，**不指定g属性**则会**返回**一个**只有一个元素的数组**。
     >
     > exec**永远返回与第一个匹配**相关的信息，其返回数组包括第一个匹配的字串，所有分组的反向引用。

     - 分组 ——使用（）分组 ，分组会捕获它们匹配到的数据，以便后续引用（exec() 方法）。也称他们是捕获型分组，(?:)表示非捕获分组

     ```javascript
     var p=/(at)(\d)/; // at 表示一个分组

     (?:)表示非捕获分组
     ```



     - 反向引用——引用前面的分组

       > 可以在正则本身里引用分组，只能引用之前出现的分组，正则里引用了不存在的分组时，此时正则不会报错，只是匹配反向引用的字符本身。

     ```javascript
     var regex = /(1)(2)(3)(#)_\4+/;
     var string = "123#_######" // 匹配123#_ 对第四个分组（#）匹配一次或更多次
     document.write(regex.test(string)) // true

     var regex = /\1\2\3\4/;
     document.write( regex.test("\1\2\3\4") );  //匹配第一个分组没有 ，用'\1' 本身去匹配"\1\2\3\4"// true
     ```



  4. **运算符的优先级****

     正则表达式从左到右进行计算，并遵循优先级顺序，**相同优先级**的**从左到右**进行运算，**不同优先级**的运算**先高后低**

     从最高到最低优先级顺序：

     | 运算符                      | 描述                                                         |
     | :-------------------------- | :----------------------------------------------------------- |
     | \                           | 转义符                                                       |
     | (), (?:), (?=), []          | 圆括号和方括号                                               |
     | *, +, ?, {n}, {n,}, {n,m}   | 限定符                                                       |
     | ^, $, \任何元字符、任何字符 | 定位点和序列（即：位置和顺序）                               |
     | \|                          | 替换，"或"操作 字符具有高于替换运算符的优先级，使得"m\|food"匹配"m"或"food"。若要匹配"mood"或"food"，请使用括号创建子表达式，从而产生"(m\|f)ood"。 |

  5. **前瞻和后瞻**

     ```javascript
     前瞻：
     exp1(?=exp2) 查找exp2前面的exp1
     var regex = /\w{2}(?=_)/g;
     document.write( "12d12_5hs_".match(regex)); // 12,hs

     后顾：
     (?<=exp2)exp1 查找exp2后面的exp1
     var regex = /(?<=_)\w{2}/g;
     document.write( "12d12_5hs_asdf".match(regex)); // 5h,as

     负前瞻：
     exp1(?!exp2) 查找后面不是exp2的exp1
     var regex = /_(?!5h)/g; //
     document.write( "12d12_5h".match(regex));  // null
     负后顾：(没有理解)
     (?<!=exp2)exp1 查找前面不是exp2的exp1
     var regex = /(?<!='_5h)2/g;
     // 按理解应该打印 null 实际打印2
     document.write( "_5h2".match(regex));  // 2
     ```



### 正则表达式在线测试工具

- #### Windows测试工具Regexbuddy
	- ##### 工具获取：[正则表达式测试工具RegexBuddy v4.5.0](http://www.zjmainstay.cn/download/category/4-regexp?download=26%3aregexbuddy-v4-5-0)

	- ##### 工具教程：[正则表达式工具RegexBuddy使用教程](http://zjmainstay.cn/regex-tool-regexbuddy)

- #### Mac测试工具[Regex101](https://regex101.com/)

  ![](https://note.youdao.com/yws/public/resource/80719707d48b241ad1ae34ab7e29ff67/xmlnote/WEBRESOURCE39cebe3cdb8fe6b9bdda8d7606fbba6b/2289)

#### 参考链接

【1】[MDN Web doc](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions)

【2】[过目不忘的js正则表达式](http://www.cnblogs.com/moqing/p/5665126.html)

【3】[JavaScript 正则表达式迷你书](https://juejin.im/post/5965943ff265da6c30653879)

【4】[正则表达式工具](http://www.zjmainstay.cn/regex-tool-mac-app)

【5】[[Angus](http://blog.yangyong.io/)](http://blog.yangyong.io/2019/05/10/javascript/正则表达式—A quick cheatsheet by examples/)
