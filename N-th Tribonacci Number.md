# N-th Tribonacci Number

***Recurrence Relation:*** T<sub>0</sub> = 0, T<sub>1</sub> = 1, T<sub>2</sub> = 1, T<sub>n + 3</sub> = T<sub>n</sub> + T<sub>n + 1</sub> + T<sub>n + 2</sub>, for n >= 0

## 1) Simple Iteration

```cpp
#include<iostream>

using namespace std;

int main()
{
    //input
    int n = 25;
    
    //cases simplication
    if( n < 3 )
    {
        //direct case
        if( n == 0 )
        {
            cout << "0";
        }
        else if( n == 1 || n == 2 )
        {
            cout << "1";
        }
    }
    else
    {
        //base case
        int a = 0;
        int b = 1;
        int c = 1;
        int d = 0;
    
        //iteration logic
        for( int i = 3; i <= n; i++ )
        {
            d = a + b + c;
            a = b;
            b = c;
            c = d;
        }   
        
        //parsing
        cout << d;
    }
    
    return 0;
}
```

***Issues:***

- No Issues

***Complexities:***

- *Time*: O( n )
- *Space*: O( 1 )

## Simple Recursion

```cpp
#include<iostream>

using namespace std;

int trib( int n )
{
    //base case
    if( n == 0 )
    {
        return 0;
    }
    if( n == 1 || n == 2 )
    {
        return 1;
    }
    
    //recursion step
    return trib( n - 1 ) + trib( n - 2 ) + trib( n - 3 );
}

int main()
{
    //input
    int n = 25;
    
    //function call
    int result = trib( n );
    
    //parsing
    cout << result;
    
    return 0;
}
```

***Issues:***

- works for small test cases: 3, 4, 5, 10
- stack overflows or program takes a lot of time for large test cases: 45, 60, 70

***Complexities:***

- *Time*: O( 3<sup>n</sup> )
- *Space*: O( n )

## Recursion + Memoization = Dynamic Programming - Top Down Approach

```cpp
#include<iostream>
#include<map>

using namespace std;

int trib( int n, map< int, int > &mapper )
{
    //memoization look-up
    if( mapper.find( n ) != mapper.end() )
    {
        return mapper[n];
    }

    //base case
    if( n == 0 )
    {
        return 0;
    }
    if( n == 1 || n == 2 )
    {
        return 1;
    }
    
    //recursion step
    int value =  trib( n - 1, mapper ) + trib( n - 2, mapper ) + trib( n - 3, mapper );

    //memoization
    mapper.insert( { n, value } );
    
    return mapper[n];
}

int main()
{
    //input
    int n = 40;
    
    //map object
    map< int, int > mapper;
    
    //function call
    int result = trib( n, mapper );
    
    //parsing
    cout << result;
    
    return 0;
}
```

***Issues***

- No Issues

***Complexities:***

- *Time*: O ( 3n ) &rarr; O ( n )
- *Space*: O ( n )

## Iteration + Tabulation = Dynamic Programming - Bottom Up Approach Aka Table Filling

```cpp
#include<iostream>
#include<vector>

using namespace std;

int main()
{
    //
    int n = 2;
    
    vector<int> holder( n + 1, 0 );
    
    holder[ 0 ] = 0;
    if( n >= 1 )
    {
        holder[ 1 ] = 1;
    }
    
    for( int i = 0; i <= n ; i++ )
    {
        if( ( i + 1 ) <= n ) 
        {
            holder[ i + 1 ] = holder[ i + 1 ] + holder[ i ];
        }
        if( ( i + 2 ) <= n ) 
        {
            holder[ i + 2 ] = holder[ i + 2 ] + holder[ i ];
        }
        if( ( i + 3 ) <= n ) 
        {
            holder[ i + 3 ] = holder[ i + 3 ] + holder[ i ];
        }
    }
    
    int result = holder.back();
    cout << result;

    return 0;
}
```

***Issues***

- No Issues

***Complexities:***

- *Time*: O ( n )
- *Space*: O ( n )