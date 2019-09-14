### HashMap performance Improvement Changes in Java 8
Here I am going to discuss JDK 8’s new strategy for dealing with Hash collisions. Earlier before Java 8, the performance of the HashMap was poor due to the hash collision, it degrades the performance of HashMap significantly. This change can be notifiable only when if you are using the HashMap for a large number of elements, why (see below points)?

The traversal of HashMap, get(), and other methods lookup time of HashMap have a negative impact due to hash collisions. This situation you can face when multiple keys end up in the same bucket, then values along with their keys are placed in a linked list. So, the retrieval time of elements from HashMap increases from O(1) to O(n). Because the linked list has to be traversed to get the entry in the worst case scenario.

But Java 8 has come with the following new strategy for HashMap objects in case of high collisions.

* To address this issue, Java 8 hash elements use balanced trees instead of linked lists after a certain threshold is reached. Which means HashMap starts with storing Entry objects in a linked list but after the number of items in a hash becomes larger than a certain threshold. The hash will change from using a linked list to a balanced tree.
* Above changes ensure the performance of O(log(n)) in worst case scenarios and O(1) with proper hashCode().
* The alternative String hash function added in Java 7 has been removed.

### Contract
You must override hashCode in every class that overrides equals. Failure to do so will result in a violation of the general
contract for Object.hashCode, which will prevent your class from functioning properly in conjunction with all hash-based 
collections, including HashMap, HashSet, and Hashtable.

#### The contract between equals() and hashCode() is:

* If two objects are equal, then they must have the same hash code.
* If two objects have the same hash code, they may or may not be equal.

```
MyClass first = new MyClass("a","first");
MyClass second = new MyClass("a","second");
```

#### Override only equals
If only equals is overriden, then when you call myMap.put(first,someValue) first will hash to some bucket and when you call
myMap.put(second,someOtherValue) it will hash to some other bucket (as they have a different hashCode). So, although they 
are equal, as they don't hash to the same bucket, the map can't realize it and both of them stay in the map.

Although it is not necessary to override equals() if we override hashCode(), let's see what would happen in this particular 
case where we know that two objects of MyClass are equal if their importantField is equal but we do not override equals().

#### Override only hashCode
If you only override hashCode then when you call myMap.put(first,someValue) it takes first, calculates its hashCode and 
stores it in a given bucket. Then when you call myMap.put(second,someOtherValue) it should replace first with second as 
per the Map Documentation because they are equal (according to the business requirement).

But the problem is that equals was not redefined, so when the map hashes second and iterates through the bucket looking 
if there is an object k such that second.equals(k) is true it won't find any as second.equals(first) will be false.

#### Hashing retrieval is a two-step process.
Find the right bucket (using hashCode())
Search the bucket for the right element (using equals() )
