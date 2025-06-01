## Table of Contents
- [Binary Exponentiation](#binary-exponentiation)
- [Sort Stack (recursion)](#sort-stack-with-recursion)
- [Reverse Stack](#reverse-a-stack)
- [Binary Substrings](#generate-binary-substrings)

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
```cpp
#include <iostream>
#include <stack>
using namespace std;

void sortedInsert(stack<int> &s, int x) {
    if (s.empty() || x > s.top()) {
        s.push(x);
        return;
    }
    int temp = s.top();
    s.pop();
    sortedInsert(s, x);
    s.push(temp);
}

void sort(stack<int> &s) {
    if (!s.empty()) {
        int x = s.top();
        s.pop();
        sort(s);
        sortedInsert(s, x);
    }
}

int main() {
    stack<int> s;

    s.push(11);
    s.push(2);
    s.push(32);
    s.push(3);
    s.push(41);

    sort(s);

    while (!s.empty()) {
        cout << s.top() << " ";
        s.pop();
    }
    cout << endl;

    return 0;
}
```

**Explanation:**
- We use two functions here `sort()` & `sortedInsert()`.
- `sort()`: Recursively removes the topmost element of the stack and inserts it in it's correct position.
### Reverse a Stack
```cpp
void insertAtBottom(stack<int> &s, int x) {

    if (s.empty()) {

        s.push(x);

        return;

    }

    int top = s.top();

    s.pop();

    insertAtBottom(s, x);

    s.push(top);

}

void reverseStack(stack<int> &stack) {

    if (stack.empty()) return;

    int top = stack.top();

    stack.pop();

    reverseStack(stack);

    insertAtBottom(stack, top);

}
```

**Dry run:**
top->bottom: 1 2 3 4
1. top=1, reverseStack[2,3,4]
2. top=2, reverseStack[3,4]
3. top=3, reverseStack[4]
4. top=4, reverseStack[] now it returns. So we can proceed to the next step of insertAtBottom([],4)
5. so, since it's empty: [4], in previous function call top value was 3. and variables are local to function calls.
6. insertAtBottom([4],3)
7. within this function: top=4, popped [], [4,3]
8. now, insertAtBottom([4,3],2)
9. top=4, [3], insertAtBottom([3],2), --> [4,3,2]
10. so on finally we get: [4,3,2,1]

### Generate Binary Substrings
```cpp
#include <iostream>
using namespace std;

void generateBinary(string current, int n) {
    // Base case
    if (current.length() == n) {
        cout << current << endl;
        return;
    }

    // Recursive case
    generateBinary(current + '0', n);
    generateBinary(current + '1', n);
}

int main() {
    int n = 3; // length of binary strings
    generateBinary("", n);
    return 0;
}

```
- Generates: 00,01,10,11. 
- We can add conditions as well. (If the problem requires no consecutive 0s to be together)
