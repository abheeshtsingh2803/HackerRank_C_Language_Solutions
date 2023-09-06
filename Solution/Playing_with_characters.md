# Day #02 - Playing with Characters
## Problem

<br>

This challenge will help you to learn how to take a character, a string and a sentence as input in C.

To take a single character _ch_  as input, you can use `scanf("%c", &ch )`; and `printf("%c", ch)` writes a character specified by the argument char to stdout

```

char ch;
scanf("%c", &ch);
printf("%c", ch);

```
This piece of code prints the character _ch_ .

You can take a string as input in C using `scanf(“%s”, s)`. But, it accepts string only until it finds the first space.

In order to take a line as input, you can use `scanf("%[^\n]%*c", s)`; where  is defined as `char s[MAX_LEN]` where  is the maximum size of _s_ . Here, `[]` is the scanset character. `^\n` stands for taking input until a newline isn't encountered. Then, with this `%*c`, it reads the newline character and here, the used * indicates that this newline character is discarded.

Note: The statement: `scanf("%[^\n]%*c", s)`; will not work because the last statement will read a newline character, \n, from the previous line. This can be handled in a variety of ways. One way is to use `scanf("\n")`; before the last statement.

***Task***

You have to print the character,_ch_ , in the first line. Then print _s_ in next line. In the last line print the sentence,_s_ .

***Input Format***

First, take a character, _ch_  as input.
Then take the string, _s_ as input.
Lastly, take the sentence  as input.

***Constraints***

Strings for _s_ and _sen_ will have fewer than 100 characters, including the newline.

***Output Format***

Print three lines of output. The first line prints the character,_ch_ .
The second line prints the string, _s_ .
The third line prints the sentence, _sen_ .

***Sample Input***

```

C
Language
Welcome To C!!

```

***Sample Output***

```

C
Language
Welcome To C!!

```

## Solution

```

#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

int main() 
{
    char ch, s[10], str[100];        
    scanf("%c", &ch);             
    scanf("%s", s);  
    fgets(str, 100,  stdin);
    scanf("%[^\n]%*c", str);
    printf("%c\n", ch);                 
    printf("%s\n", s);            
    printf("%s", str); 
    return 0;                               
}

```
