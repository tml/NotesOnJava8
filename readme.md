# Intro
These are my own notes to a week long Java 8 course.  These notes are designed to provide a quick overview of major changes or new ideas, it is not a comprehensive guide and certainly does not outline all changes introduced with Java 8.  But for any seasoned developers, it should serve as a good refresher.

_The Java 8 release is arguably the biggest update to the Java lang since ever (even bigger than 5)._

## Table of Contents

- [Day 1](#day-1): [What's New in Java 8](#whats-new-in-java-8), [Interface Additions](#interface-addtions), [Functional Programming](#functional-programming), [New Stream API](#new-stream-api), [New Optional Type](#new-optional-type)  
- [Day 2](day2.md): 

# Day 1
## What's New in Java 8
- StreamAPI most important part of Java 8, builtin support for mapreduce
- Java almost as if transitioning from OO to scripting (think Python) middleground
- Finally supports lambdas/closures (a block of code/fn that we can pass around as data)
-- not new, we used to do it with eventhandlers, now though we simplified the syntax 
-- and no longer need interface
- Added foreach method for collections
- Redesigned underlying multithreading code
- New Javascript scripting engine (for Java app extensibaility)
- Default and static methods in interfaces
- Replaced the DateTime API


Note: JRE 8 is fully backward compatible with bytecode of previous Java versions.

## Interface Additions
Can now have static methods in interfaces to provide utility methods.

```
public interface AgendaItem {
	static Comparator<AgendaItem> getDateTimeComparator() {
		return new Comparator<AgendaItem>() {

			@Override
			public int compare(AgendaItem o1, AgendaItem o2) {
				if (o1 == null)
					return -1;
				if (o2 == null)
					return 1;
				return o1.getDateTime().compareTo(o2.getDateTime());
			}
		};
	}
}
```

Can now have default methods to leave existing implementations unchanged as you eventually end up changing your interface.
```
public interface DescriptionItem {

	default String getDescription() {
		return "descriptionItem: "+toString();
	}
}
```

## Functional Programming
Lambda API, 3 interfaces: 
- Consumer (takes 1 param and returns a void)
- Function (takes 1 param and returns a value)
- BiFunction (takes 2 params and returns a value)

All these interfaces must only have one abstract method (this is what gets implemented as the lambda) aka Single Abstract Method interfaces.  All interfaces that are like this are also called functional interfaces and can be used for lambdas.

Examples: 
`list.forEach(myStr -> System.out.println(myStr))`
`list.forEach(System.out::println)` _wraps the println fn into a lambda_

Starting a new thread now takes one line...
`(new Thread(() -> {System.out.println(Thread.currentThread().getName)} )).start()`

Type inference has been improved (e is an ActionEvent that I need not declare to use it)
`button.addActionListener(e -> System.out.println("Button Clicked!"))`

An example of a BiFunction
`Comparator<AgendaItem> c = (AgendaItem a1, AgendaItem a2) -> a1.getDateTime().compareTo(a2.getDateTime())`

We can access non final variables in lambda that are from the enclosing scope. It will be treated as final, and you can't assign a value to it (even from outside the lambda)

In a lambda expression, this, references the enclosing class, rather than the inner class.


## New Stream API

Declartive programming, describe what the app should do- instead of how it should do it.
Ie. compare your SQL statement with any nontrivial collection getter Java function

The Java Stream API allows for a declarative programming approach.
```
public List<DVD> getDVDsByStudio(String studio) {
	Stream<DVD> dvds = DVDDao.getAllDVDs().filter(s -> studio.equals(s.getStudio()));
	return dvds.collect(Collectors.toList());
}

public List<String> getGenres() {
	Stream<String> dvds = DVDDao.getAllDVDs().map(DVD::getGenre).distinct().sorted();
	return dvds.collect(Collectors.toList());
}
```
Very similar to Hibernate framework.

Standard collections framework is not thread-safe.

Streams allow us to manipulate collections of data in a thread-safe way.  It handles
synchronization on retrieval and on terminal stream.  Intermediary steps should NOT modify the collection (if you were to roll your own).


##New Optional Type
Optional acts as a container for objects or it could be null.  It is used instead of returning null. `optional.isPresent()` to check if an object exists.

Helps address `NullPointerExceptions`.  Seems like a solution looking for a problem.

However, my declaring your methods to return an Optional, it serves as a reminder that the method may return a null.

[Go to Day 2 >>](/day2.md)