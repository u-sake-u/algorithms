# Fibonacci Sequence

***Reccurence Relation:*** F( n ) = F( n - 1 ) + F( n - 2 )

## 1) Variable Swap Implmentation

```cpp
#include<iostream>

using namespace std;

int main()
{
    //input
    long long int n = 30;
    
    //base values
    long long int a = 0;
    long long int b = 1;
    long long int c = 0;

    //iteration
    for( long long int i = 2; i <= n; i++ )
    {
        c = a + b;
        a = b;
        b = c;
    }

    //parsing
    cout << "The nth Fibonacci number is:" << c;

    return 0;
}
```

***Complexities:***

- *Time*: O( n )
- *Space*: O( 1 )

## Simple Recursion

```cpp
#include<iostream>

using namespace std;

long long int fib( long long int n )
{
    //Base Cases
    if( n == 0 )
    {
        return 0;
    }
    if( n == 1 )
    {
        return 1;
    }

    //Recursion Step
    return fib( n - 1 ) + fib( n - 2 );
}

int main()
{
    //input
    long long int n = 30;
    
    //function call
    long long int result = fib( n ); 

    //parsing
    cout << result;
}
```

***Issues:***

- works for small test cases: 1, 2, 5, 10
- stack overflows or program takes too long for large test cases: 60, 90

***Complexities:***

- *Time*: O ( 2<sup>n</sup> )
- *Space*: O ( n )

## Recursion + Memoization = Dynamic Programming - Top Down Approach

```cpp
#include<iostream>
#include<map>

using namespace std;

long long int fib( long long int n, map< int, int > &holder )
{
    //memoization look-up
    if( holder.find( n ) != holder.end() )
    {
        return holder[n];
    }

    //base case
    if( n == 0 )
    {
        return 0;
    }
    if( n == 1 )
    {
        return 1;
    }

    //recursion call
    long long int value = fib( n - 1, holder ) + fib( n - 2, holder );
    
    //memoization 
    holder.insert( { n, value } );

    return holder[n];
}

int main()
{
    //input
    long long int n = 40;

    //map object
    map< int, int > holder;

    //function call
    long long int result = fib( n, holder );

    //parsing
    cout << result;

    return 0;
}
```

***Issues***

- No issues

***Complexities:***

- *Time*: O ( 2n ) &rarr; O ( n )
- *Space*: O ( n )

## Iteration + Tabulation = Dynamic Programming - Bottom Up Approach Aka Table Filling

```cpp
#include<iostream>
#include<vector>

using namespace std;

int main()
{
    //input
    long long int n = 30;

    //DP table
    vector< long long int > holder( n + 1, 0 );

    //base case
    holder[ 0 ] = 0;
    if( n >= 1 )
    { 
        holder[ 1 ] = 1;
    }
    
    //iteration logic
    for( int i = 0; i <= n; i++ )
    {
        if( ( i + 1 ) <= n ) 
        {
            holder[ i + 1 ] = holder[ i + 1 ] + holder[ i ];
        }

        if( ( i + 2 ) <= n )
        {
            holder[ i + 2 ] = holder[ i + 2 ] + holder[ i ];
        }
    }

    //parsing
    long long int result = holder.back();
    cout << result;

    return 0;
}
```

***Complexities:***

- *Time*: O ( n )
- *Space*: O ( n )