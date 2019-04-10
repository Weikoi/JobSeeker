## Python实现版本
（参考）

---
---
### 基础算法
#### 非递归实现fibonacci:

```python
# 递归 O(2**n),可以考虑使用lru_cache()装饰器，将会大大提高效率，利用的是记忆化思想
def fib(n):
    if n == 0 or n== 1:
        return n
    else:
        return fib(n-1) + fib(n)
        
# 非递归 O(n)， 即递推思想，由下而上，DP思想
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

#### 链表-判断链表是否有环

#### 栈与队列-用双队列模拟栈

#### 栈与队列-用双栈模拟队列

#### 栈与队列-判断括号字符串是否合法
```python
def is_valid(s):
    stack = []
    paren_dict = {'}':'{', ')':'(', ']':'['}
    
    for i in s:
        if i not in paren_dict:
            stack.append(i)
        elif stack.pop() != paren_dict[i]:
            return False
        elif not stack:
            return False
    return not stack
    
```

#### [堆-最K小(大)的数 leet 703]("https://leetcode.com/problems/kth-largest-element-in-a-stream/")

#### [堆-滑动窗口最大值 leet 239]

#### hash-

### 排序
#### bubble sort

#### insert sort

#### select

#### quick

#### heap

#### merge

### 查找
#### 二分

### 回溯、贪心、DP
