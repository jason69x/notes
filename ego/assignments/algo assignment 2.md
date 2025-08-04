## CS 513 - Assignment 2

Name   : Abhishek Meena
Roll no : 254101002
Course : M.tech CSE (1st yr)
Email   : [abhishekm1002@iitg.ac.in](mailto:abhishekm1002@iitg.ac.in)

---

###### Arrays

**A1** : Given an array and a target value, return indices of the two numbers such that they add up to the target.

$\implies$ A simple approach to this problem would be check all the pairs and find if they add up to the target. running time of this approach would be $O(n^2)$. 

*pseudocode* - 

```cpp
for i from 1 to n-1:
	for j from i+1 to n:
		if arr[i]+arr[j] == target:
			return i,j
```

Another approach would be to sort the given array, but this will change the original indices of the elements that add up to the target. we can use a hash map to store a mapping of elements to their index. duplicated elements won't matter because we just need to return indices of two numbers that add to target. these can be any two numbers.
after building the hash_map, we need to sort the given array and now we can take two pointers $i,j$ pointing to start and end of the array respectively. we the current sum of element at indices $i$ and $j$ is less than target than we can increment $i$ else we need to decrement $j$.
time complexity of this algorithm would be $O(n\log n)$ with a space complexity of $\Theta(n)$.we use a  unordered map as our hash map with average case time complexity of $O(1)$ for insertion,deletion and lookup. however, in the worst case due to collisions and poor hash function, these methods can take $O(n)$ time. but probability of this is very low, given that we use a efficient implementation of hash maps. therefore, below analysis(s) will use average case time complexity for hash maps.


*pseudocode* - 

```cpp
i=0
for ele in arr:
	map[ele] = i++
sort(arr)        // using a sorting algo with TC nlogn
i=1,j=n
while i<j:
	sum = arr[i]+arr[j]
	if sum == target:
		return map[arr[i]],map[arr[j]]
	if sum < target:
		i++;
	else j--;
return -1,-1     //if there are no elements that satisfy condition
```

A approach that doesn't use sorting would be to use a hash_map and check for the sum condition while inserting elements in the hash_map. eg. if we are inserting $arr[i]$ in the map then we can check if the element $target-arr[i]$ already exists in the map, if it does we can return the index of these two elements ( key for the map in element and value is its index ) .

*pseudocode* - 

```cpp
for i from 1 to n:
	diff = target - arr[i]
	if map[diff] exists:
		return i,map[diff]
	map[arr[i]] = i
return -1,-1
```

above approach has time complexity $O(n)$ and space complexity $O(n)$.

---

**A2** : Find the length of the longest contiguous subarray whose sum equals a given value $K$.

**A3** : Given an array of size $n$, find the element that appears more than $⌊n/2⌋$ times.

This can be solved by creating a hash map where key is elements of array and value is their frequency. then we can iterate over this map and return the element whose frequency is greater than $⌊n/2⌋$ . time complexity of this approach is $O(n)$ and space complexity is $O(n)$.

*pseudocode* - 

```cpp
for i = 1 to n:
	map[arr[i]] += 1    // count frequency of element
for key,val in map:
	if val > n/2:
		return key
```

**A4** : For each element in the array, find next greater element to its right. If none exists, return $−1$.

We can iterate over each element of  the array and check the right subarray of the element to find next greater element. time complexity of this approach is $O(n^2)$ .

*pseudocode* -

```cpp
res = [],flag=0
for i = 1 to n:
	for j = i+1 to n:
		if arr[j] > arr[i]:
			res[i] = arr[j]
			flag = 1
	if flag == 0:
		res[i] = -1   //if greater element is not found insert -1
	flag = 0
return res	
```



---
###### Linked List

**L1** : Determine if a cycle exists in a singly linked list.

a simple approach would be to iterate over the list and store address of each node in a set and before adding address of a node check if it is already present in the set or not. if it does then we have a cycle in the given linked list else we will reach the end. this approach has two drawbacks. one that some programming language doesn't provide a method to hash pointers eg. cpp and second that this approach uses additional space.
a better approach is to use floyd's cycle detection algorithm. this algorithm uses two pointers one moves one node at a time (slow/tortoise) and another two nodes at a time (fast/hare). if the list has a cycle, the slow and fast pointers will meet at some node. this works because when both hare and tortoise are in a cycle, the relative distance between them is always decreasing. the hare will not jump over tortoise because the tortoise is moving one step and hare two step, so relative distance is decreasing by one step everytime. time complexity of this approach is $O(n)$.

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

---
**L2 :** Remove the n-th node from the end of the list in a single pass.

Given that we need to remove the n-th node in single pass, we can use a hash_map to store the position of a node and a pointer to that node. then we can make a single pass over the linked list to calculate its length . after that, we would only need to modify some pointers to remove the desired element. time complexity - $O(n)$ , where $n$ is the length of linked-list. space complexity if also $O(n)$.

*pseudocode* - 

```cpp
i=0
temp = head
while head not NULL:
	i++
	map[i] = head    //storing pointer to element i
if i-n == 0:
	return temp->next   //removing 1st element
map[i-n]->next = map[i-n]->next->next
return temp
```


Another approach would be use two pointers. move fast pointer one step at a time $n$ times. then till fast->next is not null, move slow pointer. now, slow pointer will be behind the node we want to remove. lets assume the total number of nodes in list be $L$ . we want to remove the $n^{th}$ element, it will be located at the position $L-n+1$ (eg. 1->2->3->4->5 , L=5, n=2, so 5-2=3 but the element we want to remove is 4 i.e L-n+1). at the start both pointers are at the head, now move the fast pointer $n$ steps ahead, remaing distance to the end will be $L-n$ which is the offset of the node previous to our target node. now if we move both pointers one step at a time till fast->next is not null. the slow pointer will be at previous node to our target and we can remove the target. time complexity $O(n)$, space complexity $O(1)$.

*pseudocode* - 

```cpp
slow = head
fast = head
for i = 1 to n:
	fast = fast->next
if fast == NULL:
	return head->next        // if n = length of list i.e remove head
while fast->next != NULL:
	slow = slow->next
	fast = fast->next
slow = slow->next->next
return head
```

---

**L3** : Determine whether a linked list is a palindrome.

This problem can be solved by storing elements of linked list in a array and comparing the elements of linked list with array elements in reverse. this algorithm has a time complexity $O(n)$. because it does one pass to push elements in the array and another to compare elements of array and linkedlist . both are order of $O(n)$ . space complexity is $O(n)$.

*pseudocode* -

```cpp
i=1
temp = head
while head is not null:
	arr[i++] = head->val
	head = head->next
i=n
while temp:
	if arr[i--]!=temp->val:
		return false
return true
```

Another approach is to find mid of the list and reverse the part after the mid and compare the first part with last part. we can find the mid by using slow and fast pointers and reverse the second part by using a dummy node. this algorithm's running time complexity is $O(n)$ .

*pseudocode* - 

```cpp
if head == NULL or head->next == NULL:
	return true                         //size 0 and 1 list are palindromes
slow = head
fast = head
while fast!=NULL and fast->next!=NULL:
	fast = fast->next->next;
	slow = slow->next;
slow = reverseList(slow);
fast = head
while slow!=NULL:
	if slow->val != fast->val:
		return false
return true
```

---
###### Asymptotics

Indicate, for each pair of expressions (A, B) in the table below, whether A is O(B), o(B), Ω(B), ω(B), or
Θ(B). Assume that k ≥ 1, ϵ > 0, and c > 1 are constants. Your answer should be in the form of a table with “yes” or “no” written in each box. Also, give the reasons.

| Row | Expression A | Expression B   | A ∈ O(B) | A ∈ o(B) | A ∈ Ω(B) | A ∈ ω(B) | A ∈ Θ(B) |
| --- | ------------ | -------------- | -------- | -------- | -------- | -------- | -------- |
| (a) | $\log^k n$   | $n^\in$        | yes      | yes      | no       | no       | no       |
| (b) | $n^k$        | $c^n$          | yes      | yes      | no       | no       | no       |
| (c) | $c^n$        | $cn^k$         | no       | no       | yes      | yes      | no       |
| (d) | $n^{\sin n}$ | $2^{n^{2n/2}}$ | no       | no       | no       | no       | no       |
| (e) | $2^{n/2}$    | $2^n$          | yes      | yes      | no       | no       | no       |
| (f) | $n^{\log c}$ | $c^{\log n}$   | yes      | no       | yes      | no       | yes      |
