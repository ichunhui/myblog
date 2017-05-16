---
title: 【Leetcode】224. Basic Calculator(待修改)
date: 2016-03-16 14:57:19
tags: 
- LeetCode
- 表达式求值
- 算法
categories: 算法
---


## [题目](https://leetcode.com/problems/basic-calculator/) ##
> Implement a basic calculator to evaluate a simple 
> 
> expression string.

> The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .

> You may assume that the given expression is always valid.



> Some examples:
> 
	"1 + 1" = 2
	" 2-1 + 2 " = 3
	"(1+(4+5+2)-3)+(6+8)" = 23


> Note: Do not use the eval built-in library function.

## 题目大意 ##
> 实现一个简易计算器，计算简单表达式的值。
> 
> 表达式字符串可能包含左括号（、右括号），加号 +  或者减号 -， 非负整数和空白字符。
> 
> 你可以假设给定的表达式总是有效的。
> 
> 测试样例见题目描述。
> 
> 注意：不要使用内置库函数eval

## 解题思路一 ##

> 表达式求值可以分解为下列两步完成：
> 
> 1. 中缀表达式转为后缀表达式（逆波兰表达式）
> 
> 2. 后缀表达式求值

### python代码 ###

	class Solution:
	    # @param {string} s
	    # @return {integer}
	    def calculate(self, s):
	        tokens = self.toRPN(s)
	        return self.evalRPN(tokens)
	
	    operators = ['+', '-', '*', '/']
	
	    def toRPN(self, s):
	        tokens, stack = [], []
	        number = ''
	        for c in s:
	            if c.isdigit():
	                number += c
	            else:
	                if number:
	                    tokens.append(number)
	                    number = ''
	                if c in self.operators:
	                    while len(stack) and self.getPriority(stack[-1]) >= self.getPriority(c):
	                        tokens.append(stack.pop())
	                    stack.append(c)
	                elif c == '(':
	                    stack.append(c)
	                elif c == ')':
	                    while len(stack) and stack[-1] != '(':
	                        tokens.append(stack.pop())
	                    stack.pop()
	        if number:
	            tokens.append(number)
	        while len(stack):
	            tokens.append(stack.pop())
	        return tokens
	
	    def evalRPN(self, tokens):
	        operands = []
	        for token in tokens:
	            if token in self.operators:
	                y, x = operands.pop(), operands.pop()
	                operands.append(self.getVal(x, y, token))
	            else:
	                operands.append(int(token))
	        return operands[0]
	    
	    def getVal(self, x, y, operator):
	        return {
	            '+': lambda x, y: x + y,
	            '-': lambda x, y: x - y,
	            '*': lambda x, y: x * y,
	            '/': lambda x, y: int(float(x) / y),
	        }[operator](x, y)
	
	    def getPriority(self, operator):
	        return {
	            '+' : 1,
	            '-' : 1,
	            '*' : 2,
	            '/' : 2,
	        }.get(operator, 0)

## 解题思路二 ##
> 很多人将该题转换为后缀表达式后（逆波兰表达式）求解，其实不用那么复杂。题目条件说明只有加减法和括号，由于加减法是相同顺序的，我们大可以直接把所有数顺序计算。难点在于多了括号后如何处理正负号。我们想象一下如果没有括号这题该怎们做：因为只有加减号，我们可以用一个变量sign来记录上一次的符号是加还是减，这样把每次读到的数字乘以这个sign就可以加到总的结果中了。有了括号后，整个括号内的东西可一看成一个东西，这些括号内的东西都会受到括号所在区域内的正负号影响（比如括号前面是个负号，然后括号所属的括号前面也是个负号，那该括号的符号就是正号）。但是每多一个括号，都要记录下这个括号所属的正负号，而每当一个括号结束，我们还要知道出来以后所在的括号所属的正负号。根据这个性质，我们可以使用一个栈，来记录这些括号所属的正负号。这样我们每遇到一个数，都可以根据当前符号，和所属括号的符号，计算其真实值。

### c代码 ###

	//AC - 4ms;
	int calculate(char* s)
	{
	    int* signs = (int*)malloc(sizeof(int)); //used to store the signs before opening bracket;
	    int top = -1;
	    signs[++top] = 1;
	    int sign = 1;
	    int num = 0;
	    int ret = 0; //used to store the result;
	    while(*s)
	    {
	        if(*s>='0' && *s<='9')  //collect the number between non-digits;
	        {
	            num = 10*num + *s - '0';
	        }
	        else if(*s=='+' || *s=='-') //add the previous number and reset sign;
	        {
	            ret += signs[top]*sign*num;
	            num = 0;
	            sign = (*s=='+'? 1 : -1);
	        }
	        else if(*s == '(') //store the sign before opening bracket;
	        {
	            signs = (int*)realloc(signs, sizeof(int)*(top+2));
	            signs[top+1] = sign*signs[top]; //to avoid evaluation sequence problem, moving top++ to another statement;
	            top++;
	            sign = 1;
	        }
	        else if(*s == ')') //add the previous number and delete a sign for this pair of brackets;
	        {
	            ret += signs[top--]*sign*num;
	            num = 0;
	        }
	        s++;
	    }
	    if(num) //if there is still number left;
	        ret += signs[top]*sign*num;
	    return ret;
	}



## 参考资料 ##
1. [[LeetCode]Basic Calculator](http://bookshadow.com/weblog/2015/06/09/leetcode-basic-calculator/)
2. [[Leetcode] Basic Calculator 基本计算器](https://segmentfault.com/a/1190000003796804)
3. [A simple, clean solution accepted as best submission in C, well-explained](https://leetcode.com/discuss/89418/simple-clean-solution-accepted-submission-well-explained)