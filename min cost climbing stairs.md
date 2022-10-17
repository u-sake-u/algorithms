# Min cost Climbing stairs

***Recurrence Relation:*** F( n, [ 0..n] ) = F( n - 1, [ 0..n] ) + F( n - 2, [ 0..n] )

## 1) Simple Recursion

```cpp
#include<iostream>
#include<algorithm>
#include<vector>

using namespace std;

int calc( int n, vector<int> &cost )
{
    //base case
    if( n == 0 || n == 1 )
    {
        return cost[n];
    }

    //recursive case
    return min( calc( n - 1, cost ), calc( n - 2, cost )) + cost[n];
}

int main()
{
    //input
    vector<int> cost = { 10, 15, 20 };
    
    //add final step cost
    cost.push_back(0);

    //function call
    int result = calc( cost.size() - 1, cost );

    //parsing
    cout << "The minimum cost of climbing " << cost.size() - 1 << " stairs is: " << result;

    return 0;
}
```
***Issues:***

- works for small test cases of cost.size() 
- stack overflows or program takes too long for large values of cost.size()

***Complexities:***

- *Time*: O ( 2<sup>n</sup> )
- *Space*: O ( n )

## 2) Simple Recursion - variation

```cpp
#include<iostream>
#include<algorithm>
#include<vector>

using namespace std;

int calc( int n, vector<int> &cost )
{
    //base case
    if( n == 0 || n == 1 )
    {
        return cost[n];
    }

    //recursive case
    return min( calc( n - 1, cost ), calc( n - 2, cost )) + cost[n];
}

int main()
{
    //input
    vector<int> cost = { 10, 15, 20 };

    //function call
    int result = min( calc( cost.size() - 1, cost ), calc( cost.size() - 2, cost ) );

    //parsing
    cout << "The minimum cost of climbing " << cost.size() << " stairs is: " << result;

    return 0;
}
```

***Issues:***

- works for small test cases of cost.size() 
- stack overflows or program takes too long for large values of cost.size()

***Complexities:***

- *Time*: O ( 2<sup>n</sup> )
- *Space*: O ( n )

## 3) Recursion + Memoization = Dynamic Programming - Top Down Approach

```cpp
#include<iostream>
#include<algorithm>
#include<vector>
#include<map>

using namespace std;

int calc( int n, vector<int> &cost, map< int, int > &mapper )
{
    //memoization lookup
    if( mapper.find(n) != mapper.end() )
    {
        return mapper[n];
    }

    //base case
    if( n == 0 || n == 1 )
    {
        return cost[n];
    }

    //recursive case
    int result = min( calc( n - 1, cost, mapper ), calc( n - 2, cost, mapper )) + cost[n];

    //memoization
    mapper.insert( { n, result });

    return mapper[n];
}

int main()
{
    //input
    vector<int> cost = { 1,100,1,1,1,100,1,1,100,1 };

    //memoization object
    map< int, int > mapper;

    //function call
    int result = min( calc( cost.size() - 1, cost, mapper ), calc( cost.size() - 2, cost, mapper ) );

    //parsing
    cout << "The minimum cost of climbing " << cost.size() << " stairs is: " << result;

    return 0;
}
```

***Issues***

- No issues

***Complexities:***

- *Time*: O ( 2n ) &rarr; O ( n )
- *Space*: O ( n )

## iteration + tabulation = DP - Bottom up approach

```cpp
#include<iostream>
#include<vector>

using namespace std;

int main()
{
    //input, cost
    vector<int> cost = { 1, 100, 1, 1, 1, 100, 1, 1, 100, 1 };

    //tabulation
    vector<int> table( cost.size(), 0 );

    //base case
    table[0] = cost[0];
    table[1] = cost[1];

    //iteration logic
    for( int i = 0; i < cost.size(); i++ )
    {
        if( ( i + 2 ) < cost.size() )
        {
            int min_val = min( table[i], table[i+1]);

            table[i+2] = min_val + cost[i+2];
        }
    }

    //parsing
    cout << "The minimum cost of climbing " << cost.size() << " stairs is: " << min(table[cost.size() - 1], table[cost.size()-2]);

    return 0;
}
```


***Issues:***

- No Issues

***Complexities:***

- *Time*: O ( n )
- *Space*: O ( n )

## iteration + tabulation = DP - Bottom up approach - variation

```cpp
#include<iostream>
#include<vector>

using namespace std;

int main()
{
    //input, cost
    vector<int> cost = { 1, 100, 1, 1, 1, 100, 1, 1, 100, 1 };
    
    //push zero
    cost.push_back(0);

    //tabulation
    vector<int> table( cost.size(), 0 );

    //base case
    table[0] = cost[0];
    table[1] = cost[1];

    //iteration logic
    for( int i = 0; i <= cost.size(); i++ )
    {
        if( ( i + 2 ) <= cost.size() )
        {
            int min_val = min( table[i], table[ i + 1 ] );

            table[ i + 2 ] = min_val + cost[i + 2];
        }
    }

    cout << table[cost.size() - 1];

    return 0;
}
```


***Issues:***

- No Issues

***Complexities:***

- *Time*: O ( n )
- *Space*: O ( n )

## constant space / variable swap

```cpp
#include<iostream>
#include<vector>

using namespace std;

int main()
{
    //input, cost
    vector<int> cost = { 1, 100, 1, 1, 1, 100, 1, 1, 100, 1 };
    
    //push zero
    cost.push_back(0);

    //base values
    int a = cost[0];
    int b = cost[1];
    int c = 0;

    //iteration logic
    for( int i = 0; i < cost.size(); i++ )
    {
        if( ( i + 2 ) < cost.size() )
        {
            int min_val = min( a, b );
            c = min_val + cost[ i + 2 ];
        
            a = b;
            b = c;
        }
    }

    cout << c;

    return 0;
}
```

***Issues:***

- No Issues

***Complexities:***

- *Time*: O ( n )
- *Space*: O ( 1 )

## constant space / variable swap - variation

```cpp
#include<iostream>
#include<vector>

using namespace std;

int main()
{
    //input, cost
    vector<int> cost = { 1, 100, 1, 1, 1, 100, 1, 1, 100, 1 };
    
    //base values
    int a = cost[0];
    int b = cost[1];
    int c = 0;

    //iteration logic
    for( int i = 0; i < cost.size(); i++ )
    {
        if( ( i + 2 ) < cost.size() )
        {
            int min_val = min( a, b );
            c = min_val + cost[ i + 2 ];
        
            a = b;
            b = c;
        }
    }

    cout << min(a,b);

    return 0;
}
```


***Issues:***

- No Issues

***Complexities:***

- *Time*: O ( n )
- *Space*: O ( 1 )
