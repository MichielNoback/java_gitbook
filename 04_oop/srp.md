# The Single Responsibility Principle

## Introduction  

This post deals with the most important design principle of all - the **Single Responsibility Principle**, or **SRP**. It applies to all levels of code: variables, methods, classes, packages, and even jars (code libraries). Let's start with everything at and below the class level. Have a look at the class code below and see if you can find all SRP violations.  

```java
package nl.bioinf.srp_demo;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

/**
 * my bogus but excellent Foo app.
 * @author michiel
 */
public class MyFooApp {
    /**
     * Foo counter.
     * If count is below zero, counting has not yet been initialized.
     */
    private static int myFooCounter = -1;
    /**
     * DB connection.
     */
    private Connection conn;

    /**
     * starts the app.
     * @throws SQLException ex
     */
    public void start() throws SQLException {
        //some DB init code; showing only one line of it
        conn = DriverManager.getConnection("/path/to/db");
        fetchFoos();
    }

    /**
     * fetches foo objects from DB.
     */
    private void fetchFoos() {
        //1: fetching Foo objects from DB
        //2: displaying them in the GUI
        displayFoos();
    }

    /**
     * displays Foo objects in GUI.
     */
    private void displayFoos() {
        //displaying fetched Foo objects in GIU
    }

    /**
     * init code.
     */
    private static void initStatics() {
        myFooCounter = 0;
    }

    /**
     * main.
     * @param args the CL args
     */
    public static void main(final String[] args) {
        MyFooApp main = new MyFooApp();
        initStatics();
        try {
            main.start();
        } catch (SQLException ex) {
            System.err.println("something went terribly wrong!");
        }
    }
}
```

Well, yeah, I know. This is really terrible. There are so many SRP violations it makes me dizzy. However, this is the coding style quite regularly created by my students. Even though I have been hammering away about this SRP thingy for a full 7 weeks. This post is for you (feel free to feel personally addressed).  

Let’s start with the top level of this code. This single class holds a main() method and a bunch more methods. When there’s a main method, this means the purpose of the class is starting the app. So…that should be the only purpose of the class: starting the app. Let’s throw everything else out, and have the main class adhere to SRP.

```java 
package nl.bioinf.srp_demo;

/**
 * Class with main method used to launch my Foo app from a t environment.
 */
public final class MyFooAppLauncher {
    /**
     * @param args the command line arguments
     */
    public static void main(final String[] args) {
        //maybe doing something with the CL args, but that is for another post.
        MyFooAppController control = new MyFooAppController();
        control.start();
    }
}
```

As you can see, I also took out the try/catch stuff. In my opinion, Exceptions should 
never end up in main(). You may think this is really exaggerating the point, but do you
still think so when you have read the class level Javadoc? What if your excellent app 
will also end up in a web or other environment? You won’t need the main class, but you 
will probably need the controller and other classes. So, ideally, the main class 
does nothing more than start up the application, in this case via the controller class.

The controller will, well, control  the application. It needs to set up some internal 
environment variable, initialize the database connectivity and the GUI (whatever that 
may be). I will assume an extremely simple app here; otherwise this post will get 
class-bloat really fast.  

Which brings us to the next code snippet.

```java
package nl.bioinf.srp_demo;

import java.util.logging.Level;
import java.util.logging.Logger;

class MyFooAppController {
    /**
     * Foo counter.
     * If count is below zero, counting has not yet been initialized.
     */
    private static int myFooCounter = -1;
    /**
     * Foo DB
     */
    private FooDataManager fooDb;

    /**
     * init code.
     */
    private static void initStatics() {
        myFooCounter = 0;
    }
    public void start() {
        initStatics();
        try {
            initDB();
            initGUI();
        } catch (FooException ex) {
            Logger.getLogger(MyFooAppController.class.getName()).log(Level.SEVERE, null, ex);
        }
    }

    public void initDB() throws FooException {
        this.fooDb = new FooDataManager();
    }

    public void initGUI() {
        //GUI initialization, resulting in a instance field 
        //providing access to GUI related tasks
    }

    /*NOT SHOWN: callback methods for generating GUI response*/
}
```

The responsibility of the controller is setting up the app and managing requests and responses from and to the GUI. Since there is the word and in the previous sentence, there is room for discussion here as we... (Two responsibilities?)  

The **FooDataManager** class (not shown) will be responsible for data access; it is the 
only class that will know what the actual data source is (file, MySQL, HBase, whatever). 
There is much to tell about this aspect as well, covered in another post (to be written) 
on Abstraction, Encapsulation and loose coupling.

Another class, `MyGui` (also not shown) will play a similar role with respect to the GUI. 
It will be the only class with knowledge of the actual implementation (JavaFX, Swing, 
QT) technology, only providing public methods that serve as communication channels 
between controller and GUI (e.g. `displayFoos(List<Foo> foos`) which will cause the 
GUI to update its displayed list of Foo objects).

Finally, you see a try/catch block with a Logger.getLogger… call. When creating a 
serious Java application, never ever use `System.out.println()` or `System.err.println()` 
statements, but defer managing messages (of any form) to dedicated classes. Managing 
system messages is a Single Responsibility on its own. By the way, using `System.out` 
messages for debugging is really bad – although of course everyone uses it. 
But debugging will be dealt with in a later post.

So, why is this SRP thingy so important, besides your teacher being really un-chill 
about it? Since Java is often used for larger applications, in a tested, complex 
environment, classes and methods should have only one reason to change (and break 
the application logic). Having code with only one responsibility makes it easier 
to test, and leaves only one reason to revisit it to expand, change or debug 
the code.

That’s it.

Oh, almost forgot; there’s still one big SRP violation left. Did you spot it? It’s 
the static variable myFooCounter of course. See if you can figure out how to solve 
this.
