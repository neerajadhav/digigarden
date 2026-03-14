---
title: 06 - Encode and Decode Strings
tags:
  - String
  - ArrayList
  - Parsing
  - Medium
---

Design an algorithm to encode a list of strings into a single string so it can be transmitted over a network and then decoded back into the original list of strings.

Machine 1 encodes the list:

```

string encode(vector strs)

```

Machine 2 decodes the encoded string:

```

vector decode(string s)

```

The decoded list must be exactly the same as the original list.

## Examples

Example 1

Input
```

["Hello","World"]

```

Output
```

["Hello","World"]

```

Example 2

Input
```

[""]

```

Output
```

[""]

````

## Solution

```java
import java.util.*;

class Solution {

    public String encode(List<String> strs) {

        StringBuilder encoded = new StringBuilder();

        for (String s : strs) {
            encoded.append(s.length()).append("#").append(s);
        }

        return encoded.toString();
    }

    public List<String> decode(String str) {

        List<String> result = new ArrayList<>();
        int i = 0;

        while (i < str.length()) {

            int j = i;

            while (str.charAt(j) != '#') {
                j++;
            }

            int length = Integer.parseInt(str.substring(i, j));

            String word = str.substring(j + 1, j + 1 + length);

            result.add(word);

            i = j + 1 + length;
        }

        return result;
    }
}
````

## Intuition

The main challenge is handling strings that may contain **any characters**, including separators.

Instead of relying on a delimiter alone, we encode each string as:

```
length + "#" + string
```

Example

```
["Hello","World"]
```

Encoded string

```
5#Hello5#World
```

This works because we always know exactly how many characters to read after the `#`.

Decoding process:

1. Read characters until `#` to determine the length.
    
2. Convert that length to an integer.
    
3. Extract the next `length` characters.
    
4. Add the extracted string to the result list.
    
5. Continue until the encoded string is fully processed.
    

## Java Components Used

### StringBuilder

Used to efficiently build the encoded string.

Example

```
encoded.append(s.length()).append("#").append(s);
```

Why used:

- Strings are immutable in Java.
    
- `StringBuilder` allows efficient string concatenation.
    

### ArrayList

Used to store the decoded list of strings.

Example

```
List<String> result = new ArrayList<>();
```

Why used:

- Dynamic size
    
- Efficient insertion
    

### Integer.parseInt()

Used to convert the substring length into an integer.

Example

```
int length = Integer.parseInt(str.substring(i, j));
```

## Complexity Analysis

Let

```
n = number of strings
m = total characters across all strings
```

Time Complexity

```
O(m)
```

Each character is processed once during encoding and decoding.

Space Complexity

```
O(m)
```

Space required to store encoded string and decoded result.

## Key Takeaways

- Use **length-prefix encoding** to handle arbitrary characters.
    
- Format used: `length#string`.
    
- Avoids delimiter collision problems.
    
- Works for any possible ASCII characters.