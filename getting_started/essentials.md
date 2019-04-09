# The essence of Java

This part deals with the essentials of the Java programming language. Java is a very popular platform for several reasons: it is the basis of Android, it is embedded in many domestic appliances, and it has a nice clear object-oriented design.

## Cross-platform

Java is cross-platform in the sence that once it is compiled into an executable, it can be run on any operating system that has a Java Runtime installed. This Java installation intantiates a Java Virtual Machine (**JVM**) and passes your compiled code to it, including instructions on which main() method to start on.

## Object-Oriented

In Java, you cannot run a script. Instead, you write a program where everything you create resides in **_classes_**. Classes reside in their own source file. One of these classes must contain a `main()` method. You let the JVM know which class has the main method you want executed.
Besides the raw -primitive- data types such as integer (`int`) and `double`, all types are reperesented by classes, which define the blueprint to contstruct `objects`. So objects are the representations (**_instances_**) of a single class - more formally called a **_type_**. Java provides many types, and you will routinely define your own types when programming.

**A class is a type blueprint that is used to instantiate objects**

These objects, the number of which can range from one to many millions within a running application, are the fabric of the program. They hold data (called **_instance variables_**) and **_methods_** that define their behaviour and functionality. 
 
## Strongly typed and scoped

This aspect can at first be a bit daunting when coming from languages such as Python or R. Every variable you use must have a **_declared type_**. Moreover, every **_member_** (instance variable or method) has **_keywords_** defining its scope/visibility. Here is an example class that shows these aspects. Read the comments (between `/**` and `*/`) to see what everything means. You do not have to remember or understand all these keywords yet, only get an idea of their role within the code.

```java
package snippets;

/**
 * Class Snp models a Single Nucleotide Polymorphism in its simplest form.
 * The class is public, so any class within the project may instantiate it.
 */
public class Snp {
    /**
     * These are the INSTANCE VARIABLES that model the SNPs data
     * They are PRIVATE, so they can not be accessed from outside this class.
     * The types are long (integer) and character (single letter)
     */
    private long position;
    private char referenceNucleotide;
    private char alternativeNucleotide;

    /**
     * This is the CONSTRUCTOR method that forces client code to provide the three essential
     * properties of a Snp that every INSTANCE should have defined before being INSTANTIATED.
     * It is public so accessible to all. Properties are passed as constructor arguments and
     * stored as INSTANCE VARIABLES.
     * @param position a long integer
     * @param reference a single character: A, C, G or T
     * @param alternative a single character: A, C, G or T
     * @throws IllegalArgumentException if the given position is negative
     */
    public Snp(long position, char reference, char alternative) {
        if (position < 1) {
            throw new IllegalArgumentException("postition must be positive");
        }
        this.position = position;
        this.referenceNucleotide = reference;
        this.alternativeNucleotide = alternative;
    }

    /**
     * a GETTER for the position. Since the position field is private, this makes it a READ_ONLY
     * property.
     * @return the position as long integer
     */
    public long getPosition() {
        return position;
    }

    public char getReferenceNucleotide() {
        return referenceNucleotide;
    }

    public char getAlternativeNucleotide() {
        return alternativeNucleotide;
    }

    /**
     * this method tells a client whether ths SNP is a transition or a transversion.
     * The return type is boolean (true or false).s
     * @return
     */
    public boolean isTransition() {
        return ((referenceNucleotide == 'A' && alternativeNucleotide == 'G') || (referenceNucleotide == 'C' && alternativeNucleotide == 'T'));
    }
}
```

## Compiled

Java is a compiled language, which means you have to compile the source into **_byte code_** before it can be run. Fortunately, you have to compile it only once, for the JVM and not multiple times, for every OS you want to support.

