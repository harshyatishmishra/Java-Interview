 Immutable objects are instances whose state doesn’t change after it has been initialized.
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
```
```
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
```
```
  public class ImmutableClass {
    public static void main(String[] args) {
      Immutable createNewInstance = Immutable.createNewInstance("Harsh", new MutableClass(12));
      System.out.println(createNewInstance);
    }
  }
```
