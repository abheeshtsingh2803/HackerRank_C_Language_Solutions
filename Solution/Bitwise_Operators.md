# Day #21 - Bitwise Operators
## Peoblem

In this challenge, you will use logical bitwise operators. All data is stored in its binary representation. The logical operators, and C language, use **1** to represent true and **0** to represent false. The logical operators compare bits in two numbers and return true or false, **0** or **1**, for each bit compared.

+ `Bitwise AND operator &` The output of bitwise AND is 1 if the corresponding bits of two operands is 1. If either bit of an operand is 0, the result of corresponding bit is evaluated to 0. It is denoted by &.

+ `Bitwise OR operator |` The output of bitwise OR is 1 if at least one corresponding bit of two operands is 1. It is denoted by |.

+ `Bitwise XOR (exclusive OR) operator ^` The result of bitwise XOR operator is 1 if the corresponding bits of two operands are opposite. It is denoted by ⊕.

For example, for integers 3 and 5,
```

3 = 00000011 (In Binary)
5 = 00000101 (In Binary)

AND operation        OR operation        XOR operation
  00000011             00000011            00000011
& 00000101           | 00000101          ^ 00000101
  ________             ________            ________
  00000001  = 1        00000111  = 7       00000110  = 6

```
You will be given an integer **n**, and a threshold, **k**. `Foreachnumberifrom1throughn, findthemaximumvalueofthelogicaland, orandxorwhencomparedeagainstallintegrethroughnthataregreaterthan`. `Considervalueonlyifthecomparisonreturnsaresultlessthank$` Print the results of the and, or and exclusive or comparisons on separate lines, in that order.

**Example**<br>
n = 3<br>
k = 3<br>

The results of the comparisons are below:
```

a b   and or xor
1 2   0   3  3
1 3   1   3  2
2 3   2   3  1

```
For the and comparison, the maximum is **2**. For the or comparison, none of the values is less than **k**, so the maximum is **0**. For the xor comparison, the maximum value less than **k** is **2**. The function should print:
```

2
0
2

```
***Function Description***

Complete the calculate_the_maximum function in the editor below.

calculate_the_maximum has the following parameters:

+ int n: the highest number to consider
+ int k: the result of a comparison must be lower than this number to be considered
  
***Prints***

Print the maximum values for the `and`, `or` and `xor` comparisons, each on a separate line.

***Input Format***

The only line contains **2** space-separated integers, **n** and **k**.

***Constraints***

+ 2 ≤ n ≤ $10^3$
+ 0 ≤ k ≤ n

***Sample Input***
```

5 4

```
***Sample Output***
```

2
3
3

```

## Solution
```c
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>
//Complete the following function.


void calculate_the_maximum(int n, int k) {
  //Write your code here.
  int andmax = 0 , ormax = 0 , xormax = 0;
  for(int i = 1 ; i <= n; i++){
      for(int b = i+1 ; b<= n ;b++){
          
          if(((i & b) < k) && andmax < (i & b)){
              andmax = (i&b);
          }
          if(((i | b) < k) && ormax < (i | b)){
              ormax = (i|b);
          }
          if(((i ^ b) < k) && xormax < (i ^ b)){
              xormax = (i ^ b);
          }
      }
  }
  printf("%d\n%d\n%d", andmax, ormax,xormax);
}

int main() {
    int n, k;
  
    scanf("%d %d", &n, &k);
    calculate_the_maximum(n, k);
 
    return 0;
}

```
