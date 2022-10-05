# Grid Traveller

***Reccurence Relation:*** F( n, m ) = F( n - 1, m ) + F( n, m - 1 )

## 1) Simple Recursion

```cpp
#include<iostream>

using namespace std;

int gridTraveller( int n, int m )
{
    //base case
    if( n == 1 && m == 1 )
    {
        return 1;
    }
    if( n == 0 || m == 0 )
    {
        return 0;
    }

    //recursion call
    return gridTraveller( n, m - 1 ) + gridTraveller( n - 1, m );
}

int main()
{
    // n * m grid
    int n = 18; //rows
    int m = 18; // cols

    //function call
    int result = gridTraveller( n, m );

    //parsing
    cout << "The number of ways to traverse a n:" << n << " by m:" << m << "matrix is: "<< result;

    return 0;
}
```

***Issues:***
- Works for smaller test cases: { 1, 1 }, { 2, 3 }, { 3, 2 }, { 3, 3 }
- Takes a long time or stackoverflows for large test cases: { 18, 18 }

***Complexities:***

- *Time*: O( 2<sup>(n+m)</sup> )
- *Space*: O( n + m )

## Recursion + Memoization = Dynamic Programming - Top Down Approach

```cpp
#include<iostream>
#include<map>
#include<string>

using namespace std;

#define lli long long int

lli gridTraveller( lli n, lli m, map< string, lli > &holder )
{
    //key creation
    if( m < n )
    {
        lli temp = m;
        m = n;
        n = temp;
    }
    
    string key = to_string( n ) + to_string( m );

    //memoization look-up
    if( holder.find( key ) != holder.end() )
    {
        return holder[key];
    }
    
    //base case
    if( n == 1 && m == 1 )
    {
        return 1;
    }
    if( n == 0 || m == 0 )
    {
        return 0;
    }

    //recursion call
    lli value =  gridTraveller( n, m - 1, holder ) + gridTraveller( n - 1, m, holder );

    //memoization
    holder.insert( { key, value } );
    
    return holder[key];
}

int main()
{
    // n * m grid
    lli n = 18; //rows
    lli m = 18; // cols

    //map object
    map< string, lli > holder;

    //function call
    lli result = gridTraveller( n, m, holder );

    //parsing
    cout << "The number of ways to traverse a n:" << n << " by m:" << m << "matrix is: "<< matrix[ n ][ m ];

    return 0;
}
```

***Issues:***

- No Issues

***Complexities:***

- *Time*: O( nm )
- *Space*: O( n + m )

## Iteration + Tabulation = Dynamic Programming - Bottom Up Approach Aka Table Filler

```cpp
#include<iostream>
#include<vector>

using namespace std;

#define lli long long int

int main()
{
    //input
    lli n = 18;
    lli m = 18;

    //DP table
    vector<vector<lli>> matrix( n + 1, vector<lli>( m + 1, 0 ) );

    //base case
    matrix[1][1] = 1;

    //iteration logic
    for( lli i = 0; i <= n; i++ )
    {
        for( lli j = 0; j <= m; j++ )
        {
            lli current = matrix[ i ][ j ];
            if( ( i + 1 ) <= n )
            {
                matrix[ i + 1 ][j] = matrix[ i + 1 ][ j ] + current;
            }
            if( ( j + 1 ) <= m )
            {
                matrix[ i ][ j + 1 ] = matrix[ i ][ j + 1 ] + current;
            }
        }
    }

    //parsing
    cout << "The number of ways to traverse a n:" << n << " by m:" << m << "matrix is: "<< matrix[ n ][ m ];

    return 0;
}
```

***Issues:***

- No issues

***Complexities:***

- *Time*: O( nm ) 
- *Space*: O( nm )