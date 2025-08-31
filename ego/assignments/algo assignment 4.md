## CS 513 - Assignment 4

Name   : Abhishek Meena
Roll no : 254101002
Course : M.tech CSE (1st yr)
Email   : [abhishekm1002@iitg.ac.in](mailto:abhishekm1002@iitg.ac.in)

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

