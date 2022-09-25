# Algorithms

This website will contain the list of algorithms

## Index
1) Dynamic Programming

### Dynamic Programming

#### Fibonacci Sequence
##### Variable swapping implmentation

```cpp
#include<iostream>

using namespace std;

int main()
{
    long long int n = 30;
    
    long long int a = 0;
    long long int b = 1;
    long long int c = 0;

    for( long long int i = 2; i <= n; i++ )
    {
        c = a + b;
        a = b;
        b = c;
    }

    cout << "The nth Fibonacci number is:" << c;

    return 0;
}
```
