# notes-apache-maven
My notes on Apache Maven

## Why Maven?

Let's make a simple Hello World program in Java.

Step 1: Create the `HelloWorld.java` file.

```console
% vi HelloWorld.java HelloWorld.java
```

```java
public class HelloWorld {
        public static void main(String[] args) {
                System.out.println("Hello World!");
        }
}
```

Step 2: Compile the `HelloWorld.java` file, essentially compiling the source file (files with file name extension of `.java`) to a class file (file name extension of `.class`).

<p align="center"><code>.java </code> &rarr; <code>.class</code></p>
<p align="center">Source Code &rarr; Bytecode</p>

```console
% javac HelloWorld.java

% ls
HelloWorld.class	HelloWorld.java
```

Step 3: Use Java JVM to execute the Bytecode.

```console
% java HelloWorld
Hello World!
```

Step 4: Create JAR file.

```console
% jar cf myjar.jar HelloWorld.class

% ls
HelloWorld.class	HelloWorld.java		myjar.jar
```

Step 5: Add `myjar.jar` to CLASSPATH and execute the `HelloWorld.class` from within it.

```console
% java -classpath myjar.jar HelloWorld
Hello World!
```

Step 5: Make a `/temp`directory and copy `myjar.jar` into it and then change the present working directory to `/temp`.

```console
% mkdir temp
% cp myjar.jar ./temp
% cd temp
% ls
myjar.jar
```

Step 6: Since JAR file is nothing but a zipped file. Let's unzip it.

```console
% unzip myjar.jar
Archive:  myjar.jar
   creating: META-INF/
  inflating: META-INF/MANIFEST.MF
  inflating: HelloWorld.class

% ls
HelloWorld.class	META-INF		myjar.jar
```

Step 7: Let's checkout the `META-INF/MANIFEST.MF` file.

```console
% vi META-INF/MANIFEST.MF
Manifest-Version: 1.0
Created-By: 17.0.7 (Eclipse Adoptium)
```

When we are building Java with Maven; Maven would be doing all of this for us. Building `.class` files from `.java` files, making the JAR files, and asking us what we want in the `MANIFEST.MF` file.


### Using 3rd party JARs with Java in the CLI

We are going to use Apache Commons Lang ([link](https://commons.apache.org/proper/commons-lang/)) as the 3rd party JAR. 

Step 1: Download the Apache Commons Lang JAR (from [here](https://commons.apache.org/proper/commons-lang/download_lang.cgi)). I am using Apache Commons Lang 3.12.0 (Java 8+) with Java 17. You can use any depending on the supported Java verison. 

Step 2: Make your directory look like below:
```console
% ls
HelloWorld.java	lib

% ls lib
commons-lang3-3.12.0.jar
```

Step 3: `HelloWorld.java` should look like below:

```java
import org.apache.commons.lang3.StringUtils;

public class HelloWorld {
        public static void main(String[] args) {
                System.out.println("Hello World!");
                System.out.println(StringUtils.capitalize("hello apache!"));
        }
}
```

Step 4: Compile with the 3rd party library. Refer [1](#refer1)

```console
% javac -classpath ./lib/* HelloWorld.java
```

OR 

```console
% javac -cp lib/* HelloWorld.java
```

Step 5: See what the previous step created.

```console
% ls
HelloWorld.class	HelloWorld.java		lib
```

Step 6: Run

```console
% java -cp ":./lib/*" HelloWorld
Hello World!
Hello apache!
```

This is how we have to manually manage the dependencies. If we use Maven, these dependencies will be managed for us. Depency management would be done by Maven. 

References:

---

<a name="refer1">[1]</a> javac - http://web.mit.edu/java_v1.1.6/www/tools/javac.html
