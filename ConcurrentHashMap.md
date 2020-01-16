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


##### JDK1.7 
First divide the data into segments, and then assign a lock to each segment of data. When a thread occupies a lock to access one segment of data, the data in other segments can also be accessed by other threads.

##### ConcurrentHashMap is composed of Segment array structure and HashEntry array structure .

Segment implements ReentrantLock, so Segment is a reentrant lock that plays the role of a lock. HashEntry is used to store key-value data.
`
static  class  Segment <K, V> extends  ReentrantLock  implements  Serializable {
}`
A ConcurrentHashMap contains an array of segments. The structure of Segment is similar to HashMap. It is an array and linked list structure. A Segment contains an HashEntry array. Each HashEntry is an element of a linked list structure. Each Segment guards the elements in a HashEntry array. When making changes, you must first acquire the lock for the corresponding segment.

##### JDK1.8 
ConcurrentHashMap cancels segmentation locks and uses CAS and synchronized to ensure concurrency security. The data structure is similar to the structure of HashMap 1.8, array + linked list / red and black binary tree.

synchronized only locks the first node of the current linked list or red-black binary tree, so as long as the hash does not conflict, no concurrency will occur, and the efficiency will be increased by N times.
