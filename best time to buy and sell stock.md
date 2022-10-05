## iterative best solutin

```cpp
#include<iostream>
#include<vector>

using namespace std;

int maxProfit( vector<int> values )
{
    int maxP = 0;
    
    int left = 0;
    int right = 1;
    
    while( right < values.size() )
    {
        if( values[left] < values[right] )
        {
            int profit = values[right] - values[left];
            if( profit > maxP )
            {
                maxP = profit;
            }
            right = right + 1;
        }
        else
        {
            left = right;
            right = right + 1;
        }
    }
    
    return maxP;
}

int main()
{
    vector<int> values = { 7, 1, 5, 3, 6, 4 };
    
    int result = maxProfit( values );
    
    cout << result;
    
    return 0;
}
```

## on2 solution

```cpp
#include<iostream>
#include<vector>

using namespace std;

int maxProfit( vector<int> prices )
{
     int maxP = 0;
        for( int i = 0; i < prices.size(); i++ )
        {
            int buy = prices[i];
            for( int j = i + 1; j < prices.size(); j++ )
            {
                int profit = prices[j] - buy;
                if( profit > maxP)
                {
                    maxP = profit;
                }
            }
        }
        return maxP;
}

int main()
{
    vector<int> values = { 7, 1, 5, 3, 6, 4 };
    
    int result = maxProfit( values );
    
    cout << result;
    
    return 0;
}
```