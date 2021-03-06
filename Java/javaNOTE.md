### Collection

1. `Collectors.toSet()` returns an unordered `Set`, and please remember `Set` does not have any duplicate elements.  

2. For for loop, it may hit NPE, to avoid that, make sure the list you use have a default value as an empty list instead of NULL.  

### JUnit

#### Use powermockito to return specific results for some methods



### Exception handling

#### Level of exception handling
      Trace - Only when I would be "tracing" the code and trying to find one part of a function specifically.  

      Debug - Information that is diagnostically helpful to people more than just developers (IT, sysadmins, etc.).  

      Info - Generally useful information to log (service start/stop, configuration assumptions, etc). Info I want to always have available but usually don't care about under normal circumstances. This is my out-of-the-box config level.  

      Warn - Anything that can potentially cause application oddities, but for which I am automatically recovering. (Such as switching from a primary to backup server, retrying an operation, missing secondary data, etc.)  

      Error - Any error which is fatal to the operation, but not the service or application (can't open a required file, missing data, etc.). These errors will force user (administrator, or direct user) intervention. These are usually reserved (in my apps) for incorrect connection strings, missing services, etc.  

      Fatal - Any error that is forcing a shutdown of the service or application to prevent data loss (or further data loss). I reserve these only for the most heinous errors and situations where there is guaranteed to have been data corruption or loss.  

#### Principles of exception handling

      1. Make sure the level is right;
      2. Don't append the whole stack trace if you are able to know the reason of the exception by the error message;
      3. Use ex.toString() instead of ex.getMessage() because toString() also includes the exception class name;
      4. Always leave some messages when an exception is caught, so that others can know what happened;
      5. Use getStackTrace() instead of getCause() if the exception was not chained from another layer;
      6. Throwing an exception will end the method immediately;
      7. 

#### Different method of Exception

      printStackTrace(): 
            java.lang.ArithmeticException: / by zero
                at com.thirdbrigade.manager.core.Test.main(Test.java:10)
                
      log.error with e:
            Aug 16, 2017 3:27:51 PM com.thirdbrigade.manager.core.Test main
            SEVERE: error: 
            java.lang.ArithmeticException: / by zero
                at com.thirdbrigade.manager.core.Test.main(Test.java:10)
                
      log.error with e.getStackTrace():
            Aug 16, 2017 3:27:51 PM com.thirdbrigade.manager.core.Test main
            SEVERE: error and [Ljava.lang.StackTraceElement;@27f8302d
            
      print getStackTrace()
            [com.thirdbrigade.manager.core.Test.main(Test.java:12)] (remember to use Arrays.toString(e.getStackTrace()))
            
      print e.getCause()
            null
            
      print e.toString()
            java.lang.ArithmeticException: / by zero
            
      print e.getMessage()
            / by zero

### Object hashCode() and Equals()

      Equals() by default works the same way as '==', which compares the memory address of two objects.
      If we override equals(), we need to make sure that if equals() returns true for two objects, the hashCode() will return the same value from these two objects.
      If we change override() of that object, we need to make sure that equals() perform the way we want.
      
      .equals(...) will only compare what it is written to compare, no more, no less.
      If a class does not override the equals method, then it defaults to the equals(Object o) method of the closest parent class that has overridden this method.
      If no parent classes have provided an override, then it defaults to the method from the ultimate parent class, Object, and so you're left with the Object#equals(Object o) method. Per the Object API this is the same as ==; that is, it returns true if and only if both variables refer to the same object, if their references are one and the same. Thus you will be testing for object equality and not functional equality.
      Always remember to override hashCode if you override equals so as not to "break the contract". As per the API, the result returned from the hashCode() method for two objects must be the same if their equals methods show that they are equivalent. The converse is not necessarily true.
      
      

### Object initialization

		TestObject a;
		TestObject b = new TestObject(3,4);
		TestObject c;
            
a and c will not get any space, and they will not be initialized. You can not do things like: 
            if (a == null) {
                  xxxx;
            }
            
	    
### HashMap

		1. The method get() of HashMap will return null if the key is not existing;
		2. containsKey(null) will return false for sure;
		3. computeIfAbsent() will put the absent value into the map if the value is not null;
		4. 
		

### Stream, parallelStream

             1. Source collection must be efficiently splittable
	     	For structures like ArrayList, it's easy to split it to several small lists, but for structures like LinkedList, it will only make things complicated. 
	     2. Make sure the size is big enough, otherwise does not worth parallel process.
	     3. Also, in an environment where it is already multi-thread, it is also possible to make things worse.
	     4. Be careful of parallelStream as it may have worse performance for single-core machine.
	     5. The per-element function has to be independent.
	     6. When your function is calling some synchronized functionality, then you probably would have no benefit from parallelize your stream due that your parallel streams will often wait on this synchronization barrier.        
            
            
