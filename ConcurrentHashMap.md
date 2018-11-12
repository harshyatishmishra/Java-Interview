* Fully parametrized constructor of ConcurrentHashMap takes 3 parameters, initialCapacity, loadFactor and concurrencyLevel.

1. initialCapacity
2. loadFactor
3. concurrencyLevel

* Concurrency Level : This denotes the number of shards. It is used to divide the ConcurrentHashMap internally into this number of partitions and equal number of threads are created to maintain thread safety maintained at shard level.

_The default value of “concurrencyLevel” is 16_. It means 16 shards whenever we create an instance of ConcurrentHashMap using default constructor, before even adding first key-value pair. It also means the creation of instances for various inner classes like ConcurrentHashMap$Segment, ConcurrentHashMap$HashEntry[] and ReentrantLock$NonfairSync.

In most cases in normal application, a single shard is able to handle multiple threads with reasonable count of key-value pairs. And performance will be also optimal. Having multiple shards just makes the things complex internally and introduces a lot of un-necessary objects for garbage collection, and all this for no performance improvement.

The extra objects created per concurrent hashmap using default constructor are normally in ratio of 1 to 50 i.e. for 100 such instance of ConcurrentHashMap, there will be 5000 extra objects created.

```
ConcurrentHashMap<String, Integer> instance = new ConcurrentHashMap<String, Integer>(16, 0.9f, 1);
```
An initial capacity of 16 ensures a reasonably good number of elements before resizing happens. Load factor of 0.9 ensures a dense packaging inside ConcurrentHashMap which will optimize memory use. And concurrencyLevel set to 1 will ensure that only one shard is created and maintained
