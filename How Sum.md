# simple implementation
#include<iostream>
#include<vector>

using namespace std;

bool howSum( int targetSum, vector<int> &numbers, vector<int> &combination)
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
    
    //recursion step
    for( int i = 0; i < numbers.size(); i++ )
    {
        int remainder = targetSum - numbers[i];
        
        bool verdict = howSum( remainder, numbers, combination );
        if( verdict == true )
        {
            combination.push_back( numbers[i] );
            return true;
        }
    }
    
    return false;
}

int main()
{
    vector<int> numbers = { 2, 3, 5 };
    int targetSum = 8;
    
    vector<int> combination;
    
    bool verdict = howSum( targetSum, numbers, combination );
    if( verdict == true )
    {
        cout << "It's possible to produce target sum using numbers: ";
        for( int i = 0; i < combination.size(); i++ )
        {
            cout << combination[i] << " ";
        }
    }
    else
    {
        cout << "It's not possible to produce target sum";
    }
    
    return 0;
}

## with memoization but memoizing everything

#include<iostream>
#include<vector>
#include<map>

using namespace std;

bool howSum( int targetSum, vector<int> &numbers, vector<int> &combination, map< int, vector<int>> &mapper )
{
    if( mapper.find( targetSum ) != mapper.end() )
    {
        if( !mapper[targetSum].empty())
        {
            combination = mapper[targetSum];
            combination.insert(combination.end(), mapper[targetSum].begin(), mapper[targetSum].end());
            return true;
        }
        return false;
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
    
    //recursion step
    for( int i = 0; i < numbers.size(); i++ )
    {
        int remainder = targetSum - numbers[i];
        
        bool verdict = howSum( remainder, numbers, combination, mapper );
        if( verdict == true )
        {
            combination.push_back( numbers[i] );
            mapper.insert( { targetSum, combination}  );
            return true;
        }
    }
    
    mapper.insert( { targetSum, vector<int>() });
    
    return false;
}

int main()
{
    vector<int> numbers = { 2, 3 };
    int targetSum = 7;
    
    vector<int> combination;
    
    map< int, vector<int>> mapper;
    
    bool verdict = howSum( targetSum, numbers, combination, mapper );
    if( verdict == true )
    {
        cout << "It's possible to produce target sum using numbers: ";
        for( int i = 0; i < combination.size(); i++ )
        {
            cout << combination[i] << " ";
        }
    }
    else
    {
        cout << "It's not possible to produce target sum";
    }
    
    return 0;
}

## with memoization but only adding false cases

#include<iostream>
#include<vector>
#include<map>

using namespace std;

bool howSum( int targetSum, vector<int> &numbers, vector<int> &combination, map< int, vector<int>> &mapper )
{
    if( mapper.find( targetSum ) != mapper.end() )
    {
        return false;
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
    
    //recursion step
    for( int i = 0; i < numbers.size(); i++ )
    {
        int remainder = targetSum - numbers[i];
        
        bool verdict = howSum( remainder, numbers, combination, mapper );
        if( verdict == true )
        {
            combination.push_back( numbers[i] );
            return true;
        }
    }
    
    mapper.insert( { targetSum, vector<int>() });
    
    return false;
}

int main()
{
    vector<int> numbers = { 7, 14 };
    int targetSum = 300;
    
    vector<int> combination;
    
    map< int, vector<int>> mapper;
    
    bool verdict = howSum( targetSum, numbers, combination, mapper );
    if( verdict == true )
    {
        cout << "It's possible to produce target sum using numbers: ";
        for( int i = 0; i < combination.size(); i++ )
        {
            cout << combination[i] << " ";
        }
    }
    else
    {
        cout << "It's not possible to produce target sum";
    }
    
    return 0;
}

# same as last but optimization using bool

#include<iostream>
#include<vector>
#include<map>

using namespace std;

bool howSum( int targetSum, vector<int> &numbers, vector<int> &combination, map< int, bool> &mapper )
{
    if( mapper.find( targetSum ) != mapper.end() )
    {
        return false;
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
    
    //recursion step
    for( int i = 0; i < numbers.size(); i++ )
    {
        int remainder = targetSum - numbers[i];
        
        bool verdict = howSum( remainder, numbers, combination, mapper );
        if( verdict == true )
        {
            combination.push_back( numbers[i] );
            return true;
        }
    }
    
    mapper.insert( { targetSum, false });
    
    return false;
}

int main()
{
    vector<int> numbers = { 7, 14 };
    int targetSum = 38416;
    
    vector<int> combination;
    
    map< int, bool> mapper;
    
    bool verdict = howSum( targetSum, numbers, combination, mapper );
    if( verdict == true )
    {
        cout << "It's possible to produce target sum using numbers: ";
        for( int i = 0; i < combination.size(); i++ )
        {
            cout << combination[i] << " ";
        }
    }
    else
    {
        cout << "It's not possible to produce target sum";
    }
    
    return 0;
}

## Implementing as optional without memoizaiotn

#include<iostream>
#include<optional>
#include<vector>

using namespace std;

optional<vector<int>> howSum( int targetSum, vector<int> &numbers )
{
    //base case
    if( targetSum == 0 )
    {
        return vector<int>{};
    }
    if( targetSum < 0 )
    {
        return nullopt;
    }
    
    // recursion step
    for( int i = 0; i < numbers.size(); i++ )
    {
        int remainder = targetSum - numbers[i];
        
        optional<vector<int>> combination = howSum( remainder, numbers );
        if( combination != nullopt )
        {
            (*combination).push_back( numbers[i] );
            return combination;
        }
    }
    
    return nullopt;
}

int main()
{
    vector<int> numbers = { 7, 14 };
    int targetSum = 300;
    
    optional<vector<int>> combination = howSum( targetSum, numbers );
    if( combination == nullopt )
    {
        cout << "It's not possible to generate the targetSum using the numbers of the array";
    }
    else if( combination != nullopt )
    {
        cout << "It's possible to generate the targetSum using the numbers of the array:";
        for( int i = 0; i < (*combination).size(); i++ )
        {
            cout << (*combination)[i] << " ";
        }
    }
    
    return 0;
}

## optional + memoization ( only false cases, can do with all cases too )
#include<iostream>
#include<optional>
#include<vector>
#include<map>

using namespace std;

optional<vector<int>> howSum( int targetSum, vector<int> &numbers, map< int, optional<vector<int>>> &holder )
{
    if( holder.find( targetSum ) != holder.end() )
    {
        return nullopt;
    }
    //base case 
    if( targetSum == 0 )
    {
        return vector<int>{};
    }
    if( targetSum < 0 )
    {
        return nullopt;
    }
    
    // recursion step
    for( int i = 0; i < numbers.size(); i++ )
    {
        int remainder = targetSum - numbers[i];
        
        optional<vector<int>> combination = howSum( remainder, numbers, holder );
        if( combination != nullopt )
        {
            (*combination).push_back( numbers[i] );
            return combination;
        }
    }
    
    holder[targetSum] = nullopt;
    return nullopt;
}

int main()
{
    vector<int> numbers = { 7, 14 };
    int targetSum = 300;
    
    map< int, optional<vector<int>>> holder;
    
    optional<vector<int>> combination = howSum( targetSum, numbers, holder );
    if( combination == nullopt )
    {
        cout << "It's not possible to generate the targetSum using the numbers of the array";
    }
    else if( combination != nullopt )
    {
        cout << "It's possible to generate the targetSum using the numbers of the array:";
        for( int i = 0; i < (*combination).size(); i++ )
        {
            cout << (*combination)[i] << " ";
        }
    }
    
    return 0;
}

## tabulation

#include<iostream>
#include<vector>
#include<optional>

using namespace std;

int main()
{
    vector<int> numbers = { 7, 14 };
    int targetSum = 300;

    vector<optional<vector<int>>> matrix( targetSum + 1, nullopt );

    matrix[0] = vector<int>{};

    for( int i = 0; i <= targetSum; i++ )
    {
        if( matrix[i] != nullopt )
        {
            for( int j = 0; j < numbers.size(); j++ )
            {
                if( i + numbers[j] <= targetSum )
                {
                    optional<vector<int>> temp = matrix[i];
                    temp->push_back( numbers[j]);
                    matrix[i+numbers[j]] = temp;
                }
            }
        }
    }

    if( matrix[targetSum] == nullopt )
    {
        cout << "It's not possible to generate the targetSum using the numbers of the array";
    }
    else
    {
        for( int i = 0; i < matrix[targetSum]->size(); i++ )
        {
            cout << matrix[targetSum]->operator[](i) << " ";
        }
    }

    return 0;
}