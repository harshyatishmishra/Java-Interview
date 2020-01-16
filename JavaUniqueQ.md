* Can we Override the statuc method in Java?
  * No, We can not override the static method in Java because overriding is based upon the dynamic binding at runtime.
   But, you don't get any compiler error if you try to override a static method. 
   That means, if you try to override, Java doesn't stop you doing that; but you certainly don't get the same effect as you get for          non-static methods. 
   Overriding in Java simply means that the particular method would be called based on the run time type of the object and not on the        compile time type of it (which is the case with overriden static methods). Because they are class methods and hence access to them      is always resolved during compile time only using the compile time type information.
  
      ```   
      /* Java program to show that if static method is redefined by 
         a derived class, then it is not overriding. */

      // Superclass 
      class Base { 

          // Static method in base class which will be hidden in subclass  
          public static void display() { 
              System.out.println("Static or class method from Base"); 
          } 

           // Non-static method which will be overridden in derived class  
           public void print()  { 
               System.out.println("Non-static or Instance method from Base"); 
          } 
      } 

      // Subclass 
      class Derived extends Base { 

          // This method hides display() in Base  
          public static void display() { 
               System.out.println("Static or class method from Derived"); 
          } 

          // This method overrides print() in Base  
          public void print() { 
               System.out.println("Non-static or Instance method from Derived"); 
         } 
      } 

      // Driver class 
      public class Test { 
          public static void main(String args[ ])  { 
             Base obj1 = new Derived(); 

             // As per overriding rules this should call to class Derive's static  
             // overridden method. Since static method can not be overridden, it  
             // calls Base's display()  
             obj1.display();   

             // Here overriding works and Derive's print() is called  
             obj1.print();      
          } 
      } 
      ```

Notice the second line of the output. Had the staticMethod been overriden this line should have been identical to the third line as we're invoking the 'staticMethod()' on an object of Runtime Type as 'SubClass' and not as 'SuperClass'. This confirms that the static methods are always resolved using their compile time type information only.

* Can we Overload the static method in Java?
  * ‘Yes’. We can have two ore more static methods with same name, but differences in input parameters.
      ```
      public class Test { 
          public static void foo() { 
              System.out.println("Test.foo() called "); 
          } 
          public static void foo(int a) {  
              System.out.println("Test.foo(int) called "); 
          } 
          public static void main(String args[]) 
          {  
              Test.foo(); 
              Test.foo(10); 
          } 
      } 
      ```

* Can we overload methods that differ only by static keyword?
  * We cannot overload two methods in Java if they differ only by static keyword (number of parameters and types of parameters is same). 

    ```
    public class Test { 
        public static void foo() { 
            System.out.println("Test.foo() called "); 
        } 
        public void foo() { // Compiler Error: cannot redefine foo() 
            System.out.println("Test.foo(int) called "); 
        } 
        public static void main(String args[]) {  
            Test.foo(); 
        } 
    } 
    ```

* Who get the memory allocation first Static Variable, Static method, Static block?
  * In order they appear in file. When Class loaded in memory.
  
* Way to prevent the Method overriding
  * **final**
  * **private** -  private method is not accessible in subclass, which means they can not be overridden as well, because overriding         happens at child class. 
  * **static** - static methods can not be overridden in Java, because they are resolved and bonded during compile time, while    
    overriding takes place at runtime. They are resolved based upon the type and not object.
    
* Exception with method overriding 
  * If SuperClass does not declare an exception, then the SubClass can only declare unchecked exceptions, but not the checked 
    exceptions.
  * If SuperClass declares an exception, then the SubClass can only declare the child exceptions of the exception declared by the 
    SuperClass, but not any other exception.
  * If SuperClass declares an exception, then the SubClass can declare without exception.    
  
  
#### What is the difference between four String and StringBuffer, StringBuilder? Why is String immutable?
##### Variability

In simple terms: The String keyword uses the final keyword to modify the character array to hold the string private final char value[], so the String object is immutable. Both StringBuilder and StringBuffer inherit from the AbstractStringBuilder class. AbstractStringBuilder also uses character arrays to store strings char[]valuebut is not decorated with the final keyword, so both objects are mutable.

The constructors of StringBuilder and StringBuffer are implemented by calling the parent class constructor, that is, AbstractStringBuilder. You can check the source code yourself.

`AbstractStringBuilder.java

abstract  class  AbstractStringBuilder  implements  Appendable , CharSequence {
     char [] value;
     int count;
     AbstractStringBuilder () {
    }
    AbstractStringBuilder ( int  capacity ) {
        value =  new  char [capacity];
    }
    `
##### Thread safety

Objects in String are immutable and can be understood as constant and thread-safe. AbstractStringBuilder is the common parent class of StringBuilder and StringBuffer, which defines some basic operations on strings, such as expandCapacity, append, insert, indexOf and other public methods. StringBuffer adds a synchronization lock to a method or adds a synchronization lock to a called method, so it is thread-safe. StringBuilder does not synchronize methods, so it is not thread-safe.

##### performance

Every time you change the String type, a new String object is generated, and the pointer is pointed to the new String object. StringBuffer operates on the StringBuffer object itself each time, rather than generating a new object and changing the object reference. In the same situation, using StringBuilder can only achieve a performance improvement of about 10% to 15% compared to using StringBuffer, but at the risk of multithreading insecurity.

##### A summary of the three uses:

* Manipulating small amounts of data: applicable to String
* Operate a large amount of data in a single-threaded string buffer operation: applicable to StringBuilder
* Multithreaded operation of a large amount of data in the string buffer: applicable to StringBuffer

##### Why is String immutable?
In simple terms, the String class uses a final decorated char type array to store characters. The source code is as follows:
`
    / * * The value is used for character storage. * /
     Private  final  char value [];
     `
#### Is String really immutable?
I think that if someone asks this question, the answer is immutable. Here are just two representative examples for you:

1) String is immutable but does not mean that the reference cannot be changed

		String str =  " Hello " ;
		str = str +  " World " ;
		 System . out . println ( " str = "  + str);
result:

str=Hello World
Parsing:

In fact, the contents of the original String are unchanged, but str has changed from the memory address that originally pointed to "Hello" to the memory address that points to "Hello World". string.

2) It is possible to modify so-called "immutable" objects through reflection

		// Create the string "Hello World" and assign it to the reference s 
		String s =  " Hello World " ;

		System . Out . Println ( " s = "  + s); // Hello World

		// Get the value field in the String class 
		Field valueFieldOfString =  String . Class . GetDeclaredField( " value " );

		// Change the access permissions of the value property 
		valueFieldOfString . SetAccessible( true );

		// Get the value of the value attribute on the s object 
		char [] value = ( char []) valueFieldOfString . Get (s);

		// Change the fifth character in the array referenced by 
		value value [ 5 ] =  ' _ ' ;

		System . Out . Println ( " s = "  + s); // Hello_World
result:

s = Hello World
s = Hello_World
Parsing:

You can use reflection to access private members, and then reflect the value property of the String object, and then change the structure of the array through the obtained value reference. But generally we don't do this, here is just a brief mention of this thing.
