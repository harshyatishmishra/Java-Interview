#### What is the difference between Streams and Collections in Java 8?
* Collection is used for storing data in different data structures while Stream API is used for computation of data on a large set of Objects.

* Collection API we can store a finite number of elements in a data structure. With Stream API, we can handle streams of data that can contain infinite number of elements.

* Eager vs. Lazy: 
   Collection API constructs objects in an eager manner. Stream API creates objects in a lazy manner.

* Multiple consumption: 
  Most of the Collection APIs support iteration and consumption of elements multiple times. With Stream API we can consume or iterate elements only once.
