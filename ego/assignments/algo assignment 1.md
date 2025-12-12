## CS 513 - Assignment 1


Name   : Abhishek Meena
Roll no : 254101002
Course : M.tech CSE (1st yr)
email   : [abhishekm1002@iitg.ac.in](mailto:abhishekm1002@iitg.ac.in)

---
###### Problem 1 - 

**Input :** An array A of $n$ integers.
**Output :** A sorted array A.

1.1 *implementation using insertion sort* :

insertion sort uses the idea similar to how we sort a deck of cards. lets assume we have a unsorted array $A$ . The main idea of insertion sort is that it picks element from the array 1 by 1 and inserts them into their correct place by shifting elements in already sorted part of the array to the right and making a room for the element. let's say we start with the first element then to its left there are no elements, we don't need to compare it with any element (since a single element is already sorted). now, for the next element we need to check whether it is $<$ the element to its left. if it is, then we need to shift elements to right till we find a correct place for this element and if it isn't then we can add it to the sorted part of the array ( move pointer to the next element ).

*pseudocode* : 

```cpp
  for i = 1 to n-1:
    card = arr[i]               // selecting a element to insert
    j = i-1
    while j>=0 and arr[j]>card   
      arr[j+1] = arr[j]          // shifting elements from left to right
      j--
    arr[j+1] = card;            // inserting element in its sorted place
```

*in-place algorithms* - 

algorithms which do not use external data structures other than what was provided as input to produce output. they may use some variables or $O(1)$ space.

*stable sorting algorithms* - 

algorithms which preserve the relative ordering between elements sharing the same value in given input.

insertion sort is a **in-place** sorting algorithm and it is **stable**. it is stable because it inserts elements in sorted part from left to right in the array. elements sharing same value will have the same relative position after sorting. since, the element at the left most will be inserted first in the sorted part and then the rest of the elements in left to right order. therefore, relative order is preserved.

*worst case analysis* - 

suppose that the input array is sorted in reverse order. $arr = [5,4,3,2,1]$ , when using insertion sort on this array, element at index $1$ will be compared with $1$ element to its left and index $2$ element will be compared with $2$ elements. similarly index $n-1$ will be compared with $n-1$ elements for proper insertion to its correct sorted place. 
The outer loop runs $n$ times from $i = 1$ to $n-1$ and for each of these iteration the inner loop runs  $1,2,\ldots n-1$ times. summation of these values gives us :

$1 + 2 + 3 + \ldots + n-1 = \frac{n(n-1)}{2}$ 

This is asymptotically proportional to $\Theta ( n^2 )$.

*best case analysis* - 

best case occurs when input is already sorted in ascending order. in this case the inner loop's condition $arr[j]<key$ is always *false* . hence, for each iteration of outer loop , the inner loop gets terminated after checking the condition i.e it can be counted as a single operation and it doesn't depend on anything. therefore, in this case insertion sort will run in order of $\Theta(n)$.

*loop invariant* :

* initialization - 

at the start of the loop, $arr[0\ldots i-1]$ is sorted. for $i=0$, $arr[0\ldots 1]$ contains a single element and a single element is already sorted.

* maintenance -

before start of every loop, $arr[0\ldots i-1]$ is sorted and after loop iteration, before the next iteration begins $arr[0\ldots i-1]$ is sorted (here $i$ was incremented $i.e$ a new element has been added to the sorted part of array).

* termination -

after the loop terminates $i=n$ , $arr[0\ldots i-1]$ is sorted. that is, our input array $arr[0\ldots n-1]$  is sorted after the loop ends and the loop invariant condition also holds.



1.2 *implementation using selection sort* -

Idea of selection sort is that it finds minimum element in unsorted part array and replaces it with element just after sorted part of the array (initially empty array). so it first replaces $a[0]$ with minimum element in array. then it finds minimum element in $a[1\ldots n]$ then replaces it with $a[1]$ and so on till $n-2$ , since all the numbers in range $a[0\ldots n-2]$ are sorted, element $a[n-1]$ is already in its correct sorted place.  

*pseudocode* : 

```cpp
  for i = 0 to n-2:
    min = i
    for j = i+1 to n-1:
      if arr[j]<arr[min]
        min = j
    swap(arr[i],arr[min])
```

selection sort is a **in-place** algorithm. it is a **unstable** algorithm. 

assume a array $A = [5_a,4,3,5_b,6,1]$ , after finding minimum element $(1)$ in this array, selection
sort will swap the first occurence of $5$ with $1$ . Now the array $A$ is $[1,4,3,5_b,6,5_a]$ . Here, the relative ordering of same valued element is disturb. Therefore, selection sort if not stable.

*analysis* -

the runtime of selection sort is proportional to $\Theta(n^2)$ . because it checks the unsorted part of array every time to confirm the minimum element of the array. even if the input array is already sorted. to the find the first minimum element , it performs $n-1$ comparisons, then it performs $n-2$ comparisons and so on till $1$ comparison is needed. 

$1 + 2 + 3 + \ldots + n-1 = \frac{n(n-1)}{2}$ 

therefore, it performs in $\Theta(n^2)$ in both worst and best case.

*loop invariant* :

* initialization - 

at the start of the loop, $arr[0\ldots i-1]$ is sorted. for $i=0$, $arr[0\ldots 1]$ contains a single element and a single element is already sorted.

* maintenance -

before start of every loop, $arr[0\ldots i-1]$ is sorted and after loop iteration, before the next iteration begins $arr[0\ldots i-1]$ is sorted (here $i$ was incremented i.e a new element has been added to the sorted part of array).

* termination -

after the loop terminates $i=n$ , $arr[0\ldots i-1]$ is sorted. that is, our input array $arr[0\ldots n-1]$  is sorted after the loop ends and thus loop invariant conditions holds.

1.2 *implementation using bubble sort* -

bubble sort works by swapping adjacent elements of array if they are not in sorted order. this algorithm works because by repeatedly comparing ( $arr[i]>arr[i+1]$ ) and swapping consecutive elements, the greatest element in the array will be at the last index after first iteration. after second iteration, $2_{nd}$ maximum element in array will be at the second last index in array.
similarly after $n-1$ iteration the array will be sorted.

*pseudocode* : 

```cpp
  for i = 0 to n-2:
    for j=0 to n-i-2:
      if arr[j] > arr[j+1]:
        swap(arr[j],arr[j+1])
```

bubble sort is a **in-place** algorithm and it is **stable** sorting algorithm.
consider a array $[4_a,-1,2,4_b,4_c,0]$ here when $4_a$ starts moving to the right in the array, it will be compared with $4_b$ and it won't move past $4_b$ and similarly $4_b$ won't move past $4_c$ . because, we are only swapping elements if $arr[j]$ is strictly greater than $arr[j+1]$. thus, relative ordering between same values elements is preserved.

*analysis* - 

in first iteration of the outer loop the inner loop runs $n-1$ times , in second iteration it run $n-2$ times and so on till $1$ . similar to above algorithms , we can calculate bubble sorts time complexity as $\Theta(n^2)$ .
In ordinary bubble sort, if the input array is already sorted its time complexity remains same.
but if we create a check to see if our algorithm made a swap operation, we can get a time complexity of $\Theta(n)$ in the best case (a sorted array).

*loop invariant* :

* initialization - 

before the start of first iteration $arr[n-i\ldots n]$ (array of length $0$ and $n$ is not included) is sorted.

* maintenance -

after the end of a iteration $arr[n-i\ldots n]$ is sorted and before the start of next iteration it is sorted because bubble sort just brought the $(i+1)_{th}$ maximum element in array to the front of previously sorted part of array.

* termination -

at the end of loop $arr[n-i(0)\ldots n]$ is sorted. therefore, loop invariant holds.

---
###### Problem 2 - 

**Input :** A sequence of $n$ numbers $A = <a_1,a_2,\ldots,a_n>$ and a value $x$.
**Output :** An index $i$ such that $x=A[i]$ or $NIL$ if $x$ does not appear in $A$.

We can use a linear search to find $x$, this works by comparing each element of array from $[1\ldots n]$ with $x$. if we find a match we can return that index and if not we can return $NIL$. 

*pseudocode* : 

```cpp
for i from 1 to n :
	if A[i] == x
		return i
return NIL
```

This algorithm works in  order of $\Theta(n)$ time. this is because it only requires one loop to iterate over all the $n$ elements of the array.

*loop invariant :*

* initialization -

at the start of the loop $i=1$ , the subarray $[1\ldots i-1]$ is empty and it doesn't contain key $x$.

* maintenance -

before the start of the loop the subarray $[1\ldots i-1]$ doesn't have key $x$ and after the loop iteration ends, if the key matches the element index $i$ is returned and if it doesn't, i is incremented and the subarray $[1\ldots i-1]$ still doesn't contain key $x$ . therefore, the variant holds.

 * termination - 

loop terminates either when we found a match and return the index or when $i$ reaches the end of the array. In the later case, when $i=n+1$ , the subarray $[1\ldots i-1]$ doesn't have the key $x$.
so, our variant holds at the termination of the loop.

if search key $x$ is in given array , our algorithm will return the index and if it is not our algorithm checks each element of the array and can give assurance that $x$ is not in the array.

---
###### Problem 3 - 

**Input :** An integer $n$
**Output :** A binary representation of $n$

This can be achieved by repeatedly taking mod of $n$ by $2$ and dividing $n$ by $2$ till $n$ reaches $0$.
The above method works because we can write any integer $n$ as -

$b_k*2^{k-1} + b_{k-1}*2^{k-2} +\ldots b_1*2^0$

here, b represents a bit in binary representation of a integer $n$. 1 is the least significant bit and $k$ is the most significant. if we divide this equation by 2 (same as dividing $n$ by 2), we can take $2$ common from the first $k-1$ terms and since this part is multiple of 2, it is divisible by 2. the remaining part is the remainder i.e we got the least significant bit as a remainder of this division. similarly, we can get all bits of any integer by this method.

*pseudocode* : 

```cpp
  arr = [1...log(n)+1]     // total bits needed to represent any int in binary
  i = floor(log(n)+1)      // is equal to floor(log(n)+1)
  while(n!=0){
    if n%2:
	    arr[i] = 1
    else
	    arr[i] = 0
    n/=2
    i--
```

The above algorithm has a time complexity proportional to $\Theta(\log(n))$ . because in each step we are dividing $n$ by $2$ until its equal to $0$. there are $log_2(n)$ levels, cost at each level is $O(1)$.

*extension of above problem* - 

given two $n$-bit binary integers and their binary representation stored in two $n$-element arrays $A$ and $B$  , calculate their sum and store the sum in binary form in a third array $C$ of size $n+1$.

The given problem can be solved with the help of equation's of a full-adder circuit. this circuit is used to add 3 bits. we can use this to add two bits of array $A$ and $B$ and third bit of the *carry* of previous additions. initially carry is $0$. for calculating the sum we can use equation $sum = a\oplus b\oplus c$ .
where, a and b are bits of arrays $A$ and $B$ respectively and c is the carry.
to calculate carry we can use $carry = c\land(a\oplus b)\lor (a\land b)$ .

*pseudocode* : 

```cpp
  carry = 0
  for i = 1 to n:
    sum = b1[i] XOR b2[i] XOR carry       
    carry = carry AND (n1[i] XOR n2[i]) OR (n1[i] AND n2[i])
    b3[i] = sum    
  b3[n+1] = carry
```

considering we are given integers, assuming each of $32$ bits and bit operations take constant time, the time complexity of the above algorithm is $O(1)$. but if we are given a $n$ bit array, the time comlexity becomes $O(n)$.

*formulation as decision problem* - 

given two array's containing binary representation of integers, does their sum in binary representation contains alternating 1's and 0's from left to right starting from 1 eg. 10101010


---
###### Problem 4 - 

*statement:* Express the function $\frac{n^3}{1000} - 100n^2 + 3$ in terms of $\theta$-notation.

$\implies$When using $\Theta$ notation to express a function, we only focus on the highest degree term in the function and ignore all the constants. the above function can be expressed in theta notation as,

$\Theta(n^3)$ , 

which means that worst case running time complexity of this function is proportional to $n^3$ when $n$is large.

*formally*, there exists $c_1,c_2,n_0 > 0$ such that

$0\leq c_1g(n) \leq f(n) \leq c_2g(n)$ , for all $n \geq n_0$

here, taking $c_1$ as -1 , $c_2$ as 1 and $n_0 \geq 1000$  will satisfy the above condition.

---
