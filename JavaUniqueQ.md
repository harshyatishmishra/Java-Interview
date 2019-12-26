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
