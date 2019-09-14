### Sorting Map directly with Comparators
Map interface added default methods which gives you comparators for different styles like comparingByKey, comparingByValue.

```
Map<String, String> map = new HashMap<>();
    map.put("C", "c");
    map.put("B", "b");
    map.put("Z", "z");
    List<Map.Entry<String, String>> sortedByKey = map.entrySet().stream().sorted(Map.Entry.comparingByKey())
        .collect(Collectors.toList());
    sortedByKey.forEach(System.out::println);
```

### Iterate over map easily with forEach
```
    Map<String, String> map = new HashMap<>();
    map.put("C", "c");
    map.put("B", "b");
    map.put("Z", "z");
    map.forEach((k, v) -> System.out.println("Key : " + k + " Value : " + v));
```

### if-else condition, use getOrDefault method
This method returns the value to which the specified key is mapped, otherwise returns the given defaultValue 
if this map contains no mapping for the key.
```
    Map<String, String> map = new HashMap<>();
    map.put("C", "c");
    String val = map.getOrDefault("B", "value");
    System.out.println(val); // prints value
```

### Replace and Remove utilities.
replaceAll Can replace all the values in a single go
```
    Map<String, String> map = new HashMap<>();
    map.put("C", "c");
    map.put("B", "b");
    map.replaceAll((k, v) -> "x"); // values is "x" for all keys.  
```
replace(K key, V oldValue, V newValue) method replaces the entry for the specified key only if 
currently mapped to the specified value.


### putIfAbsent
IF the key is absent then only it will add it. example.
```
    Map<String, String> map = new HashMap<>();
    map.put("C", "c");
    map.put("B", "b");
    map.putIfAbsent("B", "x");
    System.out.println(map.get("B")); // print b
```

### operate directly on values.
when we needed to get the value for specific keys, process it and put them back. 
Now you can directly modify with help of compute method.
```
    Map<String, String> map = new HashMap<>();
    map.put("C", "c");
    map.put("B", "b");
    map.compute("B", (k, v) -> v.concat(" - new "));
    System.out.println(map.get("B")); // prints "b - new"
```

Conditional computes are also available. Look at computeIfPresent, computeIfAbsent methods.


### merge maps use merge method.
This is little tricky and more useful when you are combining maps or appending values for duplicated keys.

If the specified key is not already associated with a value or is associated with null, associates it with the given non-null 
value. Otherwise, replaces the associated value with the results of the given remapping function, or removes if the result 
is null.

```
Map<String, String> map = new HashMap<>();
    map.put("C", "c");
    map.put("B", "b");
    map.merge("B", "NEW", (v1, v2) -> v1 + v2);
    System.out.println(map.get("B")); // prints bNEW
```    
    
