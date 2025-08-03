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

Another approach would be to sort the given array, but this will change the original indices of the elements that add up to the target. we can use a hash_map to store a mapping of elements to their index. duplicated elements won't matter because we just need to return indices of two numbers that add to target. these can be any two numbers.
after building the hash_map, we need to sort the given array and now we can take two pointers $i,j$ pointing to start and end of the array respectively. we the current sum of element at indices $i$ and $j$ is less than target than we can increment $i$ else we need to decrement $j$.
time complexity of this algorithm would be $O(n\log n)$ with a space complexity of $\Theta(n)$.


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

above approach has time complexity $O(n)$.

---

**A2** : Find the length of the longest contiguous subarray whose sum equals a given value K.

$\implies$ 

---
###### Linked List
**L2 :** Remove the n-th node from the end of the list in a single pass.

Given that we need to remove the n-th node in single pass, we can use a hash_map to store the position of a node and a pointer to that node. then we can make a single pass over the linked list to calculate its length . after that, we would only need to modify some pointers to remove the desired element. time complexity - $O(n)$ , where $n$ is the length of linked-list. space complexity if also $O(n)$.

*pseudocode* - 

```cpp

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
