# Python基础

(参考Python Cookbook和Fluent Python)

---
---
### Python常识

#### Python 中的深浅拷贝？


#### Python中的装饰器？

例如：
```python
import time

def metric(fn):
    def wrapper(*args):
        start = time.time()
        res = fn(*args)
        end = time.time()
        print('%s executed in %s s' % (fn.__name__, round(end - start, 3)))
        return res

    return wrapper


def log(fn):
    def wrapper(*args, **kwgs):
        start = time.time()
        res = fn(*args, **kwgs)
        end = time.time()
        with open("log.txt", 'a') as f:
            line = "excute in %f s   @   " % (end - start) + time.strftime('%Y-%m-%d, %H:%M:%S\n', time.localtime(time.time()))
            f.write(line)
        print("time using:", end - start)
        return res
    return wrapper

```

#### Python中的元类？


#### 
