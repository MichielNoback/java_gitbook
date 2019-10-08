# JUnit 5 advanced features

## Introduction

This post deals with (slightly) more advanced JUnit features  

- usage of annotations other than `@Test`
- the assertJ library for writing assertions

## JUnit 5 annotations

You probably have seen annotations in your Java code by now, for example `@Override`
 or `@SuppressWarnings("unused")`. 
Annotations are a form of syntactic metadata that can be added to Java source code (classes, methods, variables). The attach "properties" to classes, methods or variables.

Unit has a whole suite of annotations that you can use to publish a test case to the test runtime, or to modify when it is run, or its scope, for instance.

Here are a few. for a complete listing, please refer to the official docs.

### @Test

This is the most important one. It marks a method as a JUnit test method, so that it can be discovered by the test runtime. Nothing else is required. 

```java
@Test
public void testImportant() {
    String first = "Michiel";
    String second = "Michiel";
    //What do you think - will this pass?
    assertSame(first, second);
}
```

### @Disabled

The `@Disabled` annotation is used to (temporary) disable a test. It is not used very often, because forgetting to remove it again is a lurking danger. A better alternative is to use the `@Tag` annotation which is discussed in the next post.

### @DisplayName

Use the `@DisplayName` annotation to have the method represented by a more human-readable message. So instead of "testLongestWordWithSingleSpace()" you could have `@DisplayName("Single space input to longestWord()")`. unfortunately, this feature is not satisfactorily supported in IntelliJ when using Gradle as Test Runner: It does not show the display name in the Run panel. The Gradle test report does show the display names correctly however.

You can sort of "fix" this by choosing to have IntelliJ IDEA run your tests instead of Gradle. This can be done via  
Preferences &rarr; Build, Execution, Deployment &rarr; Build Tools &rarr; Gradle &rarr; choose IntelliJ IDEA from the pulldown at "Run tests using".  
Unfortunately, this disables the `@Tag` selection mechanism which is discussed in the next post...

I recommend always using the `@Displayname` annotation. Support for it will undoubtedly improve.

### @BeforeEach and @AfterEach

@BeforeEach and @AfterEach run respectively before and after each test case. You use them to re-initialize objects that may have been changed during a previous test.

For instance, a Polygon object under test:

```java
private Polygon polygon;

@BeforeEach
public void resetPolygon() {
    polygon = new Polygon();
    polygon.addCoordinate(new Coordinate(1, 2));
    polygon.addCoordinate(new Coordinate(3, 6));
}
```

### @BeforeAll and @AfterAll

@BeforeAll and @AfterAll annotations are similar to @AfterEach and @BeforeEach with the difference that they are called **_once per test class execution_** and not on a per-test basis. These are used to initialize class level resources (i.e. load a properties file).

**_Methods annotated with `@BeforeAll` or `@AfterAll` the should be static._**

### @Tag

The `@Tag` annotation is a much more sophisticated way of switching (groups of) tests on and off. You use them in combination with a configuration of your test runtime (in this case Gradle).

Here are two tagged methods.

```java
@Tag("sometimes")
@DisplayName("Tag demo method 1: 'sometimes'")
@Test
public void tagDemo1() {
    System.out.println("Tag demo 1 running");
}

@Tag("rarely")
@DisplayName("Tag demo method 2: 'rarely'")
@Test
public void tagDemo2() {
    System.out.println("Tag demo 2 running.");
}
```

Assuming all other test methods are untagged; this declaration in your `build.gradle` file

```groovy
test {
    useJUnitPlatform {
        excludeTags 'rarely'
    }
}
```

will result in `tagDemo1()` being executed as well as all other tests - also the ones without any tag. So this only excludes `tagDemo2()` and any other class or test having this tag.

On the other hand, this configuration

```groovy
test {
    useJUnitPlatform {
        includeTags 'sometimes'
    }
}
```

will result in **_only_** `tagDemo1()` being executed.

You can `@Tag` single methods as well as classes.

### @Nested


### @ParameterizedTest

You can repeat the same test with different inputs using the `@ParameterizedTest` annotation.






