---
title: leetcode题解-20.有效的括号
comments: true
categories: leetcode
tags:
  - leetcode
abbrlink: 62434
date: 2019-03-17 15:20:00
---
## 题目
leetcode 20. 有效的括号 堆栈
<!--more-->
## 有效的括号
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

示例 1:

输入: "()"
输出: true
示例 2:

输入: "()[]{}"
输出: true
示例 3:

输入: "(]"
输出: false
示例 4:

输入: "([)]"
输出: false
示例 5:

输入: "{[]}"
输出: true

## 解决方法
这道题就非常适合用我们之前介绍过的栈（[栈-C语言实现](https://www.yanbinghu.com/2019/03/16/31765.html)）这种数据结构来解决。怎么处理呢？我们发现括号都是成对的，如果成对的括号，并且括号中间的括号也可以成对，那么整个字符串就是有效的。比如说：
```
"{[]}"
```
在{}之间的[]是成对的，它们可以互相抵消掉，之后{}也是成对的。我们可以利用栈，遍历整个字符串，遇到左括号，入栈，遇到右括号，检查栈中是否是左括号，如果是，那么就将左括号出栈，右括号也不入栈。如果字符串的是合法的，那么最终栈为空，否则栈不为空。

我们来看一个例子：
```
“{[]}”
```
遇见左大括号，入栈：

|栈顶|||||
|--|--|--|--|--|
|{|||||

遇见左方括号，入栈：

||栈顶||||
|--|--|--|--|--|
|{|[||||

遇见右方括号，检查栈顶是左方括号，出栈：

|栈顶|||||
|--|--|--|--|--|
|{|||||

遇见右大括号，检查栈顶是左大括号，出栈：

||||||
|--|--|--|--|--|
||||||

此时扫描完毕，并且栈为空，因此该字符串合法。

我们再来看一个非法的例子：
```
”([)]“
```

首先遇到左小括号，入栈：

|栈顶|||||
|--|--|--|--|--|
|(|||||

遇到左方括号,入栈：

||栈顶||||
|--|--|--|--|--|
|(|[||||

遇到右小括号，检查栈顶是否有左小括号，发现没有，入栈(其实这个时候就可以判断字符串不合法了)：

|||栈顶|||
|--|--|--|--|--|
|(|[|)|||

遇到右中括号，检查栈顶是否有左方括号，发现没有，入栈：

||||栈顶||
|--|--|--|--|--|
|(|[|)|]||

扫描完成后，发现栈不为空，因此字符串不合法。

## 代码实现
在实现代码的时候，需要注意以下几点：
+ 遇见第一个右括号无匹配时即退出
+ 由于输入字符串长度可能较大，因此不适合使用静态数组
+ 判断是否有左括号前检查栈是否为空

```c
bool isValid(char* s) {
    if(NULL == s)
        return false;
    int len = strlen(s);
    /*使用数组作为栈，申请内存*/
    char *stack = (char*)malloc(len * sizeof(char));
    if(NULL == stack)
        return false;
    int topOfStack = -1;
    while(0 != *s)
    {
        /*遇见左括号入栈*/
        if('{' == *s || '[' == *s || '(' == *s)
        {
            //printf("push %c to stack\n",*s);
            topOfStack++;
            stack[topOfStack] = *s;
        }
        /*如果此时栈为空，说明之前都没有左括号，因此肯定不匹配，直接退出*/
        else if(topOfStack == -1)
        {
            topOfStack++;
            break;
        }
        /*遇见右括号，栈顶是左括号，出栈*/
        else if(('}' == *s && '{' == stack[topOfStack] )||
        (')' == *s && '(' == stack[topOfStack]) ||
        ( ']' == *s && '[' == stack[topOfStack]))
        {
            topOfStack--;
        }
       /*右括号，并且栈顶不是左括号，肯定不匹配，直接退出*/
        else if('}' == *s || ']' == *s || ')' == *s )
        {
            topOfStack++;
            break;
        }
        s++;
    }
    free(stack);
    /*判断栈是否为空*/
    return topOfStack == -1;
    
}
```

总结
本文利用栈结构在O(n)时间复杂度,O(n)空间复杂度解决了括号匹配问题。你还有什么解法？欢迎留言。



