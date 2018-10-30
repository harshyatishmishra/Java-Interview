Wrapper classes that encapsulate a primitive type within an object.

A wrapper class wraps (encloses) around a data type and gives it an object appearance. Wherever, the data type is required as an object, this object can be used. Wrapper classes include methods to unwrap the object and give back the data type.

double Double 
float	Float
long	Long
int	Integer
short	Short
byte	Byte
char	Character
boolean	Boolean

###### Java wrapper classes are used in scenarios –

1. When two methods wants to refer to the same instance of an primitive type, then pass wrapper class as method argument.
2. Generics in Java works only with object and does not support primitive types.
3. Collections in Java deal only with objects; to store a primitive type in one of these classes, you need to wrap the primitive type in a class.
4. When you want to refer null from data type, the you need object. Primitives cannot have null as value.


###### Conversion Primitive type to wrapper class
1. using constructor
```
Integer object = new Integer(10);
``` 
2. using static factory method
```
Integer anotherObject = Integer.valueOf(10);
```
###### Wrapper class to primitive type
```
Integer object = new Integer(10);
 
int val = object.intValue();
```
###### Autoboxing
conversion of the primitive types into their corresponding object wrapper classes.

###### Unboxing
 conversion happens from wrapper class to its corresponding primitive type.
