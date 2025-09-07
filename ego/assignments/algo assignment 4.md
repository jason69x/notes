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

**S5. check cycle using stack.**

$\to$ we can use a stack, by iterating over the linked list and for each node, we check if the stack already contains this node or not, if it does then there is a cycle else we push this node on the stack. if at any point we encounter a null pointer, we can conclude that there is no cycle. to check if a node is already present in the stack, we will be using a temporary stack to copy all the elements of the main stack and check using standard operations of stack. time complexity $O(n^2)$ , space complexity $O(n)$. 

*pseudocode* -

```cpp
stack st
curr = head
while curr != NULL :
	temp_st = st
	while temp_st is not empty :
		if temp_st.top == curr:
			return true         // there is cycle
		temp_st.pop
	st.push(curr)
	curr = curr->next
return false           
```

**S6. find intersecting point to two list using stack.**

$\to$ yes, we can use stack. we will use two stacks, one for each linked list, say stack1 and stack2. first, we will iterate over the list1 and push all nodes in the stack1 and do the same for the list2. if the lists intersects there last node will be the same. we can use this fact. we will use a dummy variable called last to store pointer to a node, initially it will be NULL. after creating the stack, we will pop elements from both the stack simultaneously, if the node's are same we store it in last and go to the next pop operation. if they intersect , at some point the popped nodes will differ, the intersecting point will be the node that is stored in the last variable. if they don't intersect, last will remain NULL, hence at the end we will know if they intersect or not and what is the intersecting point. time complexity $O(n+m)$ and space complexity $O(n+m)$ , where $n$ and $m$ are lengths of the two linked lists.

*pseudocode* -

```cpp
stack s1
stack s2
curr1 = head1
curr2 = head2

while curr1 != NULL : 
	s1.push(curr1)
	curr1 = curr1->next
while curr2 != NULL :
	s2.push(curr2)
	curr2 = curr2->next

last = NULL

while s1 not empty AND s2 not empty AND s1.top == s2.top :
	last = s1.top
	s1.pop
	s2.pop
	
return last
```
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

**Q3. Generate Binary Numbers from 1 to N.**

$\to$ if we observe carefully we can see a pattern in binary representation of numbers from $1$ to $N$.
{1 -> 1 , 2 -> 10 , 3 -> 11 , 4 -> 100 , 5 -> 101 , 6 -> 110 , 7 -> 111, ...}
here binary rep. of 2 and 3 can be produced by just adding 0 and 1 after binary rep. of 1, 4 and 5 can be obtained by adding 0 and 1 in binary rep. of 2. to exploit this pattern we can use a queue.
first we enque binary rep. of 1 in queue. when we deque front element from the queue, we will enque its binary rep. + 0 and 1 into the queue. now we just have to repeat this  procedure for n times and output the dequed element each time. time complexity $O(n)$ and space complexity $O(n)$. 

*pseudocode* - 

```cpp
queue q
q.enque("1")

for i from 1 to N:
	bin = q.deque
	q.enque(bin + "0")
	q.enque(bin + "1")
	print(bin)
```


**Q4. check cycle using queue.**

$\to$ we can use a queue, by iterating over the linked list and for each node, we check if the queue already contains this node or not, if it does then there is a cycle else we push this node on the queue. if at any point we encounter a null pointer, we can conclude that there is no cycle. to check if a node is already present in the queue, we will be using a temporary queue to copy all the elements of the main queue and check using standard operations of queue. time complexity $O(n^2)$ , space complexity $O(n)$. 

*pseudocode* -

```cpp
queue q
curr = head
while curr != NULL :
	temp_q = q
	while temp_q is not empty :
		if temp_q.front == curr:
			return true         // there is cycle
		temp_q.deque
	q.enque(curr)
	curr = curr->next
return false     // no cycle
```


**Q5. find intersecting point to two list using queue.**

$\to$ yes, we can use deque. we can also use normal queue's , but then we will have to reverse the queues in order to find the intersecting point. we will use two deque, one for each linked list, say deque1 and deque2. first, we will iterate over the list1 and enque at the back all nodes in the deque1 and do the same for the list2. if the lists intersects there last node will be the same. we can use this fact. we will use a dummy variable called last to store pointer to a node, initially it will be NULL. after creating the deque, we will deque elements from both the deque simultaneously from the back of the deque, if the node's are same we store it in last and go to the next deque operation. if they intersect , at some point the popped nodes will differ, the intersecting point will be the node that is stored in the last variable. if they don't intersect, last will remain NULL, hence at the end we will know if they intersect or not and what is the intersecting point. time complexity $O(n+m)$ and space complexity $O(n+m)$ , where $n$ and $m$ are lengths of the two linked lists.

*pseudocode* -

```cpp
deque d1
deque d2
curr1 = head1
curr2 = head2

while curr1 != NULL : 
	d1.enque(curr1)
	curr1 = curr1->next
while curr2 != NULL :
	d2.enque(curr2)
	curr2 = curr2->next

last = NULL

while d1 not empty AND d2 not empty AND d1.top == d2.top :
	last = d1.top
	d1.pop
	d2.pop
	
return last
```