# 算法记录

## 目录
<!-- TOC -->
- [模板](###模板)
    - [快速乘法](####快速乘法)
    - [快速幂](####快速幂)

<!-- /TOC -->

### 模板

#### 快速乘法

快速乘法相比快速幂使用频率小很多，因为语言本身就有乘法这个运算符。因此最大的用途便是取模时避免溢出。

例：

计算 $M\times N\mod P$ 

为了避免溢出，我们常常采用的方法是利用分配律将式子转换为:  
$(M\mod P) \times (N\mod P)\mod P$  
因为一个值模P后其范围为0到P-1，只要  $P\times P$  不超过数据上限即可。

```c++
#include <cstdlib>

long long quickmul(long long a, long long b){
    bool isNegative = false;
    if ((a > 0 && b < 0) || (a < 0 && b > 0)) {
        isNegative = true;
    }

    // 两个数转换为正整数
    a = std::llabs(a);
    b = std::llabs(b);

    long long ans = 0;
    while(b){
        if(b&1) ans = ans+a;
        a = a<<1;
        b = b>>1;
    }
    return isNegative ? -ans : ans;
}
```

#### 快速幂
虽然math库中有pow函数，但是因为精度问题使用的不是很多，所以在涉及取模时广泛应用的是快速幂的模板。

```c++
long long quickpow(long long base, long long power){
    long long ans = 1;

    while(power){
        if(power&1) ans = ans * base;
        base = base*base;
        power >>= 1;
    }

    return ans;
}
```

以$7^5$为例:  
将5转换为二进制，  $5=(101)_2=1\times2^2+0\times2^1+1\times2^0=4+1$  
因此，  $7^5=7^{2^2+2^0}=7^{2^2}\times7^{2^0}=7^4\times7^1$  
这样转化后运算次数将大大减少  
只要令一个数从13开始不断乘自身（即平方），然后判断需不需要乘到答案上
