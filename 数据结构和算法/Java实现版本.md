## Java实现版本
（参考算法4）

#### 欧几里得算法（求最大公约数）

核心思想就是辗转相除: gcd(m, n) = gcd(m, m % n)
```

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

注意i取平方减少判断次数,因为一个数不可能被大于自己完全平方根的数整除
```
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


