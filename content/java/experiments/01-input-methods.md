---
title: Java Input Methods Experiment
---

# Java Input Methods Experiment

This experiment explores various Java input mechanisms, ranging from the classic `BufferedReader` to the modern `NIO Files API`. Each method has its own strengths, performance characteristics, and ideal use cases.

## Overview

Java provides multiple ways to handle input. Choosing the right one depends on whether you need ease of use, high performance, or secure input handling.

| Method | Type | Best For |
| :--- | :--- | :--- |
| **Scanner** | Token-based | Simple console apps, easy parsing |
| **BufferedReader** | Line-based | Large files, performance-critical apps |
| **Console** | System-based | Secure password input |
| **DataInputStream** | Byte-based | Reading binary primitive data |
| **InputStreamReader** | Character-based | Bridge from bytes to characters |
| **StreamTokenizer** | Lexical | Simple parsing/tokenization |
| **NIO Files API** | File-based | Modern, efficient file operations |

---

## 1. Scanner

The `Scanner` class is the most common way to read user input. It's easy to use but slower than alternatives for large datasets.

```java
import java.util.Scanner;

public class ScannerExample {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in); // (1)
        
        System.out.print("Enter your name: ");
        String name = sc.nextLine(); // (2)
        
        System.out.print("Enter your age: ");
        int age = sc.nextInt(); // (3)
        
        System.out.println("Hello " + name + ", age " + age);
        sc.close();
    }
}
```

1.  Initializes the `Scanner` to read from the standard input stream.
2.  Reads an entire line of text, including spaces.
3.  Automatically parses the input into an integer. Be careful with the leftover newline character!

---

## 2. BufferedReader

`BufferedReader` is more efficient for large inputs because it buffers the data. It's often paired with `InputStreamReader`.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;

public class ReaderExample {
    public static void main(String[] args) throws IOException {
        BufferedReader reader = new BufferedReader(
            new InputStreamReader(System.in) // (1)
        );
        
        System.out.print("Enter some text: ");
        String line = reader.readLine(); // (2)
        
        System.out.println("You entered: " + line);
    }
}
```

1.  Wraps `InputStreamReader` for efficient character-based reading.
2.  Reads a line of text. Unlike `Scanner`, it is synchronized and thread-safe.

---

## 3. Console

The `Console` class provides methods specifically for interacting with the system console, including secure password input.

```java
import java.io.Console;

public class ConsoleExample {
    public static void main(String[] args) {
        Console console = System.console(); // (1)
        
        if (console == null) {
            System.err.println("No console available");
            return;
        }
        
        String username = console.readLine("Username: ");
        char[] password = console.readPassword("Password: "); // (2)
        
        System.out.println("User " + username + " logged in.");
    }
}
```

1.  Gets the unique `Console` object associated with the current Java virtual machine. May return `null` in some IDEs.
2.  Reads a password without echoing the characters to the screen.

---

## 4. DataInputStream

Used for reading primitive Java data types from an underlying input stream in a machine-independent way.

```java
import java.io.DataInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class DataInputExample {
    public static void main(String[] args) throws IOException {
        try (DataInputStream dis = new DataInputStream(new FileInputStream("data.bin"))) {
            int value = dis.readInt(); // (1)
            double price = dis.readDouble(); // (2)
            System.out.println("Int: " + value + ", Double: " + price);
        }
    }
}
```

1.  Reads 4 bytes and interprets them as an `int`.
2.  Reads 8 bytes and interprets them as a `double`. Great for binary files.

---

## 5. InputStreamReader

Acts as a bridge from byte streams to character streams. It decodes bytes into characters using a specified charset.

```java
import java.io.InputStreamReader;
import java.io.FileInputStream;
import java.nio.charset.StandardCharsets;

public class BridgeExample {
    public static void main(String[] args) throws Exception {
        InputStreamReader isr = new InputStreamReader(
            new FileInputStream("test.txt"), 
            StandardCharsets.UTF_8 // (1)
        );
        
        int data = isr.read(); // (2)
        while (data != -1) {
            System.out.print((char) data);
            data = isr.read();
        }
        isr.close();
    }
}
```

1.  Specifies the character encoding to use for decoding bytes.
2.  Reads characters one by one. Usually wrapped in `BufferedReader` for better performance.

---

## 6. StreamTokenizer

Takes an input stream and parses it into "tokens", allowing the stream to be processed one word or number at a time.

```java
import java.io.StreamTokenizer;
import java.io.StringReader;

public class TokenizerExample {
    public static void main(String[] args) throws Exception {
        StreamTokenizer st = new StreamTokenizer(new StringReader("Hello 123 World"));
        
        while (st.nextToken() != StreamTokenizer.TT_EOF) {
            if (st.ttype == StreamTokenizer.TT_WORD) { // (1)
                System.out.println("Word: " + st.sval);
            } else if (st.ttype == StreamTokenizer.TT_NUMBER) { // (2)
                System.out.println("Number: " + st.nval);
            }
        }
    }
}
```

1.  `sval` contains the string value if the token is a word.
2.  `nval` contains the numeric value if the token is a number.

---

## 7. NIO Files API

The modern way to handle file I/O in Java. Highly efficient and easy to use with Java Streams.

```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.List;
import java.util.stream.Stream;

public class NioExample {
    public static void main(String[] args) throws Exception {
        Path path = Path.of("test.txt");
        
        // Method A: Read all lines (small files)
        List<String> allLines = Files.readAllLines(path); // (1)
        
        // Method B: Stream lines (large files)
        try (Stream<String> lines = Files.lines(path)) { // (2)
            lines.filter(l -> l.contains("Java"))
                 .forEach(System.out::println);
        }
    }
}
```

1.  Reads the entire file into memory as a list of strings.
2.  Lazy-loads lines from a file as a stream, perfect for processing large files line by line.

---

> [!TIP]
> Use `Scanner` for simplicity during development, but switch to `BufferedReader` or `NIO Files` for production code dealing with high-performance requirements.
