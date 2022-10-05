## simple recursion using vectors

```cpp
#include<iostream>
#include<vector>

using namespace std;

bool coinChange( vector<int> &denominations, int targetSum, vector<int> &path, vector<int> &shortestPath, int &initial )
{
    //base cases
    if( targetSum == 0 )
    {
        if( initial == 0 || path.size() < shortestPath.size() )
        {
            shortestPath = path;
            initial = 1;
        }
        return true;
    }
    if( targetSum < 0 )
    {
        return false;
    }
    
    bool possible = false;
    
    //recursion step
    for( int i = 0; i < denominations.size(); i++ )
    {
        int remainder = targetSum - denominations[i];
        
        path.push_back( denominations[i] );
        
        bool result = coinChange( denominations, remainder, path, shortestPath, initial );
        if( result == true )
        {
            possible = true;
        }
        
        path.pop_back();
    }
    
    return possible;
}

int main()
{
    vector<int> denominations = { 1, 2, 5 };
    int targetSum = 11;
    
    vector<int> path;
    vector<int> shortestPath;
    int initial = 0;
    
    bool smallestCombination = coinChange( denominations, targetSum, path, shortestPath, initial );
    
    if( smallestCombination == false )
    {
        cout << "It's not possible to combine denominations to produce the target sum";
    }
    else
    {
        cout << "It's possible to combine denominations to produce the target sum:";
        
        for( int i = 0; i < shortestPath.size(); i++ )
        {
            cout << shortestPath[i] << " ";
        }
    }
    
    return 0;
}
```

## simple recursion using constant space values

```cpp
#include<iostream>
#include<vector>

using namespace std;

bool coinChange( vector<int> &denominations, int targetSum, int &path, int &shortestPath, int &initial )
{
    //base cases
    if( targetSum == 0 )
    {
        if( initial == 0 || path < shortestPath )
        {
            shortestPath = path;
            initial = 1;
        }
        return true;
    }
    if( targetSum < 0 )
    {
        return false;
    }
    
    bool possible = false;
    
    //recursion step
    for( int i = 0; i < denominations.size(); i++ )
    {
        int remainder = targetSum - denominations[i];
        
        path = path + 1;
        
        bool result = coinChange( denominations, remainder, path, shortestPath, initial );
        if( result == true )
        {
            possible = true;
        }
        
        path = path - 1;
    }
    
    return possible;
}

int main()
{
    vector<int> denominations = { 1, 2, 5 };
    int targetSum = 11;
    
    int path = 0;
    int shortestPath = 0;
    int initial = 0;
    
    bool smallestCombination = coinChange( denominations, targetSum, path, shortestPath, initial );
    
    if( smallestCombination == false )
    {
        cout << "It's not possible to combine denominations to produce the target sum";
    }
    else
    {
        cout << "It's possible to combine denominations to produce the target sum using:" << shortestPath << " coins";
    }
    
    return 0;
}
```