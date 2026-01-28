#### Bronze
`array<int,5> arr{}` - all init to 0

`fill_n(vec.begin()+3,4,20)` - 3 to 7 set 20
`fill(vec.begin()+3,vec.end()-2,20)` - 3 to 7 set 20

`v.pop_back()`
`s.substr(idx,len)`

type of an iterator for a ds of type `t` is `t::iterator`

`s.find(x)` - returns a iterator if `x` exist else `s.end()`

`lower_bound(x)` - returns an iterator to the smallest element in the set whose value is *at least* `x`.

`upper_bound(x)` - returns an iterator to the smallest element in the set whose value is *larger than* `x`.

else they both returns `end()`. only supported in ordered set.

`getline(cin>>ws,name)`  - ws ignores leading whitespace

`tuple<t1,t2,...,tn> t` 
`make_tuple(a,b,...,n)`
`get<i>(t)`
`tie(a,b,..,n) = t`
`t = tie(a,b,c)`
`get<2>(t) = 7`

`next_permutation(v.begin(),v.end())` - $O(n)$

`count(s.begin(), s.end(), 'i')`
$O(\log{n})$ for set,map -> 0/1.
`count_if(nums.begin(),nums.end(),[](int x){return x<3;});` - count with cond.

`" \n"[i==n-1]`
