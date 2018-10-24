## equals() and == Operator


The == operator works fine with the primitive data type but when it comes to object it will create confusion with equal().
== operator compare two objects based on the memory reference. So, == operator will return true only if two object reference 
it is comparing represent exactly the same object.

Equals() method is defined in Object class in Java and used for checking equality of two objects defined by business logic 
e.g. two Employees are considered equal if they have same empId etc. You can have your domain object and then override equals 
method for defining a condition on which two domain objects will be considered equal. equal has contracted with hashcode method 
in Java and whenever you override equals method you also need to override hashcode() in Java. 

The default implementation of equals provided in Object class is similar to "==" equality operator and return true if you are 
comparing two references to the same object. It’s one of the Java best practice to override equals in Java to define equality 
based on business requirement. It’s also worth noting that equals should be consistent with compareTo in Java So that when you 
store objects in TreeMap or TreeSet Collection, which uses compareTo for checking equality, behavior remains consistent.
