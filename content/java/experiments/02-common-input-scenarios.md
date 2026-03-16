---
title: Common Java Input Scenarios
---

# Common Java Input Scenarios

This guide covers common input handling patterns encountered in competitive programming and technical interviews. Handling input efficiently is the first step toward solving any problem.

## 1. String Input Scenarios

Handling character and string arrays often requires filtering or specific parsing methods.

### A. Input as Character Array (e.g., `['A', 'B', 'C']`)
If the input is given in a formatted string like `['A', 'B', 'C', 'D']`, we can iterate through the characters and extract letters.

```java
public static ArrayList<String> inputArrayFormat() {
    ArrayList<String> arr = new ArrayList<>();
    Scanner scanner = new Scanner(System.in);
    String input = scanner.nextLine();
    for (char c : input.toCharArray()) {
        if (Character.isLetter(c)) {
            arr.add(Character.toString(c));
        }
    }
    return arr;
}
```

### B. Space-Separated Strings
For a single line of space-separated strings (e.g., `A B C D`).

```java
public static ArrayList<String> inputSpaceSeparated() {
    ArrayList<String> arr = new ArrayList<>();
    Scanner scanner = new Scanner(System.in);
    String input = scanner.nextLine();
    Scanner ss = new Scanner(input);
    while (ss.hasNext()) {
        arr.add(ss.next());
    }
    return arr;
}
```

### C. Multi-line Input (Size Not Given)
When you need to read strings until an empty line is entered (common for manual testing).

```java
public static ArrayList<String> inputArraySizeNotGiven() {
    ArrayList<String> arr = new ArrayList<>();
    Scanner scanner = new Scanner(System.in);
    while (true) {
        String element = scanner.nextLine().trim();
        if (element.isEmpty()) {
            break;
        }
        arr.add(element);
    }
    return arr;
}
```

---

## 2. Integer Input Scenarios

Different problems provide numeric data in various formats.

### A. Extracting Digits from Brackets (e.g., `[1,2,3]`)
Similar to the character array method, we can extract digits directly.

```java
public static ArrayList<Integer> inputArrayFormat() {
    ArrayList<Integer> arr = new ArrayList<>();
    Scanner scanner = new Scanner(System.in);
    String input = scanner.nextLine();
    for (char c : input.toCharArray()) {
        if (Character.isDigit(c)) {
            int num = Character.getNumericValue(c);
            arr.add(num);
        }
    }
    return arr;
}
```

### B. Comma-Separated Integers (e.g., `1,2,3,4,5`)
Using a delimiter with `Scanner` makes parsing CSV-style input easy.

```java
public static ArrayList<Integer> inputCommaSeparated() {
    ArrayList<Integer> arr = new ArrayList<>();
    Scanner scanner = new Scanner(System.in);
    String input = scanner.nextLine();
    Scanner ss = new Scanner(input).useDelimiter(",");
    while (ss.hasNextInt()) {
        arr.add(ss.nextInt());
    }
    return arr;
}
```

---

## 3. Standard Coding Patterns

These are the most robust ways to handle competitive programming inputs.

### A. Size Given (The "Classic" Way)
The first line contains `n` (size), and the next line contains `n` elements.

```java
public static void inputWithSize(Scanner sc) {
    int n = sc.nextInt();
    int[] arr = new int[n];

    for (int i = 0; i < n; i++) {
        arr[i] = sc.nextInt();
    }
}
```

### B. Using `split()` and Parsing
Reading a whole line and splitting it by whitespace.

```java
public static void inputAsString(Scanner sc) {
    String line = sc.nextLine();
    String[] tokens = line.split("\\s+");

    List<Integer> arr = new ArrayList<>();
    for (String token : tokens) {
        try {
            arr.add(Integer.parseInt(token.trim()));
        } catch (NumberFormatException e) {
            // Error handling
        }
    }
}
```

### C. Extracting Numbers from Mixed Strings
A robust method that extracts all integers from a string containing mixed characters.

```java
public static void mostUsedInputFormat(Scanner sc) {
    String line = sc.nextLine();
    List<Integer> arr = new ArrayList<>();
    StringBuilder numStr = new StringBuilder();

    for (char ch : line.toCharArray()) {
        if (Character.isDigit(ch)) {
            numStr.append(ch);
        } else if (numStr.length() > 0) {
            arr.add(Integer.parseInt(numStr.toString()));
            numStr.setLength(0);
        }
    }
    if (numStr.length() > 0) arr.add(Integer.parseInt(numStr.toString()));
}
```

---

## 4. End of File (EOF) Input in Java

In many competitive programming platforms (like UVa, LeetCode, or HackerRank), the input size might not be provided. Instead, the program is expected to keep reading until the **End Of File (EOF)** is reached.

### What is EOF?
EOF signals that there is no more data to read from the input stream. In a terminal, this can be triggered by `Ctrl+D` (Linux/Mac) or `Ctrl+Z` (Windows).

### Implementation with `Scanner`
The `hasNext()` method is the standard way to check for EOF.

```java
public static void inputEOFScanner() {
    Scanner sc = new Scanner(System.in);
    List<Integer> arr = new ArrayList<>();
    
    while (sc.hasNextInt()) { // Checks if another integer is available
        arr.add(sc.nextInt());
    }
}
```

### Implementation with `BufferedReader`
For performance-critical tasks, `BufferedReader` is faster. It returns `null` when EOF is reached.

```java
public static void inputEOFBufferedReader() throws IOException {
    BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
    String line;
    
    while ((line = reader.readLine()) != null) { // Checks if line is null (EOF)
        if (line.trim().isEmpty()) break; // Optional: break on empty line
        // Process the line...
    }
}
```

> [!TIP]
> Always prefer `Scanner.hasNext()` for simplicity unless you are hitting execution time limits. For large inputs (10^5+ elements), switch to `BufferedReader` or a Custom Fast I/O class.
