# e.同余定理

## 定义

 如果a和b除以一个m余数相同，则称a与b同余，记作a≡b(mod m)。 

## 基本定理

1、(a + b) % p = (a % p + b % p) % p

2、(a - b) % p = (a % p - b % p) % p

3、(a * b) % p = (a % p * b % p) % p

4、 a^b % p = ((a % p)^b) % p

5、((a+b) % p + c) % p = (a + (b+c) % p) % p

6、((a*b) % p * c)% p = (a * (b*c) % p) % p

7、(a + b) % p = (b+a) % p

8、(a * b) % p = (b * a) % p

9、((a +b)% p * c) % p = ((a * c) % p + (b * c) % p) % p

10、若a≡b (% p)，则对于任意的c，都有(a + c) ≡ (b + c) (%p)

11、若a≡b (% p)，则对于任意的c，都有(a * c) ≡ (b * c) (%p)

12、若a≡b (% p)，c≡d (% p)，则 (a + c) ≡ (b + d) (%p)，(a - c) ≡ (b - d) (%p)，(a * c) ≡ (b * d) (%p)，(a / c) ≡ (b / d) (%p)

13、若a≡b (% p)，则对于任意的c，都有ac≡ bc (%p)

## 应用

1. **同余定理的加法乘法应用**

- (a + b) % m = (a % m + b % m) % m

```
设 a = k1 * m + r1，b = k2 * m + r2
则 (a + b) % m = ((k1 * m + r1) + (k2 * m + r2)) % m
               = ((k1 + k2) * m + (r1 + r2)) % m
               = (r1 + r2) % m
               = (a % m + b % m) % m
所以 (a + b) % m = (a % m + b % m) % m12345
```

- (a * b) % m = ((a % m) * (b % m)) % m

```undefined
设 a = k1 * m + r1，b = k2 * m + r2
则 (a * b) % m = ((k1 * m + r1) * (k2 * m + r2)) % m
               = (k1 * k2 * m^2 + (k1 * r2 + k2 * r1) * m + r1 * r2) % m
               = (r1 * r2) % m
               = ((a % m) * (b % m)) % m
所以 (a * b) % m = ((a % m) * (b % m)) % m12345
```

2. **高精度取模**

- 高精度对单精度取模

一个高精度数对一个数取余，可以把高精度数看成各位数的权值与个位数乘积的和。

比如1234 = ((1 * 10 + 2) * 10 + 3) * 10 + 4，对这个数进行取余运算就是上面基本加和乘的应用。

```C++
#include<iostream>
#include<string>
using namespace std;
 
int main()
{
    string a;
    int b;
    cin >> a >> b;
    int len = a.length();
    int ans = 0;
    for(int i = 0; i < len; i++)
    {
        ans = (ans * 10 + a[i] - ‘0’) % b;
    }
    cout << ans << endl;
    return 0;
}
```



- **快速幂取模**

将幂拆解为多个底数的平方次的积，如果指数为偶数，把指数除以2，并让底数的平方次取余，如果指数为奇数，就把多出来的底数记录下来，再执行偶数次的操作。

```C++
#include<iostream>
using namespace std;
 
int PowerMod(int a, int b, int c)
{
    int ans = 1;
    a = a % c;
    while(b > 0)
    {
        if(b&1)
        {
            ans *= (a % c);
        }
        b >>= 1;
        a = (a * a) % c;
    }
    ans %= c;
    return ans;
}
 
int main()
{
    int a, b, c;
    cin >> a >> b >> c;
    cout << PowerMod(a, b, c) << endl;
    return 0;
}
```