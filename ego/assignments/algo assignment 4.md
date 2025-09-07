## CS 513 - Assignment 4

Name   : Abhishek Meena
Roll no : 254101002
Course : M.tech CSE (1st yr)
Email   : [abhishekm1002@iitg.ac.in](mailto:abhishekm1002@iitg.ac.in)


###### Stacks - 

**S1. stack permutation.**

$\to$ 325641 -   yes, this order can be permuted.

| operation |  stack  |    output     |
| :-------: | :-----: | :-----------: |
|  push(1)  |   [1]   |      []       |
|  push(2)  |  [1,2]  |      []       |
|  push(3)  | [1,2,3] |      []       |
|   pop()   |  [1,2]  |      [3]      |
|   pop()   |   [1]   |     [3,2]     |
|  push(4)  |  [1,4]  |     [3,2]     |
|  push(5)  | [1,4,5] |     [3,2]     |
|   pop()   |  [1,4]  |    [3,2,5]    |
|  push(6)  | [1,4,6] |    [3,2,5]    |
|   pop()   |  [1,4]  |   [3,2,5,6]   |
|   pop()   |   [1]   |  [3,2,5,6,4]  |
|   pop()   |   []    | [3,2,5,6,4,1] |

154623 - no, this order can't be permuted.

| operation | stack     | output    |
| --------- | --------- | --------- |
| push(1)   | [1]       | []        |
| pop()     | []        | [1]       |
| push(2)   | [2]       | [1]       |
| push(3)   | [2,3]     | [1]       |
| push(4)   | [2,3,4]   | [1]       |
| push(5)   | [2,3,4,5] | [1]       |
| pop()     | [2,3,4]   | [1,5]     |
| pop()     | [2,3]     | [1,5,4]   |
| push(6)   | [2,3,6]   | [1,5,4]   |
| pop()     | [2,3]     | [1,5,4,6] |
after the last pop(), we have a problem, if we pop() then 3 will be in the output and not 2.

**S2. Evaluate Postfix Expression.**

$\to$ in a postfix expression operators occurs after the operands, eg. 3 5 +  this is  3 + 5 . we can evaluate a postfix expression using a stack. the approach is to push input when we see a operand i.e a number and pop last two pushed elements from the stack and calculate their result and push it back into the stack if the input is a operator. time complexity $O(n)$ , space complexity $O(n)$.

*pseudocode* - 

```cpp
stack st
for i in tokens :
    if i != '+' AND i != '-' AND i != '/' AND i != '*' :
		st.push(i)
	else :
		x = st.pop
		y = st.pop
		switch i :
			case '+' : st.push(y+x)
			case '-' : st.push(y-x)
			case '*' : st.push(y*x)
			case '/' : st.push(y/x)
return st.top
```

**S3. Next Greater Element Using Stack.**

$\to$ given a array of elements , we can find next greater element of each of the array elements by using a stack and iterating in reverse. if at the current element stack is empty that there is no greater element for current. if top of the stack has a number greater than current than that is its NGE, otherwise we need to pop elements from the stack that are less than the current till its not or the stack is empty. time complexity $O(n)$ , space complexity $O(n)$.

*pseudocode* - 

```cpp
stack st
for i from N-1 to 0 :
	while !st.empty AND st.top <= nums[i] :
		st.pop
	if st.empty :
		res[i] = -1
	else
		res[i] = st.top
	st.push(nums[i])
return res
```

**S4. Remove K Digits to Get Smallest Number.** 

$\to$ 
###### Queues -

**Q1. First Non-Repeating Character in a Stream.**

-> we need to process a stream of characters and find the first non-repeating character at each point. since, we don't have all the characters at the start, we need some data structure to store non-repeating characters in a first come first serve manner. we can use a queue for this purpose. we need another data structure to keep track on frequencies of the characters, this will be used to eliminate characters that are repeating. we can use a array of 26 characters (considering input as only small alphabet letters) or we can use a unordered map. 
the approach to solve this problem is as follows :
for the first character the input character is the first non-repeating, we can push this into result string and into the queue and increase its frequency in the map. for next characters, we can check if it is non-repeating or not via the map (by checking if freq > 1), if it is non-repeating we can push it to queue and push front of queue to result string. if queue is empty we can push some placeholder like '#' to the result. also we need check if the front of queue is non-repeating, we can eliminate repeating characters from queue using a loop. considering only small alphabetical letters as input, the space complexity is $O(1)$ since, at max queue and map will store 26 elements. average case access time complexity of unordered_map is $O(1)$ and in worst case it is $O(n)$. if we consider the average case time complexity of map, this algorithm runs in $O(n)$.

*pseudocode* -

```cpp
string res
queue q
unordered_map mp

for c in s:    // here s is the input string, c represents a char in s
    mp[c]++
    if mp[c]==1 :
        q.push(s[i])
    while !q.empty AND mp[q.front]>1:
        q.pop
    if q.empty:
        res.push_back('#')
    else
		res.push_back(q.front())
    return res
```

