# The Java Collections API

Storing variables collectively is nice; doing so in Arrays is usually not so 
nice - simply because they are static and cannot be extended or shortened or inserted within.
The Java Collections API offers a wealth of other ways to store data, in the form of Lists, Queues, 
Sets, Maps, in variations and hybrids of these.

Finally (not a collection but essential to show you), we will see a nice way to deal with changeable String data: class StringBuilder

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

### ArrayList

ArrayList is the workhorse for most collections where you want to store a number of variables 
in an ordered way. It is, as the name suggests, an Array-like List. The big -no huge- difference 
is that is will grow and shrink according to your needs. So, the previous method could be 
refactored as follows:

```java
import java.util.ArrayList;

ArrayList<String> readSequences(String fileName) {
    //no initial size needed (although you can give one, 
    //if you are really into efficiency)
    ArrayList<String> sequences = ArrayList<>();

    //open file
    //read sequences
    //return the sequences as an arrays of Strings
}
```

There is an element here that you haven't seen before, it is the **_generic type declaration_**:

<pre style="color:darkblue;font-weight:bold;font-family:courier;font-size:1.2em;">
ArrayList<span style="color:darkred;">< String> </span> sequences = ArrayList<span style="color:darkred;"><></span>();
</pre>




## A contract without implementation: interface


## The collection interfaces: Map, List, Set

--> Different implementations of the List interface to serve different needs.