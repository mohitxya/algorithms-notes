## Table of Contents
- [Binary Exponentiation](#binary%20exponentiation)
- [Sort Stack (recursion)](#sort-stack-with-recursion)

### Binary Exponentiation
- If we are calculating $2^{10}$, instead of multiplying 2 10 times. Use this algorithm.
>For $a^{n}$:
>if n is even: `a x a` to the power `n/2`. Continue this till we get 0 as the exponent.
>If n is odd: `ans = ans x a` and `n-=1`.

```cpp
double myPow(double x, int n) {
    	double ans=1.0;
        long long nn=n;
        if(n<0) nn=-nn;
        while(nn)
        {
        	if(nn%2)
        	{
        		ans=ans*x;
        		nn=nn-1;
        	}
        	else
        	{
        		x=x*x;
        		nn=nn/2;
        	}
        }

        if(n<0) ans=(double) 1.0/(double) ans;
        return ans;

    }
```
Example: $2^{10}$ 
1. ans=1.0, nn=10, x=4, nn=5
2. ans=4.0, nn=4
3. ans=4.0, nn=2, x=16
4. ans=4.0, nn=1, x= 256
5. ans=4.0*256=1024.0, nn=0

**THE FINAL ANSWER:** 1024

More Optimal Version:
```cpp
long long fast_power(long long base, long long exp) {
    long long result = 1;
    while (exp > 0) {
        if (exp % 2 == 1)
            result = result * base;   // no % mod
        base = base * base;           // no % mod
        exp /= 2;
    }
    return result;
}
```

Uses shifting (source; cp algorithms):
```cpp
long long binpow(long long a, long long b) {
    long long res = 1;
    while (b > 0) {
        if (b & 1)
            res = res * a;
        a = a * a;
        b >>= 1;
    }
    return res;
}
```

### Sort Stack with Recursion
- sample content
- sample content 2