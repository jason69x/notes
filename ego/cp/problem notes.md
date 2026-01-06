###### divisors
*fundamental theorem of arithmetic*, for any integer $n>1$ :
- $n$ is either prime or
- $n$ can be written as a product of primes $n = p_1^{a_1}p_2^{a_2}\ldots p_k^{a_k}$
- this representation is unique, except for the order of primes. 

negative numbers can be written as $n =($-1$)\times($pf of $\lvert n\rvert)$  

a positive divisor $d\ |\ n$ must be of the form: $d=p_1^{e_1}p_2^{e_2}\ldots p_k^{e_k}$
where each exponent satisfies $0\leq e_i\leq a_i$

for each prime $p_i^{a_i}$,
- choices for exponent $e_i$ : $0,1,2,\ldots,a_i$
- total choices $a_i + 1$

*divisor count formula*, $d(n) = (a_1+1)(a_2+1)\ldots(a_k+1)$

since every number $n$ can be written as a product of primes, so divisor $d$ of $n$ can also be written as product of primes, power of these primes can be $0$ or $\leq$ power of corresponding prime in p.f of $n$. since, d must be a subset of n. so, for each prime in p.f of $n$ we have its power$+1$ choices. since, each prime can be chosen independently we are multiplying these choices.

**general rule**

prime factorize $n$
multiplicative factorize $d(n)$

if $d(n) = k$ then,
- write all multiplicative factorizations of $k$ into integers $\geq 2$
- subtract $1$ from each factor
- those values are the possible prime exponents of $n$.

eg. $d(n) = 6\ ;\ 6,2\times3 \to p^5$ or $p^1p^2$

$p^3$ - divisors 4 : $1,p,p^2,p^3$
$pq$ - divisors 4 : $1,p,q,pq$


