* For frequently used objects, you can omit the time it takes to create them, which is a considerable system overhead for those heavyweight objects
* Because the number of new operations is reduced, the frequency of system memory usage is also reduced, which will reduce GC pressure and shorten GC pause times.

* Singleton pattern restricts the instantiation of a class and ensures that only one instance of the class exists in the java virtual machine.
* The singleton class must provide a global access point to get the instance of the class.
* Singleton pattern is used for logging, drivers objects, caching and thread pool.
* Singleton design pattern is also used in other design patterns like Abstract Factory, Builder, Prototype, Facade etc.
* Singleton design pattern is used in core java classes also, for example _java.lang.Runtime_, _java.awt.Desktop_.

```
import java.util.ArrayList;
import java.util.List;

class singleton {
	private volatile static singleton instance;

	private singleton() {
	}

	public static singleton getinstance() {
		if (instance == null) {
			synchronized (singleton.class) {
				if (instance == null) {
					instance = new singleton();
				}
			}
		}
		return instance;
	}
}
```
https://refactoring.guru/design-patterns/singleton
