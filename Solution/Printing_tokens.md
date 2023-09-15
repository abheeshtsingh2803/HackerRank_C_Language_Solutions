# Day #08 - Printing Tokens
## Problem

Given a sentence, **s** , print each word of the sentence in a new line.

***Input Format***

The first and only line contains a sentence, **s**.

***Constraints***

**1 ≤ len(s) ≤ 1000**

***Output Format***

Print each word of the sentence in a new line.

***Sample Input 0***
```

This is C

```
***Sample Output 0***
```

This
is
C

```
****Explanation 0***

In the given string, there are three words ["This", "is", "C"]. We have to print each of these words in a new line.

***Sample Input 1***
```

Learning C is fun

```
***Sample Output 1***
```

Learning
C
is
fun

```
***Sample Input 2***
```

How is that

```
***Sample Output 2***
```

How
is
that

```

## Solution
```C
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

int main() {

    char *s;
    s = malloc(1024 * sizeof(char));
    scanf("%[^\n]", s);
    s = realloc(s, strlen(s) + 1);
    //Write your logic to print the tokens of the sentence here.
    for (char *c = s; *c != NULL; c++) {
      if (*c == ' ') {
          *c = '\n';
      }
    }
    printf("%s", s);
    return 0;
}
```
