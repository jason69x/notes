competitive programming combines two topics :

1. design of algorithms
2. implementation of algorithms

#optimizecpp

`#include<bits/stdc++.h>` 

allows us to include the entire standard library. it is a precompiled header file. it copies all the content of the header file in current file. header file generally contains function prototypes.

`using namespace std` 

uses the standard namespace. used to reduce naming conflicts. entities declared inside a namespace are placed in a namespace scope.

`::` - scope resolution operator

```cpp
namespace ns-name { declarations }
ns-name::member-name
using namespace ns-name
```

`using`

using is generally used for namespaces and type aliases.

```cpp
using vec = std::vector<int>;
```

code can be compiled using : 

`g++ -std=c++11 -Wall -O2`

-O2 increases performance of code but takes more time to compile.

make input/output more efficient by including at the start of main :

```cpp
ios::sync_with_stdio(0);
cin.tie(0);
```

i/o streams are initialized when the program starts, so including them at the start of main applies them right after program starts executing. 

first line , disables synchronization between c and cpp standard streams. this allows cpp streams to have their own buffer instead of mixing streams of c and cpp .

second line, unties `cin` and `cout`  .`cout` is buffered by default, it only outputs when buffer is full or flushed. therefore, you need to flush the buffer manually before accepting input from the user. you can flush buffer by using `\n` .

`\n` is faster than `endl`.

#inputcpp

read a whole line from input :

```cpp
string s;
getline(cin,s);
```

 if amount of data is unknown :
 
```cpp
while(cin>>x){
//code
}
```

this loops reads input from the data one after another until there is no data left.

`cin>>a>>b;` works because `cin` returns a reference to itself.

`if(cin)` it returns false if the last input operation failed ex. mismatch datatype.
or you can use `cin.fail()` to check last input status (returns true if failed).

read from a file and write to a file :

```cpp
freopen("input.txt","r",stdin);
freopen("output.txt","w",stdout);
fclose(stdout);
```

*inheritance*, some classes inherits properties of previously written classes. the descendant classes then add on additional properties making them specializations of their parent class.

#datatypescpp

*integers* - 

`int` - 32 bits
`long long` - 64 bits

common mistake - 

```cpp
int a = 123456789;
long long b = a*a;
cout<<b<<"\n";   // -1757895751
```

here type of $a$ is int and result of expression  $a*a$  is also of type int and not type long long.
you can either change type of $a$ to long long or use typecast.

```cpp
long long a = 123456789;
long long b = (long long)a*a;
long long x = (long long)y*(long long y);
```

`__int128_t` - 128 bits

*modulo* -  $\%$

in addition, subtraction and multiplication remainder can be taken before operation, so that the numbers never become too large.

$(a+b) \mod m = (a \mod m + b \mod m) \mod m$

in cpp, the remainder of a negative number is either 0 or negative. to make sure remainder is between $0$ and $n-1$ use ,

```cpp
x = x%m;
if(x<0) x+=m;
```

*floating point numbers* -

`double`  - 64bits
`long double` - 80bits

`printf("%.9f\n",x);` use this to format floating point numbers.

a better way to compare floating point numbers is by assuming that they are equal if difference between them is less than $e$ , where $e$ is a small number.

```cpp
if( abs(a-b) < 1e-9 ){
cout<<"a and b are equal";
}
```

#shortcpp

use typedef to give shorter names to datatypes - 

```cpp
typedef long long ll;
typedef vector<int> vi;
typedef pair<int,int> pi;
```

***macros*** -

macros means that certain strings in the program will be changes before compilation. macros are defined using `#define` keyword.

```
#define F first
#define S second
#define PB push_back
#define MP make_pair

#define REP(i,a,b) for(int i=a;i<=b;i++)
```



***Mathematics*** - 

each sum of the form : 

$\displaystyle\sum_{x=1}^n x^k = 1^k + 2^k + 3^k ... + n^k$ , has a closed formula that is a polynomial of degree $k+1$.

$\displaystyle\sum_{x=1}^n x = 1 + 2 + 3 ... + n = \frac{n(n+1)}{2}$

$\displaystyle\sum_{x=1}^n x^2 = 1^2 + 2^2 + 3^2 ... + n^2 = \frac{n(n+1)(2n+1)}{6}$

*arithmetic progression* : 

a sequence of numbers where the difference between any two consecutive numbers is constant.

$a + ... + b = \frac{n(a+b)}{2}$

where a is the first term , b is the second term and n is the amount of numbers.

*geometric progression :*

a sequence of numbers where the ratio between any two consecutive numbers is constant.

$a+ak+ak^2 ... + b = \frac{bk-a}{k-1} = \frac{a(r^n-1)}{r-1}$

$1+2+4+8+\ldots+2^{n-1} = 2^n -1$

*harmonic sum* :

$\displaystyle\sum_{x=1}{n}\frac{1}{x}  = \frac{1}{1} + \frac{1}{2} + \frac{1}{3} +\ldots+ \frac{1}{n}$

upper bound for harmonic sum is $log{_2}{(n)} + 1$ , replace every part with nearest power of 2 not exceeding x.

*binet's formula fibonacci* - 

$f(n) = \frac{(1+\sqrt{5})^n-(1-\sqrt{5})^n}{2^n\sqrt{5}}$

*logarithms* - 

$\log{_k}{(x)} = a$   exactly when $k^a = x$ .

$\log{_k}{(x)} = a$ equals to the number of times we need to divide $x$ by $k$ before we reach the number $1$.

change of base, $\log_u(x) = \frac{log_k(x)}{log_k(u)}$

number of digits of an integer in base b is $\lfloor \log_b(x) + 1 \rfloor$ .

$(a\% b + b)\% b$

*inversions* - 

a pair of array elements $(arr[i],arr[j])$ such that $i<j$ and $arr[i]>arr[j]$ .

sorting lower bounds for *comparison* based sorting algorithms is $O(n\log(n))$ 

`for(auto x:vec)` copies each element, modifies a copy not the original vector.

`for(auto& x:vec)` doesn't copy, modifies original vector.

`for(const auto& x:vec)` used for read only purposes.

*counting sort* - 

it creates a bookkeeping array where array indices represents the elements of input array and their  values represents the frequency of their occurences in the input array. time complexity - $O(n)$ .
suitable when $c$ (max no. in input array) is small.

in cpp , *sets* are implemented as self-balancing binary search trees ( usually red-black trees). operations like insertion,deletion and searching has avg. time complexity of $O(\log(n))$.

**set** - stores unique elements in sorted order

**multiset** - allows duplicates, sorted order

**unordered_set** - unique elements, unsorted, faster for lookups $O(1)$ , uses hashing

**unordered_multiset** - allows duplicates, unsorted order, $O(1)$ lookups

**map** - sorted , balanced bst
**unordered_map** - unsorted,hash table $O(1)$ 
**multimap** - multiple values for a key

`set<int> set;`

`map<string,int> map;`

`unordered_set<int> us;`

`unordered_map<string,int> ump;`

`stack<int> s;`

`queue<int> q;`

`priority_queue<int> max_heap;`

`priority_queue<int,vector<int>,greater<int>> min_heap;`


**stack** - 

when a function is called , *a stack frame ( or activation record )* is created on the stack. this frame contains : local variables, function parameters, return address.
when a function exits,its stack frame is popped, and the memory is reclaimed automatically.

bcoz CPU can't address anything smaller than a byte.

**slow and fast pointer** - 

tortoise , moves one node at a time
hare , moves two nodes

cycle, hare and tortoise meet at some node
no cycle, hare reaches end of list

if the hare is k nodes behind , after each move, the gap reduces by one. since it is reduced by one hare doesn't skip over tortoise, instead they meet at some point.

after detecting cycle, to find the node where cycle starts. reset tortoise to start and move both tortoise and hare one node at a time. the point where they meet is the node where cycle starts.

this method can be used on other structures which contains a cycle.

`git add .`  used to stage changes.

**Boyer-Moore : bad character rule** 

fast string matching, check characters from right to left of string that needs to be matched.
upon mismatch, skip alignments until (a) a mismatch becomes a match or (b) p moves past mismatched character.

