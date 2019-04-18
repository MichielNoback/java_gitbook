# The Java Collections API

Storing variables collectively is nice; doing so in Arrays is usually not so 
nice - simply because they are static and cannot be extended or shortened or inserted within.
The Java Collections API offers a wealth of other ways to store data, in the form of Lists, Queues, 
Sets, Maps, in variations and hybrids of these.

Finally (not a collection but essential to show you), we will see a nice way to deal with changeable String data: class StringBuilder.

#### The old way
In a previous lecture, you have seen the most basic collection type: array.
Arrays store a fixed number of variables. But suppose you want to do something like this:

```java
Sequence[] readSequences(String fileName) {
    //open file
    //read sequences
    //return the sequences as an array of Sequence objects
}
```

There is a problem here: You usually don’t know how many sequences there are in a file!
So, the simple solution is to create a very large array that is as large as the biggest sequence file you know:

```java
String[] readSequences(String fileName) {
    String[] sequences = new String[10_000_000];
    //open file
    //read sequences
    //return the sequences as an arrays of Strings
}
```

When you read only 1 or 2 sequences you will have wasted all this memory space! Moreover, all 
these null values are `NullPointerExceptions` in the waiting.

![unused](figures/unused_array_space.png)

When somebody invents a new sequencing machine that generates 100 million sequences 
in one run, you have a bigger problem! You will get `ArrayIndexOutOfRangeExceptions` 
or other errors. Actually, these machines already exist!

There is a nice class that will really make your day: `java.util.ArrayList`.

## Lists: ArrayList and LinkedList

ArrayList is the workhorse for most collections where you want to store a number of variables 
in an ordered way. It is, as the name suggests, an Array-like List. The big -no huge- difference 
is that is will grow and shrink according to your needs. So, the previous method could be 
refactored as follows:

```java
import java.util.ArrayList;

ArrayList<String> readSequences(String fileName) {
    //no initial size needed (although you can give one, 
    //if you are really into efficiency)
    ArrayList<String> sequences = new ArrayList<>();

    //open file
    //read sequences
    //return the sequences as an arrays of Strings
}
```

There is an element here that you haven't seen before, it is the **_generic type declaration_**:

<pre style="color:darkblue;font-weight:bold;font-family:courier;font-size:1.2em;">
ArrayList<span style="color:darkred;">&lt;String&gt; </span>sequences = new ArrayList<span style="color:darkred;">&lt;&gt;</span>();
</pre>

## Generics

The `<String>` and `<>` have everything to do with Java being strongly typed. Since everything is typed, a collection that is being instantiated must also know what type it will contain. You specify the type of its elements using the **_diamond operators_**. This mechanism is called **_generics_** in Java, because it makes collection classes generic usable.
Before Java 8, you had to do it twice (`ArrayList<String> sequences = new ArrayList<String>();`) but in newer versions of Java you can leave the second set of diamond operators empty. 

Leaving the type declaration out does not yield a compiler error, but makes your life so much harder:

```java
//typed collection
ArrayList<String> words = new ArrayList<>();
words.add("Game");
words.add("of");
words.add("Thrones");

//type is inferred from collection
for (int i = 0; i < words.size(); i++){
    String word = words.get(i);
    System.out.println("word = " + i + ": " + word);
}

//"raw" type collection: everything is Object
ArrayList wordsNonGeneric = new ArrayList();
wordsNonGeneric.add("House");
wordsNonGeneric.add("of");
wordsNonGeneric.add("Cards");
//danger!
wordsNonGeneric.add(new Duck());

//iterate over Object type
for (int i = 0; i < wordsNonGeneric.size(); i++){
    //need to cast to actual type
    //this will give a ClassCastException exception on the Duck, which is of course not a String!
    String word = (String)wordsNonGeneric.get(i);
    System.out.println("word = " + i + ": " + word);
}
```

So, without the type declaration, you need to cast to the actual type. That makes your code less obvious. But more problematic is that _any_ object can be inserted into the collection, without warning. So, when you attempt to cast a Duck to a String, you get a `ClassCastException`. You can circumvent this problem by checking the type (using the `instanceof` operator):

```java
for (int i = 0; i < wordsNonGeneric.size(); i++){
    Object element = wordsNonGeneric.get(i);
    if (element instanceof String) {
        String word = (String) element;
        System.out.println("word = " + i + ": " + word);
    } else {
        System.out.println("skipped non-String element of type " 
            + element.getClass().getSimpleName());
    }
}
```

but why would you make your life harder than it already is?

## ArrayList operations

Lists in general, and ArrayList is no exception, are used to store in an ordered way. Operations on ArrayLists are of course related to the property. Here are some example usages.

```java
//create
ArrayList<String> words = new ArrayList<>();
//add elements
words.add("Game");
words.add("of");
words.add("Thrones");
//won't compile!
//words.add(new Duck());

//foreach iteration
for (String word : words) {
    System.out.println("word = " + word);
}

//iteration with counter; note the use of ".size()"
for (int i = 0; i < words.size(); i++) {
    //fetch by index - zero based!
    String word = words.get(i);
    System.out.println("word = " + i + ": " + word);
}

words.contains("Thrones"); //true
words.size(); //3
words.isEmpty(); //false
//same as
boolean empty = words.size() == 0;

words.remove("of"); //deletes word
words.remove(1); //second element
words.clear(); //empty list
```

## Code against interfaces, not implementations

Be aware that the `contains()` method performs at **O(n)** (see your Algorithms & Data structures course!), so if you plan to check for
element presence often, List types are not a good choice. Also, if you are going to do a lot of insert and/or delete operations, LinkedList is a much better choice. Fortunately, you only need to change one line of code in your application to achieve this, and if you design your code well, you only need to change a single word:

```java
//This is OK but not efficient with insert/delete 
//Also, the type declaration is an implementation (ArrayList) and not an 
//interface (List)
ArrayList<String> words = new ArrayList<>();

//Better, declare the interface (abstraction) type
List<String> words = new ArrayList<>();

//Best: LinkedList is very good at insertions and deletions
List<String> words = new LinkedList<>();
```

This is an example of the rule **_code against interfaces (abstractions), not implementations_**.




## The collection interfaces: Map, List, Set

--> Different implementations of the List interface to serve different needs.
