---
layout: post
title: "正则表达式匹配罗马数字"
date: 2015-02-15 12:23
comments: true
categories: python
---

最近开始重新学Python，做完了[hackerrank](https://www.hackerrank.com/) python tutorial的题目，基本语法算是掌握了。

最后AC的一道题就是[正则匹配罗马数字](https://www.hackerrank.com/challenges/regex-2-validate-a-roman-number)。正则语法看过很多次，但感觉一直没有掌握，这次总算实战了一下，感觉正则匹配罗马数字
是学习基本正则的一个很好的case，stackoverflow也有与这相关的问题。在做这道题的时候，我也在在sof上面提了
一个[问题](http://stackoverflow.com/questions/28381287/whats-the-difference-between-regular-pattern-abcd-and-abcd).

先分析一下题目：

要求是匹配[1~3999]之间的所有罗马数字，匹配则输出True,不匹配则输出False.罗马数字由以下几个字母表示：

* I, 1, 最多出现3次
* V, 5, 不能重复出现
* X, 10, 最多出现3次
* L, 50, 不能重复出现
* C, 100, 最多出现3次
* D, 500, 不能重复出现
* M, 1000, 最多出现3次
 
只需按照1~10、10~100等分段表示罗马数字，然后由低到高组合所有段，即可得到匹配的正则表达式。
<pre>
[1,10): IX|IV|V|V?I{1,3}
		V?I{1,3}: 1~3, 6~8 
	 	IV: 4 
 	    IX: 9
 	    V: 5
 	    合并4与5得到 I?V 
 	    => IX|I?V|V?I{1,3}

[10, 100): XC|XL|L|L?X{1,3}
		L?X(1,3): 10~30， 60~80
		XL: 40
		XC: 90
		L: 50
		合并40与50得到 X?L
		=> XC|X?L|L?X{1,3}
同理可得
	[100~1000): CM|C?D|D?C{1,3}
	[1000~4000): M{1,3}

综上：得到M{1,3}?|(CM|C?D|D?C{1,3})?|(XC|XL|L|L?X{1,3})?|(IX|I?V|V?I{1,3})?
      通过？,来达到四位数的组合，但是可能会出现空串的情况
      加上前向断言(?=.)，防止空串出现
最后得到：^(?=.)M{1,3}?(CM|C?D|D?C{1,3})?(XC|XL|L|L?X{1,3})?(IX|I?V|V?I{1,3})?$
</pre>

总结：

   ^ab|cd$: 匹配以ab开头或以cd结尾的字符串

   ^(ab|cd)$: 匹配ab或cd， 不匹配abcd

   (?=sth): look ahead assert前向断言, 匹配满足括号中正则的前面一个位置，不消耗字符,意味匹配了括号中的字符继续作为输入匹配后续正则.

   比如a(?=b): 匹配ab中的a   
       (?=.)a: 只能匹配a

   (?:): not catch group，匹配括号正则的字符串会成为一个group被捕捉，捕捉group会降低匹配效率。对于无需
   捕捉group的正则，应该将每个()改为(?:).比如上面的罗马数字匹配正则就可以改为

   ^(?=.)M{1,3}?(?:CM|C?D|D?C{1,3})?(?:XC|XL|L|L?X{1,3})?(?:IX|I?V|V?I{1,3})?$













