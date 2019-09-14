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
