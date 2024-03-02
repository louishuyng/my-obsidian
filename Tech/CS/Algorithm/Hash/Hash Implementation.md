---
tags: Algorithm, Generic
---
Hash maps are most commonly implemented using arrays under the hood. Our hash map can be of size zero but initially our array will not be of size zero.

Suppose that we want to fill up our array with the follow **key-value** pairs.

```javascript
hashmap["Alice"] = "NYC";
hashmap["Brad"] = "Chicago";
hashmap["Collin"] = "Seattle";
```

To do so, we need to introduce something called **hashing** and the **hash function**. A hash function takes the key (a string in this case) and converts it into an integer. The same string will always always result in the same integer. Using this integer, we can determine the location at which we wish to store our **key-value pair**.

---

## Insertion and Hashing

Take `"Alice"` for example. Our hash function will take each character in the string and get its ASCII code. Then it will add up the ASCII codes to determine where it should end up in the array. However, since this can be a massive number and can easily go out of bounds, we can use the modulo operator. Let's imagine that summing up all the ASCII codes for all the characters in `"Alice"` is 2525. To determine its location in the array, we can use the formula: `sum of ASCII codes mod size of the array`. In this case, the size is 22. So, `25 mod 2` results in 11, which is where we store the first key value pair. The process is also sometimes known as **pre-hashing**.

Because the size of our array is only 22, it is entirely possible that another key-value pair results in the same array position when we take the mod. This is called a **collision** and collisions are very common. There are ways to try and minimize collisions but they are quite inevitable. We will discuss collisions in more detail shortly.

To ensure each key-value pair finds a vacant spot, we will keep track of the size of the array, and the number of indices that are actually full. The way to do this is to: `capacity * 2`, where `capacity` is the size of the array, or when the hash map becomes half full, meaning half of the indices in the array are occupied. In this case, we have a size of 22 and because adding `"Alice" : "NYC"` will result in the map being half full, we will double its size _before_ performing the next insertion. The sizing works the same as we described in the dynamic arrays chapter.

> We don't resize the array at the time of insertion of a new key-value pair, but rather as soon as the array becomes half full. This ensures that we minimize collisons before insertion.

Once we perform the resize, we perform an operation called **re-hashing**. This is needed because the size of the array has changed. Rehashing tells us to re-compute the position of all the elements that already exist in the hash map. We inserted `"Alice"` at index 11. After doubling the size of the array, the new position of `"Alice"`, in this case, just happens to be at index 11 again so we don't need to update it. However, if `"Alice"`'s new position was now at index 22, we would need to move it.

> We need to perform re-hashing if we want to maintain the �(1)O(1) time complexity for our operations. We initially started with an array of size 22. Let's say that we wanted to insert 10,00010,000 key-value pairs into the array. When we wish to search `"Alice"`, we would calculate the position using the new size, and `"Alice"` might not be in that position anymore!

Suppose that converting `"Brad"` to integer results in 2727. `27 mod 4 = 3`, meaning that `"Brad" : "Chicago"` would end up in the 33rd index. Now, we will double the size to be size 88.

We have domonstrated the insertions in the visual below.

---

## Collisions

Suppose converting `"Collin"` to an integer results in 3333. `33 mod 8 = 1`. This is a collision since Alice already resides in the first index. How do we go about resolving this? Okay, we could just overwrite but that would mean that we end up losing `"Alice"`. We could also keep increasing the size of the array until we find a vacant space for `"Collin"`. This, however is a gigantic memory hog.

There are two common ways in which we can deal with collisions:

**1. Chaining**

Chaining (recall the concept from the Linked List chapter) is achieved by chaining Linked List nodes together, so that multiple key-value pairs can be stored at the same index.

Because Alice and Collin belong to the same index, we can store them as a linked list object. In this case, the time complexity comes down to �(�)O(n), for searching, inserting and deleting. This way, any future keys that belong to the same index will be stored as a node in the linked list chain.

The visual below demonstrates this.

**2. Open Addressing** The idea behind open addressing is to find the next available slot so that we don't store more than one key-value pair in one index. This technique, compared to chaining is more efficient if there are a small number of collisions. The limitation here however is that the total number of entries in the table is limited by the size of the array.

![hashing](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/32cd21d1-9655-408f-858b-5b35c2ddf800/sharpen=1)

> To reduce the amount of collisions that occur, it mathematically makes sense to choose the hashmap to be of a prime size. This is because the prime number is only divisible by 11 and itself! This post from [CS StackExchange](https://cs.stackexchange.com/questions/11029/why-is-it-best-to-use-a-prime-number-as-a-mod-in-a-hashing-function) provides a mathematical proof if you are interested.

---

## Code Implementation

Now that we understand the concept behind hashing, its implementation, searching, deletion, insertion and how a hash function works, let's take a look at how we would go about implementing it in code.

> We will store a list of `Pair`s in our array, for which we declare a class.

```javascript
class Pair {
    constructor(key, val) {
        this.key = key;
        this.val = val;
    }
}
```

> We initialize a size, capacity and the map itself in our constructor. Here, `size` refers to the size of the hash map and `capacity` refers to size of the array under the hood.

```javascript
class HashMap:
    // Global variables
    size, capacity, map
    constructor(size, capacity, map):
        size = 0
        capacity = 2
        map = [None, None]
```


> The hash function below iterates through each of the characters in a given key, sums up their ASCII code and finds the position of the key in the array.

```javascript
hash(key) {
    let index = 0;
    for (let i = 0; i < key.length; i++) {
        index+= key.charCodeAt(i);
    }
    return index % this.capacity;
}
```

> To retrieve the value, we first need to retrieve the position and check if the value exists in that position. If it does, we can return that value. Otherwise, we can perform open addressing and look for it in the next available index.

```javascript
get(key) {
    let index = this.hash(key);
    while (this.map[index] != null) {
        if (this.map[index].key == key) {
            return this.map[index].val;
        }  
        index += 1;
        index = index % this.capacity;
    }    
    return null;
}
```

> To add to the map, we first compute the hash of the key and find the position. Once this is calculated, there are three scenarios.
> 
> 1. The index is occupied
> 2. The index is occupied with the same key
> 3. The index is vacant

```javascript
put(key, val) {
    let index = this.hash(key);

    while (true) {
        if (this.map[index] == null) {
            this.map[index] = new Pair(key, val);
            this.size += 1;
            if (this.size >= this.capacity / 2) {
                this.rehash();
            }
            return;       
        } else if (this.map[index].key == key) {
            this.map[index].val = val;
            return;
        }
        index += 1;
        index = index % this.capacity;
    }    
}
```

> To remove, we find the index, remove the key, and set the index to null.

```javascript
remove(key) {
    if (this.get(key) == null) {
        return;
    }
    
    let index = this.hash(key);
    while (true) {
        if (this.map[index].key == key) {
            // Removing an element using open-addressing actually causes a bug,
            // because we may create a hole in the list, and our get() may 
            // stop searching early when it reaches this hole.
            this.map[index] = null;
            this.size -= 1;
            return;
        }    
        index += 1;
        index = index % this.capacity;
    }
}
```

> When we perform re-hashing, we double the capacity, copy our previous map's values into our new map and set the size to be zero.

```javascript
rehash() {
    this.capacity = 2 * this.capacity;
    let newMap = new Array(this.capacity).fill(null);
    let oldMap = this.map;
    this.map = newMap;
    this.size = 0;
    for (let i = 0; i < oldMap.length; i++) {
        if (oldMap[i]) {
            this.put(oldMap[i].key, oldMap[i].val)
        }
    }
}
```

> Finally, if we wish to print all of the pairs, it would look like the following.

```javascript
print() {
    for (let i = 0; i < this.map.length; i++) {
        if (this.map[i]) {
            console.log(this.map[i].key + " " + this.map[i].val)
        }
    }
}
```