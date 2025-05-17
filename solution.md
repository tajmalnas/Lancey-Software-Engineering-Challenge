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
![image](https://github.com/user-attachments/assets/72bf5703-d973-4473-9141-067c234efcd9)



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
![image](https://github.com/user-attachments/assets/79ae0182-d7a4-4028-9919-7462bd950af4)



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
![image](https://github.com/user-attachments/assets/6c655cec-4a2f-47f8-a296-771c12c6517b)


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

Edge Cases I Handled:
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
![image](https://github.com/user-attachments/assets/2ca8c1c6-7bc5-48d9-a3bf-5c9fd591247b)



## Problem 4 :- Given the position of two queens on a chess board, indicate whether or not they are positioned so that they can attack each other.
##Explaination :- 
First, I thought about how to represent the board. Since the chessboard is 8x8, and positions are zero-indexed, I just used two pairs of coordinates: one for the white queen and one for the black queen.Now, to check if they can attack each other, I just had to verify these three conditions:
Same Row : If the rows of both queens are equal, then they are on the same row — so they can attack.
Same Column: If the columns are equal, then they’re in the same column — attack is possible again.
Same Diagonal: This was the only slightly tricky part. I remembered that in diagonals, the difference between rows and columns is equal. So I used:
abs(row1 - row2) == abs(col1 - col2)
If this is true, then they are on the same diagonal — again, they can attack.
If any one of these conditions is true, then the queens can attack each other.
Finally, just for fun and to verify visually, I printed the board showing where the queens are. It helped me confirm that my logic actually works.

```
#include <iostream>
#include <vector>
using namespace std;

// Function to check if two queens can attack each other
bool canQueensAttack(int row1, int col1, int row2, int col2){
    // Same row
    if (row1==row2) return true;

    // Same column
    if (col1==col2) return true;

    // Same diagonal
    if (abs(row1-row2)==abs(col1-col2)) return true;

    return false;
}

// Optional: Visualize the chessboard
void printChessBoard(int row1, int col1, int row2, int col2){
    cout<<"Chessboard:\n";
    for(int i = 0; i < 8; i++){
        for(int j = 0; j < 8; j++){
            if(i == row1 && j == col1)
                cout<<"W "; // White Queen
            else if(i == row2 && j == col2)
                cout<<"B "; // Black Queen
            else
                cout << ". ";
        }
        cout << endl;
    }
}

int main(){
    // White Queen at c5 → (row 3, col 2)
    // Black Queen at f2 → (row 6, col 5)
    int row1 = 3, col1 = 2;
    int row2 = 6, col2 = 5;

    // Print the board (optional)
    printChessBoard(row1, col1, row2, col2);

    // Check if they can attack
    if (canQueensAttack(row1, col1, row2, col2)){
        cout<<"\nYes, the queens can attack each other.\n";
    } else{
        cout<<"\nNo, the queens cannot attack each other.\n";
    }
    return 0;
}

```
![image](https://github.com/user-attachments/assets/3e519291-d1b8-46c1-8706-2f21e46ab52e)



## Problem 6 :- Parse and evaluate simple math word problems returning the answer as an integer.
##Explaination :- 
When I first read the problem, I realized it's not a typical math expression with numbers and operators — it's a sentence that needs to be understood and broken down.

Step 1 – Understand the input:
I saw that the input starts with "What is..." and ends with a "?", and everything in between is describing a math operation in words — like “plus”, “minus”, etc. So, I needed to extract the meaningful parts from the string.

Step 2 – Tokenizing the sentence:
I thought about splitting the sentence into words, then identifying which are numbers and which are operations. I knew I’d need to convert "plus" into "+", "minus" into "-", and so on. For “multiplied by” and “divided by,” I remembered they are two words but represent a single operator — so I had to handle that specially.

Step 3 – Left-to-right evaluation:
The question clearly said to ignore operator precedence, which is different from normal math. So I decided I would just process the tokens from left to right, doing one operation at a time.

Step 4 – Error Handling:
I realized that I also need to be strict:
If someone gives a question like "What is 5 cubed?", that’s not valid.
If there are two operators in a row like "plus plus", that’s wrong too.
If someone divides by zero, I should catch that.
If it’s not a math question at all, like "Who is the President?", I should reject it.

Step 5 – Putting it together:
Once I had the parsing logic, the operator conversion, and error checking in place, I combined everything into a function that could evaluate any valid input. I also tested it on various cases to make sure it handled both correct and incorrect input.

```
#include <iostream>
#include <sstream>
#include <vector>
#include <stdexcept>
using namespace std;

int evaluateWordProblem(const string& input){
    string question=input;
    if (question.substr(0, 8) != "What is " || question.back() != '?'){
        throw invalid_argument("Invalid question format");
    }
    question = question.substr(8, question.size() - 9);

    // Replace words with operators
    vector<string>tokens;
    istringstream ss(question);
    string word;
    while(ss>>word) {
        if(word=="plus") tokens.push_back("+");
        else if(word=="minus") tokens.push_back("-");
        else if(word == "multiplied"){
            string by;
            ss>>by;
            if (by!="by") throw invalid_argument("Invalid syntax after 'multiplied'");
            tokens.push_back("*");
        } else if(word == "divided"){
            string by;
            ss>>by;
            if (by != "by") throw invalid_argument("Invalid syntax after 'divided'");
            tokens.push_back("/");
        } else{
            // Could be number
            try{
                int num = stoi(word);
                tokens.push_back(to_string(num));
            }catch (...) {
                throw invalid_argument("Unsupported token or syntax: " + word);
            }
        }
    }

    if (tokens.empty()) throw invalid_argument("Empty problem");

    // Evaluate left to right
    int result = stoi(tokens[0]);
    for(size_t i=1;i<tokens.size();i+=2){
        if(i+1>=tokens.size()) throw invalid_argument("Incomplete operation");
        string op = tokens[i];
        int num = stoi(tokens[i+1]);

        if(op == "+") result+=num;
        else if(op=="-") result -= num;
        else if(op=="*") result *= num;
        else if(op=="/") {
            if(num == 0) throw invalid_argument("Division by zero");
            result/=num;
        } else {
            throw invalid_argument("Unknown operation: " + op);
        }
    }

    return result;
}

int main(){
    vector<string>testCases ={
        "What is 5?",
        "What is 5 plus 13?",
        "What is 7 minus 5?",
        "What is 6 multiplied by 4?",
        "What is 25 divided by 5?",
        "What is 5 plus 13 plus 6?",
        "What is 3 plus 2 multiplied by 3?",
        "What is 10 divided by 0?", // should throw error
        "What is 1 plus plus 2?",    // invalid syntax
        "What is 5 cubed?",          // unsupported operation
        "Who is the President?"      // non-math question
    };

    for(const string& q:testCases){
        try{
            cout<<q<<" = "<<evaluateWordProblem(q)<<endl;
        }catch (const exception& e) {
            cout <<q<<" => Error: "<<e.what()<<endl;
        }
    }
    return 0;
}

```
![image](https://github.com/user-attachments/assets/a2d9b152-6f74-44f1-97cb-33c87d589d12)
