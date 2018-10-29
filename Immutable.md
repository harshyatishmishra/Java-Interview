
1. Create a *_final_* class. So, No one can extends or override it.
2. Create the _fields_ as a _private_ and _final_.
3. Don't expose any setter method for the field.
4. Construct a _private_ constructor and create a factory method to return new object every time.
OR
4. public constructor will also help.
5. When expose a method always return a new object.
6. If the class holds a mutable object:
    1. Inside the constructor, make sure to use a clone copy of the passed argument and never set your mutable field to the real instance passed through constructor, this is to prevent the clients who pass the object from modifying it afterwards.
    2. Make sure to always return a clone copy of the field and never return the real object instance.
    
    
    ```

	class MutableClass {
		private Integer age;

		public Integer getAge() {
			return age;
		}

		public void setAge(Integer age) {
			this.age = age;
		}

		public MutableClass(Integer age) {
			super();
			this.age = age;
		}
	}

	final class Immutable {
		private final String field;
		private final MutableClass mClass;

		public static Immutable createNewInstance(String field, MutableClass mClass) {
			return new Immutable(field, mClass);
		}

		private Immutable(String field, MutableClass mClass) {
			super();
			this.field = field;
			MutableClass clone = new MutableClass(mClass.getAge());
			this.mClass = clone;
		}

		public MutableClass getmClass() {
			return mClass;
		}

		public String getField() {
			return field;
		}

		@Override
		public String toString() {
			return "Immutable [field=" + field + ", mClass=" + mClass + "]";
		}
	}

	public class Test {
		public static void main(String[] args) {
			Immutable createNewInstance = Immutable.createNewInstance("Harsh", new MutableClass(12));
			System.out.println(createNewInstance);
		}
	}
    ```
