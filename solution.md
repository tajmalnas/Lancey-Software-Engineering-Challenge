## Problem 1 :- Given a string, return the first non-repeating character in it and return its index. If it does not exist, return -1.

To solve the problem of finding the first non-repeating (i.e., unique) character in a string, I initially considered the brute-force approach of using a nested loop, checking each character against the rest. However, this would result in an inefficient O(n²) time complexity.To optimize, I explored the idea of using an array of size 26 (representing characters a to z) to count character frequencies. This would work well for lowercase alphabetic characters and could run in O(n), but it might be limited in terms of flexibility (e.g., if input contains uppercase or other characters).
1)Ultimately, the most efficient and scalable approach was to use a hash map to store the frequency of each character. This allows me to:
2)Iterate over the string once to count each character's frequency.
3)Iterate over the string a second time to find the first character with a frequency of 1, which would be the first non-repeating character.
4)If no such character exists, return -1.

This approach offers both clarity and efficiency (O(n) time and O(k) space, where k is the number of unique characters).

<pre>```
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
    string input = "leetcode";
    int index = sol.firstUniqChar(input);

    cout<<"The index of the first non-repeating character is: "<<index<<endl;
    return 0;
}
```</pre>
