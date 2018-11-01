##### What is difference between component and service?
A component is a glob of software that’s intended to be used, without change, by an application that is out of the control of the writers of the component. By ‘without change’ means that the using application doesn’t change the source code of the components, although they may alter the component’s behavior by extending it in ways allowed by the component writers.

A service is similar to a component in that it’s used by foreign applications. The main difference is that a component to be used locally (think jar file, assembly, dll, or a source import). A service will be used remotely through some remote interface, either synchronous or asynchronous (eg web service, messaging system, RPC, or socket.)

##### Which is better constructor injection or setter injection?
The choice between setter and constructor injection is interesting as it mirrors a more general issue with object-oriented programming – should you fill fields in a constructor or with setters.
Constructors with parameters give you a clear statement of what it means to create a valid object in an obvious place. If there’s more than one way to do it, create multiple constructors that show the different combinations. Another advantage with constructor initialization is that it allows you to clearly hide any fields that are immutable by simply not providing a setter. I think this is important – if something shouldn’t change then the lack of a setter communicates this very well. If you use setters for initialization, then this can become a pain.

But If you have a lot of constructor parameters things can look messy, particularly in languages without keyword parameters. If you have multiple ways to construct a valid object, it can be hard to show this through constructors, since constructors can only vary on the number and type of parameters. Constructors also suffer if you have simple parameters such as strings. With setter injection you can give each setter a name to indicate what the string is supposed to do. With constructors you are just relying on the position, which is harder to follow.

