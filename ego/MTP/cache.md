#### cache
above pipeline assumed memory can be accessed in a single clock cycle.
but large memories are slow and requires many cycles.
on cache miss, pipeline is stalled until data is available.

processor communicates to memory over a memory interface.
the processor sends an address over *Address* bus. for read *MemWrite* is 0 and the memory returns the data on *ReadData* bus.
for write MemWrite is 1 and processor provides data to memory on the *WriteData* bus.
ReadData bus is not logically disabled on write rather it is not used.

![[mem_interface.png|300]]

by keeping the books that you have recently used or might likely use in future, you reduce number of time-consuming trips to the stacks.

*temporal locality* means that if you have used a book recently, you are likely to use it again soon.

*spatial locality* means that when you use one particular book, you are likely to be interested in other books on the same shelf.

you obtain benefits of both large memory and quick access using a heirarchy of storage.

computers store most commonly used instructions and data in faster smaller memory, called a *cache*.
cache is usually built out of SRAM on the same chip as the processor.

if processor requests data that is available in the cache, *cache hit*
else *cache miss*, retrieves from DRAM

hard drives provides an illusion of more capacity than actually exists in main memory, it is thus called *virtual memory*.
main memory works as cache for hdd.

$hit\ rate = \frac{hits}{total\ mem\ access}$ 

![[AMAT.png|300]]

*Amdahl's Law*
the effort spent on increasing performance of a subsystem is worthwhile only if the subsystem affects a large percentage of overall performance.

number of data words cache can hold is called *capacity* `C`

if the cache is missed , the processor fetches data from main memory and places it in cache for future use.
to accommodate new data, old data must be replaced.

capacity `C`, number of sets `S`, block size `b`, number of blocks `B`, degree of associativity `N`

when the processor access a piece of data, it is also likely to access data in nearby locations. therefore, when the cache fetches one word from memory, it may also fetch several adjacent words. this group of words is called a *cache block* or *cache line*.

number of words in cache block , `b` is called block size.
a cache of capacity `C` contains $B=\frac{C}{b}$ blocks.

cache is organized into `S` sets, each of which holds one or more blocks of data.
the relationship between address of data in main memory and location of that data in the cache is called *mapping*.
each memory address maps to exactly one set in the cache.
some of the address bits are used to determine which cache set contains the data.
if the set contains more than one block, data may be kept in any of the blocks in the set.

caches are categorized based on number of blocks in a set.
in a *direct mapped* cache, each set contains exactly one block.

in a *N-way associative* cache, each set contains N blocks.
addresses maps to unique set, but data can go in any block in the set.

a *fully associative* cache, has only $S=1$ set. data can go in any of the $B$ blocks in the set.
###### direct mapped cache
![[direct_mapped_cache.png|600]]
the bottom two bits of address are always 00, because they are word aligned.
the next $log_2{S}$ bits indicate set onto which memory address maps.
each main memory address maps to exactly one set in cache.

because many addresses map to a single set, the cache must also keep track of the address of the data actually contained in each set.
the least significant bits of address specify which set holds the data.
the remaining most significant bits are called the *tag* and indicates which of the many possible addresses is held in that set.

the two least-significant bits of 32bit address are known as *byte offset*

cache uses a *valid bit* for each set to indicate whether the set holds meaningful data. if the valid bit is 0, the contents are meaningless.
![[directmappedcache.png|400]]

Tag comparison:
- Each stored tag is fed into a comparator circuit
- Comparator = XOR gates + NOR gate
    - XOR each bit of incoming tag with stored tag
    - If all bits match → XOR outputs all 0
    - NOR of all XOR outputs → 1 (hit)

if tag matches and valid bit is set, cache hits and data is returned to the processor, else data must be fetched from main memory.

in set associative cache, there are N comparators (one per way), outputs go into a priority encoder/ mux control. if any comparator outputs 1 -> hit. that way's data block is selected via multiplexer.
everything is fully parallel, thats why cache is fast.

when two recently accessed address map to the same cache block, a *conflict* occurs, and the least recently accessed address *evicts* the previous one from the block.
direct mapped caches have only one block in each set, so two addresses that map to the same set always cause a conflit.

###### multi-way set associative cache
an n-way set associative cache reduces conflicts by providing N blocks in each set where data mapping to that set might be found.
each memory address still maps to a specific set, but it can map to any one of the N blocks in the set.
![[two_way_set_asso.png|500]]

the cache reads blocks from both ways in the selected set and checks the tag and valid bits for a hit. if a hit occurs in one of the ways, a multiplexer selects data from that way.

set associative caches generally have less conflicts then direct mapped cache of same size.
but are slower and expensive then direct.

You don’t rely only on Hit1. You use two things: 1) Way select = Hit1 (since only one of Hit0/Hit1 can be 1 on a valid hit). 2) Overall Hit = Hit0 OR Hit1. The data mux may still output Way0 when Hit1=0, but that output is ignored if Hit=0. The control logic uses Hit to decide whether the data is valid. If Hit=0 (miss), the processor ignores the mux output and starts a memory access instead. So even though the mux selects something, it is not used unless Hit=1.
###### fully associative cache
a *fully associative* cache contains a single set with *B* ways.
![[fully_associative.png]]
**?** use hot muxing to select data after comparing tags, ANDing with valid, ORing result to find hit/miss.**?**
###### block size
previous examples were able to take advantage of only temporal locality
coz block size was one word.
a larger block size means less blocks, may lead to more conflicts, but in general real programs benefit from large block sizes

the time required to load the missing block into the cache is called *miss penalty*.
![[direct_block_size.png|600]]
all words in a block have same tag, use *block offset* to select word needed.
only one tag is needed for the entire block, because the words in the block are at consecutive addresses.

On the first loop iteration, the cache misses on the access to memory address 0x4. This access loads data at addresses 0x0 through 0xC into the cache block.

caches are organized as 2d-arrays, the rows are called sets, and columns are called ways. each entry consists of a data block and its associated valid and tag bits.
![[cache_organization.png|400]]

data replacement,

in direct mapped cache, if a set is full it needs to be replaced.
in set and fully associative cache, we must choose which way to evict.
principle of temporal locality suggest that we should remove set which was *least recently used (LRU)*.

in a 2-way set ac, a *use* bit `U`, indicates which way whithin a set was least recently used.
tracking last used becomes difficult in case of more than 2-way set ac.
to takcle this, ways are divided into two groups, use bit indicates which group of ways was least recently used, during replacement the new block replaces a random way from lru group of ways.
this method is known as *pseudo-LRU*.
![[use_bit_lru.png|400]]

cache misses can be reduced by changing capacity, block size, and/or associativity. first understand cause of misses.
can be compulsory, conflict, capacity.

Changing cache parameters can affect one or more type of cache miss. eg, increasing cache capacity can reduce conflict and capacity misses, but it does not affect compulsory misses. On other hand, increasing block size could reduce compulsory misses (due to spatial locality) but might actually increase conflict misses (because more addresses would map to the same set and could conflict).

![[write_policy.png|600]]

---
word size(data path/ register width)

for smaller associativity, True LRU can be implemented using an Order Matrix. for 4-way SAC, hardware stores `4x4` matrix of bits excluding diagonals.

store which block was used more recently than another.
`M[i][j] = 1` way $i$ was used more recently than $j$.

row with all zeroes is LRU way.

when accesing $W_i$ update `M[i][j]=1 & M[j][i]=0` $\forall_{i\neq j}$

total bits used $N\times (N-1)$ 

PLRU-m : https://github.com/karlmcguire/plru

![[PLRU-Tree based.png|500]]

random, used where cache hit/miss are unpredictable or design needs to be simple.

MFU, used when older data is frequently used rather than newer data.

adaptive replacement, 1/4 - algo1 1/4 - algo2, 1/2 - based on results of these two algo, whichever performs best.

True-LRU, bits required = $\lceil\log_2(n!)\rceil$ , $4-way = 5bits$
state transition logic is complex because every access requires decoding, computing, encoding a permutation.

Pseudo-LRU, bits required = $n-1$ , $4-way=3bits$
simple tree traversal.

NRU, not RU, 

*write policies*

write hit:
- write through - write to both cache and main memory
- write back - write to cache, when evicting write to MM (dirty)

write miss:
- write allocate - bring block from MM to cache then write
- write around - directly write a word to MM 

cpu reads/writes a word, cache fetches/writes a block/line

cache hardware reads entire cache line, but cpu receives only the specific word using block_offset.

simulations:
- trace-based, reading a pre recorded trace of instructions
- execution-driven, directly execute program's instructions

continuous sequential access direct mapped get miss on all.

*cache misses*

slows down retrieval and increase server load.
cause of cache misses is important to minimize them and improve system performance

- compulsory 
- conflict
- capacity

compulsory,

occurs when data is accessed for the first time and has not yet been cached. mitigation strategies includes:
- data prefetching, where frequently used data is loaded into the cache in advance
- increasing cache block size, store larger chunks of data, so that next nearby access gets hit.
use multi-layer caches to reduce traffic to the origin.

conflict,

happens when multiple cache blocks compete for the same cache slot,
leads to frequent evictions and reloading.
solutions include :
- increasing associativity
- optimizing access patterns

capacity,

occurs when cache is too small to store all necessary data, forcing frequent replacements. *when the working set of data exceeds available cache size, older data gets removed before its needed again*.
reducing capacity misses involves: 
- increasing cache 
- efficient replacement policies
- ensuring frequently accessed data remains in cache longer

cause of cache misses,
- poor data locality, inefficient data access patterns
- limited cache size
- low associativity or improper blocksize

cdn's doesn't require entire file to be cached before serving, as soon as first byte arrives from origin it begins serving users. doesn't wait for the complete file to be cached from origin first & then serve.
this reduces delay caused by cache misses

during a traffic spike (when multiple requests for the same content reach the cache server before the origin has responded to the first request) CDN temporarily pauses the operation before forwarding additional request to origin, this pause helps reduce unnecessary load on origin, this helps to get more time to get the requested file to the caches and then provide them from there and not from origin.

cache expiry
static content like images,videos, stylesheets can be cached for extended periods to reduce compulsory misses.

cache-aside, application decides when to use cache

on modern processors, running time is dominated by memory access.

cache invalidation,

when one processor modifies a value in its cache, other processors cannot use the old value anymore. that memory location will be invalidated in all of the caches. Furthermore, since caches operate on the granularity of cache lines and not individual bytes, the entire cache line will be invalidated in all caches.

N-way set associative caches are the typical solution for processor caches, as they make good trade off between implementation simplicity and good hit rate.

SRAM is large, putting a lot of it on the die gobbles up die area very fast.

*direct*
- power efficient, avoids search through all cache lines
- simple placement policy, just evict the line
- simple hardware, only one tag needs to compared and selected
- lowe cache hit rate, more conflict misses
*fully*
- full utilization of cache, place in any line
- draws more power, comparison over entire cache
- high cost of comparison hardware

pseudo-associative