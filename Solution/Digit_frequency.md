# Day #10 - Digit Frequency
## Problem

Given a string,**s** , consisting of alphabets and digits, find the frequency of each digit in the given string.

***Input Format***

The first line contains a string, **num** which is the given number.

***Constraints***

**1 ≤ len(num) ≤ 1000**

All the elements of num are made of english alphabets and digits.

***Output Format***

Print ten space-separated integers in a single line denoting the frequency of each digit **0** from  to **9** .

***Sample Input 0***
```

a11472o5t6

```
***Sample Output 0***
```

0 2 1 0 1 1 1 1 0 0 

```
***Explanation 0***

In the given string:

+ **1** occurs two times.
+ **2, 4, 5, 6 and 7** occur one time each.
+ The remaining digits **0, 3, 8 and 9** don't occur at all.

***Sample Input 1***
```

lw4n88j12n1

```
***Sample Output 1***
```

0 2 1 0 1 0 0 0 2 0

```
***Sample Input 2***
```

1v88886l256338ar0ekk

```
***Sample Output 2***
```

1 1 1 2 0 1 2 0 5 0

```

## Solution
```C
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

int main() {

    /* Enter your code here. Read input from STDIN. Print output to STDOUT */ 
    char s[1000];
    int count0=0, count1=0, count2=0, count3=0, count4=0, count5=0, count6=0, count7=0, count8=0, count9=0;
    scanf("%s", &s);
    for(int i=0; s[i] != '\0'; i++){
        if(s[i] == '0'){
            count0++;
        }
        if(s[i] == '1'){
            count1++;
        }
        if(s[i] == '2'){
            count2++;
        }
        if(s[i] == '3'){
            count3++;
        }
        if(s[i] == '4'){
            count4++;
        }
        if(s[i] == '5'){
            count5++;
        }
        if(s[i] == '6'){
            count6++;
        }
        if(s[i] == '7'){
            count7++;
        }
        if(s[i] == '8'){
            count8++;
        }
        if(s[i] == '9'){
            count9++;
        }
    }
    printf("%d ", count0);
    printf("%d ", count1);
    printf("%d ", count2);
    printf("%d ", count3);
    printf("%d ", count4);
    printf("%d ", count5);
    printf("%d ", count6);
    printf("%d ", count7);
    printf("%d ", count8);
    printf("%d ", count9);
    return 0;
}
```
