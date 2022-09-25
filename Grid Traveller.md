# Grid Traveller

***Reccurence Relation:*** F( n, m ) = F( n - 1, m ) + F( n, m - 1 )

## 1) Simple Recursion

```cpp
#include<iostream>

using namespace std;

int gridTraveller( int n, int m )
{
    if( n == 1 && m == 1 )
    {
        return 1;
    }
    if( n == 0 || m == 0 )
    {
        return 0;
    }
    return gridTraveller( n, m - 1 ) + gridTraveller( n - 1, m );
}

int main()
{
    // n * m grid
    int n = 18; //rows
    int m = 18; // cols

    int result = gridTraveller( n, m );

    cout << result;

    return 0;
}
```


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
    if( m < n )
    {
        lli temp = m;
        m = n;
        n = temp;
    }
    
    string key = to_string( n ) + to_string( m );

    if( holder.find( key ) != holder.end() )
    {
        return holder[key];
    }
    
    if( n == 1 && m == 1 )
    {
        return 1;
    }
    if( n == 0 || m == 0 )
    {
        return 0;
    }
    lli value =  gridTraveller( n, m - 1, holder ) + gridTraveller( n - 1, m, holder );

    holder.insert( { key, value } );
    return holder[key];
}

int main()
{
    // n * m grid
    lli n = 18; //rows
    lli m = 18; // cols

    map< string, lli > holder;

    lli result = gridTraveller( n, m, holder );

    cout << result;

    return 0;
}
```

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
    lli n = 18;
    lli m = 18;

    vector<vector<lli>> matrix( n + 1, vector<lli>( m + 1, 0 ) );

    matrix[1][1] = 1;

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

    cout << matrix[ n ][ m ];

    return 0;
}
```

***Complexities:***

- *Time*: O( nm ) 
- *Space*: O( nm )