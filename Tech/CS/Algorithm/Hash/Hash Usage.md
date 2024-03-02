---
tags: Algorithm, Generic
---
Having talked about TreeSets and TreeMaps in the previous chapter, let's discuss how the map and the set interface can be implemented using hashing. In this chapter, we will be focusing on the usage of a HashSet and a HashMap, with the next chapter covering their implementation under the hood.

Hash maps are probably one of the most useful data structures/concepts for coding interviews, for reasons we will discuss soon. They are also extremely ubiquitous in interviews. When questions ask "unique, count, frequency", take hash maps out your arsenal.

Recall that the difference between a set and a map is that in sets do not map their keys to anything, whereas maps have key-value pairs.


## Motivation

Let's start off by comparing hash maps to tree maps and sorted arrays and understand what trade-offs we make using each data structure.

|Operation|TreeMap|HashMap|Array|
|---|---|---|---|
|Insert| O(log n) | O(1) | (n)|
|Remove|O(log n)|O(1) | O(n)|
|Search | O(log n)| O(1)| O(log n), if sorted|
|Inorder Traversal| O(n)|-|-|

> The time complexity listed in the table for hash maps is the average case time complexity. However, it is widely accepted in interviews to assume constant time complexity.

### Tree Maps vs Hash Maps

The downside of hash maps is that they are not ordered, so there is no guarantee that the map will store the values in set positions like BSTs or arrays do. If you were to iterate through all the keys, you would first need to sort them and then traverse, which will run in O(n log n) time.

Because hashmaps don't allow duplicates and have key-value pairs, we can use them to count frequency of keys. Going back to our phonebook example, we can count the number of times a given name appears in our phonebook by mapping the name to its frequency.

Given the array below, we can add all the elements into a hash map as keys. Because hash maps do not allow duplicates, we can use this to our advantage. If a name already exists in our hash map as the key and we encounter it again in our array, we can just increment its value by 1. If the name does not exist, we can add it to our hash map and set the frequency to 1.

`["alice", "brad", "collin", "brad", "dylan", "kim"]`

|Key|Value|
|---|---|
|Alice|1|
|Brad|2|
|Collin|1|
|Dylan|1|
|Kim|1|

The following code demonstrates how the above operation will execute.

```javascript
const names = ["alice", "brad", "collin", "brad", "dylan", "kim"];
const countMap = new Map()

for (let i = 0; i < names.length; i++) {
    // If countMap does not contain name
    if (!countMap.has(names[i])) {
        countMap.set(names[i], 1);
    } else {
        countMap.set(names[i], countMap.get(names[i]) + 1);
    }
}
```

The above algorithm, when implemented using a hash map, is more efficient than using a tree map. With a tree map, the insertion operation would cost O(log n) time and if  is the size of the array, it would total to O(n log n) time. This only costs O(n) in the case of a hash map.