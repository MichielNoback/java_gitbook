# Unit testing: JUnit 5 basics

## Introduction

In this post, we'll address a topic that is subject to heated debate in development land: testing.
Why would you spend a significant portion of your development time writing code to test other code?
This will be addressed in more detail later, but think about this question first:

**_How much of your time is lost on debugging and solving problems that could have been foreseen and prevented if you would have taken the time to think about special cases?_**

Several major Java unit testing frameworks exist, but we will explore JUnit since it is the most widely used. Also, the concepts you learn with JUnit are easily transferred to other frameworks.

The **_unit_** in unit testing is the method - a named piece of reusable code. If designed well it does one thing only, and it does it well. When creating tests, we **_cover_** the software units (methods) of our code base with one or more test methods - unit tests or **_test cases_**.

Here is an example, to clarify the concept. Suppose you have class `Polygon` that has as field a list of `Coordinate` instances. Here is the `Coordinate` class:

```java
package nl.bioinf;
public class Coordinate {
    private int x;
    private int y;
    
    //constructors, getters and setters omitted
    
    public double distanceTo(Coordinate other) {
        return 0;
    }
}
```

And here is the `Polygon` class:

```java
package nl.bioinf;
import java.util.List;

public class Polygon {
    private List<Coordinate> coordinates;

    //constructors, getters and setters omitted
    

    public void addCoordinate(Coordinate coordinate) {
        if (coordinates == null) {
            coordinates = new LinkedList<>();
        }
        this.coordinates.add(coordinate);
    }
    
    public double getLength() {
        throw new NotImplementedException();
    }
}
```

Now look at the Coordinate and Polygon classes and ask yourself: "Is this code robust?". Can I think of usage scenario's where this class will fail? Or is there even a real bug in there? Take a minute and then read on.

One thing that will immediately alert a trained programmer is the instance variable in the Polygon class: it is being initialized in a "lazy" manner. There is a big fat `NullPointerException` lurking there. When the `getLength()` method is called on an object where no Coordinate has been added yet, `coordinates` will be null.

This is the essence of JUnit testing: it makes you think about cases like this, and will help you create robust code in a systematic way.

A typical test could now be:

```java
package nl.bioinf;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;
public class PolygonTest {
    @Test
    public void getLength() {
        Polygon polygon = new Polygon();
        assertEquals(0, polygon.getLength());
    }
}
```

This test **asserts_** that the length of an empty Polygon is zero - a good choice of **_expected behavior_**. Any other result, including a `NullPointerException`, will make the test fail.

The next step is of course to modify method `getLength()` in such a way that it will pass this test. Here is a simple solution:

```java
    public double getLength() {
        if (coordinates == null) return 0;
        else {
            double distance = 0;
            //calculate distance
            return distance;
        }
    }
```

The how-to of creating and running tests in IntelliJ will be shown shortly.

### Why testing

Test Driven Development (TDD) is today’s standard in software development. 
If you introduce new features a solid test suite protects you against regression in existing code

The approach we’ll take in this course is hand-in-hand development of test and production code. This is one school. The other believes that all test code should be written before any production code.

#### Three reasons for unit testing

1. For any function and given a set of inputs, we can determine if the function is returning the proper values and will gracefully handle failures during the course of execution should invalid input be provided.
2. You'll be writing code that is easy to test: you are more likely to have a higher number of smaller, more focused functions that provide a single operation rather than large functions performing a number of different operations
3. Since you're testing your code as you introduce your functionality, you can prevent changes and additions from breaking functionality
