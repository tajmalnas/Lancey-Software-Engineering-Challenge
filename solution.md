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

## Problem 3 :- Make a chain of dominoes.
Compute a way to order a given set of dominoes in such a way that they form a correct domino chain (the dots on one half of a stone match the dots on the neighboring half of an adjacent stone) and that dots on the halves of the stones which don't have a neighbor (the first and last stone) match each other.
For example given the stones `[2|1]`, `[2|3]` and `[1|3]` you should compute something
like `[1|2] [2|3] [3|1]` or `[3|2] [2|1] [1|3]` or `[1|3] [3|2] [2|1]` etc, where the first and last numbers are the same.
For stones `[1|2]`, `[4|1]` and `[2|3]` the resulting chain is not valid: `[4|1] [1|2] [2|3]`'s first and last numbers are not the same.
4 != 3
Some test cases may use duplicate stones in a chain solution, assume that multiple Domino sets are being used.

#Explaination :- 
To solve this domino problem, I first understood that each domino has two numbers and to form a valid chain, the second number of one domino should match the first number of the next one. Also, since it needs to be a loop, the first number of the first domino should match the second number of the last one.
So I thought of using backtracking — that means I try out every possible arrangement of dominoes, including flipping them (because [2|1] is same as [1|2] in chain-building). I keep a used array to track which dominoes are already used in the current chain.

Then I build the chain step by step. If a domino fits, I add it and continue. If not, I backtrack and try a different one. If I can build the entire chain and it forms a loop (start and end match), then it’s a valid solution.
This way I can check if a valid domino loop is possible or no

```
// Online C++ compiler to run C++ program online
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

typedef pair<int,int>Domino;

bool isValidChain(const vector<Domino>&chain){
    for(int i=0;i<chain.size()-1;i++){
        if(chain[i].second!=chain[i+1].first)
            return false;
    }
    // Check if first and last match to form a loop
    return chain.front().first == chain.back().second;
}

bool backtrack(vector<Domino>& dominoes,vector<bool>& used,vector<Domino>& chain){
    if(chain.size() == dominoes.size()){
        return isValidChain(chain);
    }

    for(int i=0;i<dominoes.size();++i){
        if (used[i]) continue;

        for(int flip=0;flip<2;++flip){
            Domino current = dominoes[i];
            if(flip == 1) swap(current.first, current.second);

            if(chain.empty() || chain.back().second == current.first){
                used[i] = true;
                chain.push_back(current);
                if (backtrack(dominoes, used, chain)) return true;
                chain.pop_back();
                used[i] = false;
            }
        }
    }

    return false;
}

bool canFormDominoLoop(vector<Domino>& dominoes){
    vector<bool>used(dominoes.size(),false);
    vector<Domino>chain;
    return backtrack(dominoes, used, chain);
}

int main(){
    vector<Domino>dominoes={{2, 1},{2, 3},{1, 3}}; // You can change this input

    if(canFormDominoLoop(dominoes)){
        cout<<"Valid domino loop exists!"<<endl;
    }
    else{
        cout<<"No valid domino loop possible."<<endl;
    }
    return 0;
}

```

## Problem 4 :- Given a number from 0 to 999,999,999,999, spell out that number in English.
##explaination :- 
First, I saw the problem was about converting numbers into words — like how 123 becomes "one hundred twenty-three". So I thought to break the problem into small steps:

Step-by-step:
1. Handling Small Numbers (0–99):
I started with the basics — numbers from 0 to 99.
For 0–9, I just map them to words using an array like ["", "one", "two", ..., "nine"].
For 10–19, I used another array like ["ten", "eleven", ..., "nineteen"].
For 20–99, I used the tens array like ["", "", "twenty", "thirty", ..., "ninety"], and added the ones part if needed — like for 42, I combine "forty" and "two" with a hyphen.

2. Handling 3-digit Numbers (0–999):
Once I had 0–99 working, I moved to numbers like 345:
If it's something like 300, I say "three hundred".
If it's 345, I say "three hundred forty-five" by combining the hundreds part and the 2-digit part.

3. Breaking the Big Number into Chunks:
Now for big numbers like 1234567890, I thought — let's split them into groups of 3 digits from the right:
So 1234567890 becomes [1, 234, 567, 890]
These chunks represent: 1 billion, 234 million, 567 thousand, 890

4. Adding Scale Words (thousand, million, etc.):
After splitting the number, I just add the correct scale word to each chunk:
Index 0 = "", index 1 = "thousand", 2 = "million", 3 = "billion", 4 = "trillion"
I loop through the chunks from left to right, skipping any chunks that are 0

5. Final Touches:
If the number is 0, I return "zero"
If the input is bigger than 999,999,999,999, I return an error
I made sure to trim extra spaces in the end of the result

🔍 Edge Cases I Handled:
0 → "zero"
Numbers like 100, 101, 110, 999999999999
Invalid inputs like 1000000000000 → throw an error

```
#include<iostream>
#include<vector>
#include<string>
#include<sstream>
using namespace std;

const vector<string>ones = {"", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine"};
const vector<string>teens = {"ten", "eleven", "twelve", "thirteen", "fourteen", "fifteen",
                              "sixteen", "seventeen", "eighteen", "nineteen"};
const vector<string>tens = {"", "", "twenty", "thirty", "forty", "fifty", "sixty", "seventy", "eighty", "ninety"};
const vector<string>scales = {"", "thousand", "million", "billion", "trillion"};

// Spell numbers from 0 to 99
string spellTwoDigits(int num){
    if(num<10) return ones[num];
    else if(num<20) return teens[num - 10];
    else{
        string res = tens[num / 10];
        if(num%10!=0)res+="-"+ones[num % 10];
        return res;
    }
}

// Spell numbers from 0 to 999
string spellThreeDigits(int num){
    string result;
    int hundred=num/100;
    int remainder=num%100;

    if(hundred>0){
        result+=ones[hundred]+" hundred";
        if(remainder>0)result += " ";
    }

    if(remainder>0){
        result+=spellTwoDigits(remainder);
    }

    return result;
}

// Break number into chunks of 3 digits from right
vector<int>splitByThousands(unsigned long long num){
    vector<int>chunks;
    while(num>0){
        chunks.push_back(num%1000);
        num/=1000;
    }
    return chunks;
}

// Spell entire number with scale words
string spellNumber(unsigned long long num){
    if(num == 0) return "zero";

    vector<int>chunks=splitByThousands(num);
    stringstream ss;

    for(int i=(int)chunks.size()-1;i>=0;--i){
        if(chunks[i] == 0) continue;

        ss<<spellThreeDigits(chunks[i]);
        if(!scales[i].empty()) {
            ss<<" "<<scales[i];
        }

        if(i!=0)ss<<" ";
    }

    string result = ss.str();
    // Trim trailing spaces if any
    while(!result.empty() && isspace(result.back())){
        result.pop_back();
    }
    return result;
}

int main(){
    unsigned long long num;

    cout<<"Enter a number between 0 and 999,999,999,999: ";
    cin>>num;

    if(num>999999999999ULL){
        cout<<"Error: Number out of range (must be 0 to 999,999,999,999)."<<endl;
        return 1;
    }

    cout<<spellNumber(num)<<endl;

    return 0;
}

```
