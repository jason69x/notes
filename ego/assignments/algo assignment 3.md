## CS 513 - Assignment 3

Name   : Abhishek Meena
Roll no : 254101002
Course : M.tech CSE (1st yr)
Email   : [abhishekm1002@iitg.ac.in](mailto:abhishekm1002@iitg.ac.in)

###### Linked Lists - 

**L1** : As an extension to the previous assignment question, consider the following question: check whether the given linked list is either NULL-terminated or ends in a cycle.

(a) What would be the brute force way of solving it?

-> we can try iterating over the list till we find a null node, but this can lead to a infinite loop if there is a cycle. one way to eliminate this problem is by using a timer. if program runs more than this time then we have a cycle. lets say we have $100\ GB$ of ram. each node has size $20\ bytes$. number of elements would be $5$ billion. if a cpu has clock cycle speed of $5\ Ghz$ it can process these elements in approximately $1$ sec . if it takes more than $3$ or $4$ seconds then there is a cycle. we can calculate for each cpu and ram configuration the approximate amount of time program should take. but this approach is not accurate in all cases.
Another approach would be iterate over linked list and for each node start from the head and see if the address of current node appears again when iterating from head to the current node. if it does then we have a cycle. time complexity $O(n^2)$.


(b) Can you use hashing technique to solve this problem?

-> Yes, we can create a hash table that stores address of each node. then we can iterate over the linked list and check if the address is already present in the hash table or not. if it is then we have a cycle and if we encounter a NULL then no cycle.  time complexity $O(n)$.  space complexity $O(n)$.

(c) Can you use sorting to solve this problem?

-> yes, we can use sorting. we can iterate over the list and insert the address of  current node to a array $a$ . then we can sort the array and iterate over it to check if we have a pair where $a[i-1] = a[i]$ , this is only possible if we have a cycle. we can then move on to the next element in the list and do the same for it. time complexity is $O(n^2\log(n))$, space complexity is $O(n)$. 

(d) Can you design an algorithm for the problem that runs in O(n) time?

we can use floyd's cycle detection algorithm. this algorithm uses two pointers one moves one node at a time (slow/tortoise) and another two nodes at a time (fast/hare). if the list has a cycle, the slow and fast pointers will meet at some node. this works because when both hare and tortoise are in a cycle, the relative distance between them is always decreasing. the hare will not jump over tortoise because the tortoise is moving one step and hare two step, so relative distance is decreasing by one step everytime. time complexity of this approach is $O(n)$.

*pseudocode* - 

```cpp
if head == NULL or head->next == NULL:
	return false
slow = head
fast = head->next->next
while fast!=NULL and fast->next!=NULL:
	if(slow==fast) return true
	slow = slow->next
	fast = fast->next->next
return false
```


**L2 :** Add two numbers represented by linked lists, where each node contains a single digit.

-> assuming the list contains digits in reverse order. if they aren't we can use method shown in problem **L4** to reverse them. then we just need to iterate over the list and add the two digits and maintain carry. $sum = dig1 + dig2 + carry$ and $carry = sum/10$ , the final digit that need to be stored in result list would be $sum\%10$.


```cpp
carry = 0,sum = 0
ListNode *n1=list1,*n2=list2
ListNode *head = NULL
ListNode *tail = NULL
while n1 != NULL || n2 != NULL || carry != 0 :
    val1 = n1!=NULL ? n1->val : 0
    val2 = n2!=NULL ? n2->val : 0
    sum = val1 + val2 + carry
    carry = sum/10
    if n1!=NULL : n1 = n1->next
    if n2!=NULL : n2 = n2->next
    if head == NULL :
        head = new ListNode(sum%10)
        tail = head
    else :
        tail->next = new ListNode(sum%10)
        tail = tail->next
return head
```

---


**L3.** Determine the node at which two singly linked lists intersect.

(a) What would be the brute force way of solving it?

-> we can iterate over the first list and for each element of this list traverse over the second list and check if they have the same address or not. time complexity $O(n*m)$ .

(b) Can you use hashing technique to solve this problem?

-> we can create a hash set and iterate over the first list and insert the addresses of each node in the set and then we can iterate over the second list and check if the address of the current node is already present in the set or not. if it is then the given lists intersect at some point else they don't.
time complexity $O(n+m)$  and space complexity $O(length\ of\ first\ list)$.

(c) Can you use sorting to solve this problem?

-> yes, we can iterate over both the lists and insert address of each element to a array and sort this array. then we can iterate over this array and check if two consecutive elements have same address or not. if they do then lists intersects else they don't. time complexity $O((n+m)\log(n+m))$ and space complexity $O(n+m)$ .

(d) Can you design an algorithm for the problem that runs in O(n+m) time, where n and m denotes
the number of nodes before the first intersecting node of the linked lists?

-> suppose the given lists intersects at some point and they have same length, we can easily find the intersecting point by just traversing over lists one node at a time. but if they have different length, we can't use this approach. we can find the difference in their length and adjust the larger list according to it, then both the strings will go in sync till the intersecting point.if there is not intersecting point then one of the node will be NULL and we can return that the given list's doesn't intersects. time complexity of this approach is $O(max(n,m))$ where $n$ and $m$ are lengths of the given lists.

```c++
getLength(head):
    res=0
    if head==NULL : return 0
    if head->next == NULL : return 1
    while head!=NULL :
        res++
        head = head->next
    
    return res

getIntersectionNode(headA,headB) :
        l1 = getLength(headA),l2 = getLength(headB)
        n = abs(l1-l2)
        while n-- :
            if l1>l2 :
                headA = headA->next
            else :
                headB = headB->next
        while headA!=NULL AND headB!=NULL :
            if headA == headB :
                return headA
            headA = headA->next
            headB = headB->next
        return NULL
```

(e) Is it possible to use stacks?

-> yes, we can use two stacks and push elements of these two lists in these two stacks respectively.
then we can pop from both the stack if the elements have same address until they don't. the last popped element is the intersecting point. if during the pop phase if one of the stack becomes empty then they don't intersect. time complexity $O(n+m)$ and space complexity $O(n+m)$. 

---

**L4 :** Reverse a singly linked list. What will you do if you are asked to reverse a doubly linked list?

-> we can use a dummy node (prev - a pointer to NULL) and reverse the list. first we store address of next node of head in a temporary pointer (temp) and make head point to prev node. make prev point to head and head to temp. repeat this process till head becomes NULL and return prev. time complexity $O(n)$.

*pseudocode -*

```cpp
if head == NULL || head->next == NULL : 
	return head
prev = NULL 
while head!=NULL :
    temp = head->next
    head->next = prev
    prev = head
    head = temp
return prev
```

to reverse a doubly linked list, we just have to exchange previous and next pointers and return the last node. time comlexity $O(n)$.

*pseudocode -*

```cpp
if head is NULL OR head->next is NULL : 
	return head
temp,res
while head!=NULL :
    res = head
    temp = head->next
    head->next = head->prev
    head->prev = temp
	head = temp
return res
```


---
###### Stack -

**S1.** Min Stack : Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

-> we can use two stacks to implement this functionality, one stack works as a normal stack to push,pop elements and another for storing the current minimum among the elements that are in the normal stack. when we pop a element from normal stack we also pop it from minimum stack if it is the top element. when we push a element to normal stack , we check if it is less than or equal to the current minimum (<= because we can have duplicate elements), if it is then we push this element onto the min stack. time complexity $O(n)$ for $n$ operations , space complexity $O(n)$ , time complexity for individual operations $O(1)$.

*pseudocode -*

```cpp
stack normal
stack min 

push(val) : 
normal.push(val)
if min.empty || min.top >= val
    min.push(val)
            
pop() :
if normal.top == min.top:
    min.pop
normal.pop
    
top() :
return normal.top
    
getMin() :
return min.top
```

---

**S2.** Valid Parentheses : Given a string containing only brackets, determine if it is valid (balanced and closed properly).

-> if by brackets we mean only **'('**  and **')'** . then this problem can be solved in $O(n)$ by iterating over the string and keeping count of occurences of **'('** parentheses. if count becomes less than $0$ , then number of closing parentheses is greater than opening and we return *false*. at the end if count is $0$ , we can return *true*.

*pseudocode -*

```cpp
count = 0
for char in string:
	if char == '(':
		count++
	else:
		count--
		if count < 0:
			return false
return count == 0
```

if brackets $= (,\{,[,),\},]$ , then we can use a stack and iterate over the string. if the current character is a open bracket , we can push it on the stack else we check if the stack.empty or top of the stack has its corresponding opening bracket. if stack is empty we return false. because more number of closing brackets and if top of stack in not the correspoding bracket then we return false. if top is its opening bracket then we continue with the next iteration. time complexity $O(n)$ and space complexity $O(n)$.

*pseudocode -*

```cpp
stack st
for c in string:
    if c=='(' || c=='{' || c=='[':
        st.push(c)
    else:
        if st.empty
            return false
        if c==')' && st.top!='(':
            return false
        if c=='}' && st.top!='{':
            return false
		if c==']' && st.top!='[':
            return false
        st.pop
return st.empty
```

---
###### Queues - 

**Q1.** Implement a queue with enqueue and dequeue operations using two stacks.

-> let s1,s2 be two stacks. for enque operation we can just push elements in the s1 stack and to deque, if s2 stack is empty then pop all elements from s1 and push into s2 then returnand pop the top element of s2. if s2 is not empty just pop and return the top element of s2. time complexity of enque operation is $O(1)$ and for deque is $O(n)$.

*pseudocode - 

```cpp

stack s1
stack s2

push(x) :
    s1.push(x)

pop() :
    if s2.empty :
        while not s1.empty :
            s2.push(s1.top)
            s1.pop
    t = s2.top
    s2.pop
    return t
        
peek() :
    if s2.empty :
        while not s1.empty :
            s2.push(s1.top)
            s1.pop
    return s2.top
    
empty() :
	return s1.empty AND s2.empty
```

---