# Can Sum

***Recurrence Relation:*** F( n, [0...i] ) = F( n - [i], [0...i] )

## 1) Simple Recursion

```cpp
#include<iostream>
#include<vector>

using namespace std;

bool canSum( int targetSum, vector<int> &numbers)
{
    //base case 
    if( targetSum == 0 )
    {
        return true;
    }
    if( targetSum < 0 )
    {
        return false;
    }

    //recursion case
    for( int i = 0; i < numbers.size(); i++ )
    {
        int remainder = targetSum - numbers[i];

        bool verdict = canSum( remainder, numbers );

        if( verdict == true )
        {
            return true;
        }
    }

    return false;
}

int main()
{
    vector<int> numbers = { 7, 14 };
    int targetSum = 300;

    bool verdict = canSum( targetSum, numbers );
    
    if( verdict == true )
    {
        cout << "It's possible" << endl;
    }
    else if( verdict == false )
    {
        cout << "It's not possible" << endl;
    }

    return 0;
}
```

***Issues:***
1) Works for smaller values: 8, { 2, 3, 5 }
2) Takes forever or stack overflows for large values: 300, { 7, 14 };

Reason: The search space becomes enormous

***Complexities:***

n = targetSum
m = numbers.size()

- *Time*: O( m<sup>n</sup> )
- *Space*: O( n )

## 2) Recurion + Memoization = DP - Top Down Approach

```cpp
#include<iostream>
#include<vector>
#include<map>

using namespace std;

bool canSum( int targetSum, vector<int> &numbers, map< int, bool > &holder )
{
    //memoization look-up
    if( holder.find( targetSum ) != holder.end() )
    {
        return holder[ targetSum ];
    }

    //base case
    if( targetSum == 0 )
    {
        return true;
    }
    if( targetSum < 0 )
    {
        return false;
    }

    //recursion case
    for( int i = 0; i < numbers.size(); i++ )
    {
        int remainder = targetSum - numbers[i];

        bool verdict = canSum( remainder, numbers, holder );
      
        if( verdict == true )
        {
            //holder.insert( { remainder, true } );
            //return holder[remainder];
            // can choose to not do this, because we immediately end the program once we find the true value

            return true;
        }
    }

    holder.insert( { targetSum, false } );
    return holder[targetSum];
}

int main()
{
    vector<int> numbers = { 7, 14 };
    int targetSum = 300;

    map< int, bool > holder;

    bool verdict = canSum( targetSum, numbers, holder );
    
    if( verdict == true )
    {
        cout << "It's possible" << endl;
    }
    else if( verdict == false )
    {
        cout << "It's not possible" << endl;
    }

    return 0;
}
```
***Issues:***

No issues

***Complexities:***

n = targetSum
m = numbers.size()

- *Time*: O( n * m )
- *Space*: O( n )

## Iteration + Tabulation = DP - Bottom Up Approach aka Table Filling

```cpp
#include<iostream>
#include<vector>

using namespace std;

int main()
{
    vector<int> holder = { 7, 14 };
    int targetSum = 300;

    vector<bool> table( targetSum + 1, false );    

    table[ 0 ] = true;

    for( int i = 0; i <= table.size(); i++ )
    {
        if ( table[ i ] == true )
        {
            for( int j = 0; j < holder.size(); j++ )
            {
                if( i + holder[ j ] <= table.size() )
                {
                    table[ i + holder[ j ] ] = true;
                }
            }
        }
    }

    bool verdict = table.back();
    if( verdict == true )
    {
        cout << "It's possible" << endl;
    }
    else
    {
        cout << "It's not possible" << endl;
    }

    return 0;
}
```
***Issues:***

No issues

***Complexities:***

n = targetSum
m = numbers.size()

- *Time*: O( n * m )
- *Space*: O( n )