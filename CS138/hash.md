Hashing
=============
* All operations are basically O(1).
* A hash table is basically a vector/array with buckets.
* A hash function takes a key and returns a bucket index (eg. sum ASCII values of a string, mod by # of buckets).

We've got to watch out for collisions - multiple keys with the same bucket index. 

## Closed Hashing
Three states: Empty, Zombie, Active.

* For insert, if the bucket index given is full, go to the next bucket until an empty or zombie bucket is found. 
* For lookups, go to the bucket index given, go to the next until you find the stuff you're looking for, or an empty slot (it's not there).
* For deletes, mark the bucket as a zombie (inserts can overwrite it). 

**Issues with this**
* As it gets more full, operations approach O(N) time. 
    * < 80% full is still "reasonably efficient"
    
## Open Hashing with Chaining
* Uses a list of lists. 
* Each bucket holds a pointer to a list rather than an element.
* Simply append/remove items to the linked list as you would typically. 

Usually: ` vector<Node*> HashTable;`

## A bit more on hashing
* We **do not** need unique keys.
* Not a good idea for sorted info.
* Optimization techniques:
    * A better hash function
    * More buckets - although this wastes space.


**Properties of a hash function**
* The same input should give the same answer (so we can lookup stuff later)
* Uniform spread is always good.
* Efficient calculations
* Scalable (if we use more buckets)