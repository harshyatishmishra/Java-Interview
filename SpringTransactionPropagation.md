* We can use @Transactional to wrap a method in a database transaction.

* It allows us to set propagation, isolation, timeout, read-only, and rollback conditions for our transaction. Also, we can specify the transaction manager.

* Spring creates a proxy or manipulates the class byte-code to manage the creation, commit, and rollback of the transaction. In the case of a proxy, Spring ignores @Transactional in internal method calls.

* We can put the annotation on definitions of interfaces, classes, or directly on methods.  They override each other according to the priority order; 
from lowest to highest we have: Interface, superclass, class, interface method, superclass method, and class method.

#### Spring applies the class-level annotation to all public methods of this class that we did not annotate with @Transactional.

#### However, if we put the annotation on a private or protected method, Spring will ignore it without an error.

#### Case1
    `
    @Transactional
    public class UserServiceImpl implements UserService {

        ...................
        public void method1(){
            try{
                method2();
            }catch(Exception e){

            }
        }
        public void method2(){

        }
    }
    `
#### Case 2
    `
    public class UserServiceImpl implements UserService {

        ...................
        public void method1(){
            try{
                method2();
            }catch(Exception e){

            }
        }
        @Transactional
        public void method2(){

        }
    }
    `
* In case 1 @Transactional is applied to every public individual method. Private and Protected methods are Ignored by Spring.
* In case 2 @Transactional is only applied to method2(), not on method1()
* Case 1: - Invoking method1() -> a transaction is started. When method1() calls method2() no new transaction is started, because there is already one.
* Case 2: - Invoking method1() -> no transaction is started. When method1() calls method2() NO new transaction is started. This is because @Transactional does not work when calling a method from within the same class. It would work if you would call method2() from another class.    

#### Transaction Propagation
* Propagation defines our business logic ‘s transaction boundary. Spring manages to start and pause a transaction according to our propagation setting.

* #### ` REQUIRED is the default propagation.` Spring checks if there is an active transaction, then `it creates a new one if nothing existed. Otherwise, the business logic appends to the currently active transaction`.

    `
    @Transactional(propagation = Propagation.REQUIRED)
    public void requiredExample(String user) { 
        // ... 
    }
    `
* #### `SUPPORTS`, Spring first checks if an active transaction exists. If a transaction exists, then the existing transaction will be used. `If there isn't a transaction, it is executed non-transactional`:
    `
      @Transactional(propagation = Propagation.SUPPORTS)
    `
*  #### `MANDATORY`, if there is an active transaction, then it will be used. If there isn't an active transaction, then Spring throws an exception.
* #### `NEVER` propagation, Spring throws an exception if there's an active transaction.
* #### `NOT_SUPPORTED Propagation` Spring at first suspends the current transaction if it exists, then the business logic is executed    without a transaction.
* #### `REQUIRES_NEW`, Spring suspends the current transaction if it exists and then creates a new one.

For `NOT_SUPPORTED` and 'REQUIRES_NEW' JTATransactionManager supports real transaction suspension out-of-the-box. Others simulate the suspension by holding a reference to the existing one and then clearing it from the thread context.

#### `NESTED propagation`, Spring checks if a transaction exists, then if yes, it marks a savepoint. This means if our business logic execution throws an exception, then transaction rollbacks to this savepoint. If there's no active transaction, it works like REQUIRED.

#### DataSourceTransactionManager supports this propagation out-of-the-box. Also, some implementations of JTATransactionManager may support this.

#### JpaTransactionManager supports NESTED only for JDBC connections. However, if we set nestedTransactionAllowed flag to true, it also works for JDBC access code in JPA transactions if our JDBC driver supports savepoints.

    `@Transactional(propagation = Propagation.NESTED)`

### Transaction Isolation
* Isolation is one of the common ACID properties: Atomicity, Consistency, Isolation, and Durability. Isolation describes how changes applied by concurrent transactions are visible to each other.

* Each isolation level prevents zero or more concurrency side effects on a transaction:

* `Dirty read`: read the uncommitted change of a concurrent transaction
* `Nonrepeatable read`: get different value on re-read of a row if a concurrent transaction updates the same row and commits
* `Phantom read`: get different rows after re-execution of a range query if another transaction adds or removes some rows in the range and commits
* We can set the isolation level of a transaction by @Transactional::isolation. It has these five enumerations in Spring: DEFAULT, READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE.

#### READ_UNCOMMITTED is the lowest isolation level and allows for most concurrent access.

* As a result, it suffers from all three mentioned concurrency side effects i.e. Dirty Read, Nonrepeatable, Phantom read. So a transaction with this isolation reads uncommitted data of other concurrent transactions. Also, both non-repeatable and phantom reads can happen. Thus we can get a different result on re-read of a row or re-execution of a range query.

    `@Transactional(isolation = Isolation.READ_UNCOMMITTED)`
    
#### READ_COMMITTED Isolation, is the second level of isolation, READ_COMMITTED, prevents dirty reads.

* The rest of the concurrency side effects still could happen. So uncommitted changes in concurrent transactions have no impact on us, but if a transaction commits its changes, our result could change by re-querying.
##### READ_COMMITTED is the default level with Postgres, SQL Server, and Oracle.

#### REPEATABLE_READ, prevents dirty, and non-repeatable reads. So we are not affected by uncommitted changes in concurrent transactions.
* Also, when we re-query for a row, we don't get a different result. But in the re-execution of range-queries, we may get newly added or removed rows.
* Moreover, it is the lowest required level to prevent the lost update. The lost update occurs when two or more concurrent transactions read and update the same row. REPEATABLE_READ does not allow simultaneous access to a row at all. Hence the lost update can't happen.
##### REPEATABLE_READ is the default level in Mysql. Oracle does not support REPEATABLE_READ.

#### SERIALIZABLE is the highest level of isolation. It prevents all mentioned concurrency side effects but can lead to the lowest concurrent access rate because it executes concurrent calls sequentially.
* In other words, concurrent execution of a group of serializable transactions has the same result as executing them in serial.
