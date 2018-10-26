###### Serialization is a mechanism of converting the state of an object into a byte stream. Deserialization is the reverse process.

## Advantages of Serialization
1. To save/persist state of an object.
2. To travel an object across a network.

## Points to remember
1. If a parent class has implemented Serializable interface then child class doesn’t need to implement it but vice-versa is not true.
2. Only non-static data members are saved via Serialization process.
3. Static data members and transient data members are not saved via Serialization process.So, if you don’t want to save value of a non-static data member then make it transient.
4. Constructor of object is never called when an object is deserialized.
5. Associated objects must be implementing Serializable interface.

## SerialVersionUID
The Serialization runtime associates a version number with each Serializable class called a SerialVersionUID, which is used during Deserialization to verify that sender and reciever of a serialized object have loaded classes for that object which are compatible with respect to serialization. If the reciever has loaded a class for the object that has different UID than that of corresponding sender’s class, the Deserialization will result in an InvalidClassException. A Serializable class can declare its own UID explicitly by declaring a field name.
It must be static, final and of type long.
i.e-
```ANY-ACCESS-MODIFIER static final long serialVersionUID=42L;```

If a serializable class doesn’t explicitly declare a serialVersionUID, then the serialization runtime will calculate a default one for that class based on various aspects of class, as described in Java Object Serialization Specification. However it is strongly recommended that all serializable classes explicitly declare serialVersionUID value, since its computation is highly sensitive to class details that may vary depending on compiler implementations, any change in class or using different id may affect the serialized data.

It is also recommended to use private modifier for UID since it is not useful as inherited member.

## Type Changes Affecting Serialization

###### Incompatible Changes
1. Deleting fields - If a field is deleted in a class, the stream written will not contain its value. When the stream is read by an earlier class, the value of the field will be set to the default value because no value is available in the stream. However, this default value may adversely impair the ability of the earlier version to fulfill its contract.
2. Moving classes up or down the hierarchy - This cannot be allowed since the data in the stream appears in the wrong sequence.
3. Changing a nonstatic field to static or a nontransient field to transient - When relying on default serialization, this change is equivalent to deleting a field from the class. This version of the class will not write that data to the stream, so it will not be available to be read by earlier versions of the class. As when deleting a field, the field of the earlier version will be initialized to the default value, which can cause the class to fail in unexpected ways.
4. Changing the declared type of a primitive field - Each version of the class writes the data with its declared type. Earlier versions of the class attempting to read the field will fail because the type of the data in the stream does not match the type of the field.
5. Changing the writeObject or readObject method so that it no longer writes or reads the default field data or changing it so that it attempts to write it or read it when the previous version did not. The default field data must consistently either appear or not appear in the stream.
6. Changing a class from Serializable to Externalizable or vice versa is an incompatible change since the stream will contain data that is incompatible with the implementation of the available class.
7. Changing a class from a non-enum type to an enum type or vice versa since the stream will contain data that is incompatible with the implementation of the available class.
8. Removing either Serializable or Externalizable is an incompatible change since when written it will no longer supply the fields needed by older versions of the class.
Adding the writeReplace or readResolve method to a class is incompatible if the behavior would produce an object that is incompatible with any older version of the class.

###### Compatible Changes
1. Adding fields - When the class being reconstituted has a field that does not occur in the stream, that field in the object will be initialized to the default value for its type. If class-specific initialization is needed, the class may provide a readObject method that can initialize the field to nondefault values.
2. Adding classes - The stream will contain the type hierarchy of each object in the stream. Comparing this hierarchy in the stream with the current class can detect additional classes. Since there is no information in the stream from which to initialize the object, the class's fields will be initialized to the default values.
3. Removing classes - Comparing the class hierarchy in the stream with that of the current class can detect that a class has been deleted. In this case, the fields and objects corresponding to that class are read from the stream. Primitive fields are discarded, but the objects referenced by the deleted class are created, since they may be referred to later in the stream. They will be garbage-collected when the stream is garbage-collected or reset.
4. Adding writeObject/readObject methods - If the version reading the stream has these methods then readObject is expected, as usual, to read the required data written to the stream by the default serialization. It should call defaultReadObject first before reading any optional data. The writeObject method is expected as usual to call defaultWriteObject to write the required data and then may write optional data.
5. Removing writeObject/readObject methods - If the class reading the stream does not have these methods, the required data will be read by default serialization, and the optional data will be discarded.
6. Adding java.io.Serializable - This is equivalent to adding types. There will be no values in the stream for this class so its fields will be initialized to default values. The support for subclassing nonserializable classes requires that the class's supertype have a no-arg constructor and the class itself will be initialized to default values. If the no-arg constructor is not available, the InvalidClassException is thrown.
7. Changing the access to a field - The access modifiers public, package, protected, and private have no effect on the ability of serialization to assign values to the fields.
8. Changing a field from static to nonstatic or transient to nontransient - When relying on default serialization to compute the serializable fields, this change is equivalent to adding a field to the class. The new field will be written to the stream but earlier classes will ignore the value since serialization will not assign values to static or transient fields.


## Object Serialization with Inheritance

1.  If superclass is serializable then subclass is automatically serializable.
      ```
      class A implements Serializable 
      { 
         int i; 
      
       // parameterized constructor 
       public A(int i)  
       { 
           this.i = i; 
       } 
      
      } 
      class B extends A 
      { 
          int j; 

          // parameterized constructor 
          public B(int i, int j)  
          { 
              super(i); 
              this.j = j; 
          } 
      }
      public class Test 
      { 
          public static void main(String[] args)  
                  throws Exception  
          { 
              B b1 = new B(10,20); 

              System.out.println("i = " + b1.i); 
              System.out.println("j = " + b1.j); 

              /* Serializing B's(subclass) object */

              //Saving of object in a file 
              FileOutputStream fos = new FileOutputStream("abc.ser"); 
              ObjectOutputStream oos = new ObjectOutputStream(fos); 

              // Method for serialization of B's class object 
              oos.writeObject(b1); 

              // closing streams 
              oos.close(); 
              fos.close(); 

              System.out.println("Object has been serialized"); 

              /* De-Serializing B's(subclass) object */

              // Reading the object from a file 
              FileInputStream fis = new FileInputStream("abc.ser"); 
              ObjectInputStream ois = new ObjectInputStream(fis); 

              // Method for de-serialization of B's class object 
              B b2 = (B)ois.readObject(); 

              // closing streams 
              ois.close(); 
              fis.close(); 

              System.out.println("Object has been deserialized"); 

              System.out.println("i = " + b2.i); 
              System.out.println("j = " + b2.j); 
          } 
      } 
      ```
      ```
      Output:

         i = 10
         j = 20
         Object has been serialized
         Object has been deserialized
         i = 10
         j = 20
      ```
2.  If a superclass is not serializable then subclass can still be serialized.
    * Serialization: At the time of serialization, if any instance variable is inheriting from non-serializable superclass, then JVM ignores original value of that instance variable and save default value to the file.
    
    * De- Serialization: At the time of de-serialization, if any non-serializable superclass is present, then JVM will execute instance control flow in the superclass. To execute instance control flow in a class, JVM will always invoke default(no-arg) constructor of that class. So every non-serializable superclass must necessarily contain default constructor, otherwise we will get runtime-exception.
    ```
      class A  
      { 
          int i; 

          // parameterized constructor 
          public A(int i)  
          { 
              this.i = i; 
          } 

          // default constructor 
          // this constructor must be present 
          // otherwise we will get runtime exception 
          public A() 
          { 
              i = 50; 
              System.out.println("A's class constructor called"); 
          } 

      } 

      // subclass B  
      // implementing Serializable interface 
      class B extends A implements Serializable 
      { 
          int j; 

          // parameterized constructor 
          public B(int i,int j)  
          { 
              super(i); 
              this.j = j; 
          } 
      } 

      public class Test 
      { 
          public static void main(String[] args)  
                  throws Exception  
          { 
              B b1 = new B(10,20); 

              System.out.println("i = " + b1.i); 
              System.out.println("j = " + b1.j); 

              // Serializing B's(subclass) object  

              //Saving of object in a file 
              FileOutputStream fos = new FileOutputStream("abc.ser"); 
              ObjectOutputStream oos = new ObjectOutputStream(fos); 

              // Method for serialization of B's class object 
              oos.writeObject(b1); 

              // closing streams 
              oos.close(); 
              fos.close(); 

              System.out.println("Object has been serialized"); 

              // De-Serializing B's(subclass) object  

              // Reading the object from a file 
              FileInputStream fis = new FileInputStream("abc.ser"); 
              ObjectInputStream ois = new ObjectInputStream(fis); 

              // Method for de-serialization of B's class object 
              B b2 = (B)ois.readObject(); 

              // closing streams 
              ois.close(); 
              fis.close(); 

              System.out.println("Object has been deserialized"); 

              System.out.println("i = " + b2.i); 
              System.out.println("j = " + b2.j); 
          } 
      }
    ```
    ```Output
      i = 10
      j = 20
      Object has been serialized
      A's class constructor called
      Object has been deserialized
      i = 0
      j = 20
    ```
3.  If the superclass is serializable but we don’t want the subclass to be serialized.
    * There is no direct way to prevent subclass from serialization in java. One possible way by which a programmer can achieve this is by implementing the writeObject() and readObject() methods in the subclass and needs to throw NotSerializableException from these methods. These methods are executed during serialization and de-serialization respectively. By overriding these methods, we are just implementing our own custom serialization.
   ```
   class A implements Serializable 
   { 
       int i; 

       // parameterized constructor 
       public A(int i)  
       { 
           this.i = i; 
       } 

   } 

   // subclass B  
   // B class doesn't implement Serializable 
   // interface. 
   class B extends A 
   { 
       int j; 

       // parameterized constructor 
       public B(int i,int j)  
       { 
           super(i); 
           this.j = j; 
       } 

       // By implementing writeObject method,  
       // we can prevent 
       // subclass from serialization 
       private void writeObject(ObjectOutputStream out) throws IOException 
       { 
           throw new NotSerializableException(); 
       } 

       // By implementing readObject method,  
       // we can prevent 
       // subclass from de-serialization 
       private void readObject(ObjectInputStream in) throws IOException 
       { 
           throw new NotSerializableException(); 
       } 

   } 

   // Driver class 
   public class Test 
   { 
       public static void main(String[] args)  
               throws Exception  
       { 
           B b1 = new B(10, 20); 

           System.out.println("i = " + b1.i); 
           System.out.println("j = " + b1.j); 

           // Serializing B's(subclass) object  

           //Saving of object in a file 
           FileOutputStream fos = new FileOutputStream("abc.ser"); 
           ObjectOutputStream oos = new ObjectOutputStream(fos); 

           // Method for serialization of B's class object 
           oos.writeObject(b1); 

           // closing streams 
           oos.close(); 
           fos.close(); 

           System.out.println("Object has been serialized"); 

           // De-Serializing B's(subclass) object  

           // Reading the object from a file 
           FileInputStream fis = new FileInputStream("abc.ser"); 
           ObjectInputStream ois = new ObjectInputStream(fis); 

           // Method for de-serialization of B's class object 
           B b2 = (B)ois.readObject(); 

           // closing streams 
           ois.close(); 
           fis.close(); 

           System.out.println("Object has been deserialized"); 

           System.out.println("i = " + b2.i); 
           System.out.println("j = " + b2.j); 
       } 
   } 
   ```
   ```output
         i = 10
         j = 20
         Exception in thread "main" java.io.NotSerializableException
            at B.writeObject(Test.java:44)
   
   ```


