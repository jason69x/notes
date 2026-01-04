
#### time complexity

time complexity estimates how much time a algorithm will take for some input.
idea is to represent efficiency of a algorithm as a function whose parameter is the size of input.

slowest phase is usually the bottleneck of the program.

time complexity of a recursive function depends on the number of times the function is called and the time complexity of a single call.

```cpp
void g(int n) {
	if (n==1) return;
	g(n-1);
	g(n-1);
}
```

![[tc_recu.png|300]]

![[tc.png|400]]

if an algorithm has an $O(n^2)$ block and an $O(n\cdot m)$ block, the overall time complexity would be $O(n^2 + n\cdot m)$.

count simultaneous stack space.

