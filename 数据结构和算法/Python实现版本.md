## Python实现版本
（参考）

---
---
### 基础算法
#### 非递归实现fibonacci:

```
# 递归 O(2*n)
def fib(n):
    if n == 0 or n== 1
        return n
    else:
        return fib(n-1) + fib(n)
        
# 非递归 O(n)
def fib(n):
    if n==0 or n==1:
        return n
    else:
        a = 1
        b = 1
        while n!=b:
            a,b = b, a+b
        return b
```
### 数据结构应用
#### 链表-反转链表

#### 链表-交换成对节点

#### 判断链表是否有环

#### 栈与队列-用双队列模拟栈

#### 栈与队列-用双栈模拟队列

#### 栈与队列-判断括号字符串是否合法
```
def is_valid(s):
    stack = []
    paren_dict = {'}':'{', ')':'(', ']':'['}
    
    for i in s:
        if i not in paren_dict:
            stack.append(i)
        elif stack.pop() ！= paren_dict[i]:
            return False
        elif not stack:
            return False
    return not stack
    
```

#### [堆-最K小(大)的数 leet 703](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

### 排序
#### bubble sort

#### select

#### quick

#### merge



### 查找
#### 二分

#### 
