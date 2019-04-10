## Java实现版本
（参考算法4）

---
---
### 基础算法
#### 欧几里得算法（求最大公约数）

核心思想就是辗转相除: gcd(m, n) = gcd(m, m % n)
```java

public class GcdAlgorithms {
    public static void main(String[] args) {
        int result = gcd2(100, 10);
        System.out.println(result);
    }
//    非递归实现，循环
    private static int gcd(int m, int n) {
        while (n != 0) {
            int rem = m % n;
            m = n;
            n = rem;
        }
        return m;
    }
//    递归实现
    private static int gcd2(int m, int n) {
        if(n==0){
            return m;
        }
        int p = m%n;
        return gcd2(n, p);
    }
}
```
---

#### 判断随机整数是否为素数

注意 i 取平方减少判断次数,因为一个数不可能被大于自己完全平方根的数整除
```java
import java.util.Random;

public class numsDemo {
    public static void main(String[] args) {
        System.out.println("生成的随机整数是否为素数？" + isPrime(101));
    }
    public static boolean isPrime(int bound) {
        Random random = new Random();
        int x = random.nextInt(bound);
        if (x < 2) return false;
        else {
            for (int i = 2; i*i < x; i++) {
                if (x % i == 0) return false;
            }
            return true;
        }
    }
}
```
---
#### 牛顿迭代法（求完全平方根）
Java实现开平方的牛顿迭代法. 就是求f(x)=x^2-C的根的正根。
推导过程参考：
https://baike.baidu.com/item/牛顿迭代法
和
https://blog.csdn.net/ccnt_2012/article/details/81837154
```java
import java.util.Random;

public class numsDemo {
    public static void main(String[] args) {
        System.out.println("生成的随机浮点数的完全平方根为" + sqrt(100));
    }

    public static double sqrt(int bound) {
        Random random = new Random();
        double c = random.nextInt(bound) + random.nextDouble();

        if (c < 0) {
            return Double.NaN;
        }
        double e = 1e-15;
        double x = c;
        double y = (x + c / x) / 2;
        while (Math.abs(x - y) > e) {
            x = y;
            y = (x + c / x) / 2;
        }
        return x;
    }
}
```

---
---

### 查找
#### 二分查找
```
public class binarySearch {
    public static void main(String[] args) {
        int[] nums = new int[]{2, 3, 4, 5, 6, 7, 8, 9};
        System.out.println(search(5, nums, 0, nums.length));
        System.out.println(search2(5, nums));
    }

    //    递归版本
    public static int search(int key, int[] nums, int low, int high) {
        int mid = (low + high) / 2;
        if (nums[mid] == key) {
            return mid;
        } else if (nums[mid] < key) {
            low = mid + 1;
            return search(key, nums, low, high);
        } else if (nums[mid] > key) {
            high = mid - 1;
            return search(key, nums, low, high);
        }
        return -1;
    }

    //    非递归版本
    public static int search2(int key, int[] nums) {
        int low = 0;
        int high = nums.length;
        int mid = (low + high) / 2;
        while (low <= high) {
            mid = (low + high) / 2;
            if (nums[mid] == key) {
                System.out.println(nums[mid] == key);
                return mid;
            } else if (nums[mid] < key) {
                low = mid + 1;
            } else if (nums[mid] > key) {
                high = mid - 1;
            }
        }
        return -1;
    }
}
```
#### 暴力查找（又称白名单过滤）
此处略去

---
---

