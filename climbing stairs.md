# Climbing stairs

***Recurrence Relation:*** F( n ) = F( n - 1 ) + F( n - 2 )

## 1) Simple Recursion

```
#include<iostream>

using namespace std;

int climbStairs( int n )
{
    //base case
    if( n < 0 )
    {
        return 0;
    }
    if( n == 0 )
    {
        return 1;
    }

    //recursive case
    return climbStairs( n - 1 ) + climbStairs( n - 2 );
}

int main()
{
    //input, n stairs
    int stairs = 3;

    //function call
    int result = climbStairs( stairs );

    //parsing
    cout << "The number of ways to climb: " << stairs << " stairs, by taking 1 or 2 steps at a time is: " << result;

    return 0;
}
```

***Issues:***

- works for small test cases: 1, 2, 3, 5 etc..
- stack overflows or program takes too long for large test cases: 45, 50 etc...

***Complexities:***

- *Time*: O ( 2<sup>n</sup> )
- *Space*: O ( n )

## Recursion + Memoization = Dynamic Programming - Top Down Approach

```cpp
#include<iostream>
#include<map>

using namespace std;

int climbStairs( int n, map< int, int > &mapper )
{
    //memoization lookup
    if( mapper.find( n ) != mapper.end() )
    {
        return mapper[n];
    }

    //base case
    if( n < 0 )
    {
        return 0;
    }
    if( n == 0 )
    {
        return 1;
    }

    //recursive case
    int result = climbStairs( n - 1, mapper ) + climbStairs( n - 2, mapper );

    //memoization
    mapper.insert( { n, result } );

    return mapper[n];
}

int main()
{
    //n stairs
    int stairs = 45;

    //memoization object
    map< int, int > mapper;

    //function call
    int result = climbStairs( stairs, mapper );

    //parsing
    cout << "The number of ways to climb: " << stairs << " stairs, by taking 1 or 2 steps at a time is: " << result;

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
#include<vector>
#include<iostream>

using namespace std;

int main()
{
    //input, n stairs
    int stairs = 3;

    //tabulation
    vector<int> table( stairs + 1, 0 );

    //base case
    table[0] = 1;

    //iteration logic
    for( int i = 0; i <= stairs; i++ )
    {
        if( ( i + 1 ) <= stairs )
        {
            table[ i + 1 ] = table[ i + 1 ] + table[ i ];
        }

        if( ( i + 2 ) <= stairs )
        {
            table[ i + 2 ] = table[ i + 2 ] + table[ i ];
        }
    }

    cout << "The number of ways to climb: " << stairs << " by taking 1 or 2 steps at a time is: " << table[stairs];

    return 0;
}
```

***Issues:***

- No Issues

***Complexities:***

- *Time*: O ( n )
- *Space*: O ( n )

## Variable Swap Implementation

```cpp
#include<vector>
#include<iostream>

using namespace std;

int main()
{
    //input, n stairs
    int stairs = 3;

    //base values
    int a = 1;
    int b = 0;
    int c = 0;

    //iteration logic
    for( int i = 0; i <= stairs; i++ )
    {
        c = a + b;
        a = b;
        b = c;
    }

    //parsing
    cout << "The number of ways to climb " << stairs << " by taking 1 or 2 steps at a time, is: " << c;
    
    return 0;
}
```

***Issues:***

- No Issues

***Complexities:***

- *Time*: O( n )
- *Space*: O( 1 )