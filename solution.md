## Problem 1 :- Given a string, return the first non-repeating character in it and return its index. If it does not exist, return -1.
#Explanation:-
To solve the problem of finding the first non-repeating (i.e., unique) character in a string, I initially considered the brute-force approach of using a nested loop, checking each character against the rest. However, this would result in an inefficient O(n²) time complexity.To optimize, I explored the idea of using an array of size 26 (representing characters a to z) to count character frequencies. This would work well for lowercase alphabetic characters and could run in O(n), but it might be limited in terms of flexibility (e.g., if input contains uppercase or other characters).
1)Ultimately, the most efficient and scalable approach was to use a hash map to store the frequency of each character. This allows me to:
2)Iterate over the string once to count each character's frequency.
3)Iterate over the string a second time to find the first character with a frequency of 1, which would be the first non-repeating character.
4)If no such character exists, return -1.

This approach offers both clarity and efficiency (O(n) time and O(k) space, where k is the number of unique characters).

```
#include <iostream>
#include <string>
#include <map>
using namespace std;

class Solution {
public:
    int firstUniqChar(string s){
        map<char, int> m;

        for(int i=0;i<s.size();i++){
            m[s[i]]++;
        }
        for(int i=0;i<s.size();i++) {
            if (m[s[i]]==1){
                return i;
            }
        }
        return -1;
    }
};

int main(){
    Solution sol;
    string input = "lancey";
    int index = sol.firstUniqChar(input);

    cout<<"The index of the first non-repeating character is: "<<index<<endl;
    return 0;
}
```


## Problem 2 :- Implement the `accumulate` operation, which, given a collection and an operation to perform on each element of the collection, returns a new collection containing the result of applying that operation to each element of the input collection.

Given the collection of numbers:

- 1, 2, 3, 4, 5

And the operation:

- square a number (`x => x * x`)

Your code should be able to produce the collection of squares:

- 1, 4, 9, 16, 25

#Explanation:- 
For this problem, I was asked to apply an operation like squaring to each element of a collection. The first thing that came to my mind was to loop through the array and apply the function manually.But instead of hardcoding it, I wanted to make it reusable for any operation. So I used a function pointer in C++ to pass any operation (like square, cube, etc.) to my accumulate function.
Inside the accumulate function, I just ran a loop from 0 to size and for each element, applied the given operation and stored the result in an output array.
To test it, I used a basic array {1, 2, 3, 4, 5} and passed the square function, which returns x * x. The result I got was {1, 4, 9, 16, 25} — which is correct.
So overall, I built a mini version of map() like in Python, but without using any STL or fancy stuff — just raw C++ arrays and function pointers.

```
#include<iostream>
using namespace std;

const int MAX_SIZE = 1000;

// Function pointer type for the operation
typedef int(*Operation)(int);

// Accumulate function
void accumulate(int in[],int out[],int size,Operation op) {
    for (int i=0;i<size;i++){
        out[i] = op(in[i]);
    }
}
// Operation: square a number
int square(int num) {
    return num*num;
}

int main() {
    int input[] = {1, 2, 3, 4, 5};
    int output[MAX_SIZE];
    int size = 5;

    accumulate(input, output, size, square);

    cout<<"Accumulated result (squares): ";
    for(int i=0;i<size;i++){
        cout<<output[i]<<" ";
    }
    cout<<endl;

    return 0;
}
```

