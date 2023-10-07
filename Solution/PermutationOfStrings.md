# Day #17 - Permi=utation Of Strings
## Problem

Strings are usually ordered in lexicographical order. That means they are ordered by comparing their leftmost different characters. For example, **abc < abd** because **c < d**. Also **z > yy** because **z < y**. If one string is an exact prefix of the other it is lexicographically smaller, e.g., **gh > ghij**.

Given an array of strings sorted in lexicographical order, print all of its permutations in strict lexicographical order. If two permutations look the same, only print one of them. See the 'note' below for an example.

Complete the function next_permutation which generates the permutations in the described order.

For example, **s = [ab, bc, cd]**. The six permutations in correct order are:
```

ab bc cd
ab cd bc
bc ab cd
bc cd ab
cd ab bc
cd bc ab

```
Note: There may be two or more of the same string as elements of **s**.
For example, **s = [ab, bc, cd]**. Only one instance of a permutation where all elements match should be printed. In other words, if **s[0] == s[1]**, then print either **s[0] s[1]** or **s[1] s[0]** but not both.

A three element array having three distinct elements has six permutations as shown above. In this case, there are three matching pairs of permutations where **s[0] = ab** and **s[1] = ab** are switched. We only print the three visibly unique permutations:
```

ab ab bc
ab bc ab
bc ab ab

```
***Input Format***

The first line of each test file contains a single integer **n**, the length of the string array **s**.

Each of the next  lines contains a string .

***Constraints***
+ 2 ≤ n ≤ 9
+ 1 ≤ |s[i]| ≤ 10
+ **s[i]** contains only lowercase English letters.
  
***Output Format***

Print each permutation as a list of space-separated strings on a single line.

***Sample Input***
```

2
ab
cd

```
***Sample Output***
```

ab cd
cd ab

```
***Sample Input***
```

3
a
bc
bc

```
***Sample Output***
```

a bc bc
bc a bc
bc bc a

```
***Explanation***

This is similar to the note above. Only three of the six permutations are printed to avoid redundancy in output.

## Solution
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void swap(char **s, int i, int j)
{
    char * tmp = s[i];
    s[i] = s[j];
    s[j] = tmp;
}

void reverse(char **s, int start, int end)
{
    while(start<end)
    {
        swap(s,start++,end--);
    }
}

int next_permutation(int n, char **s)
{
	for(int i=n-2;i>-1;i--)
    {
        if(strcmp(s[i+1],s[i])>0)
        {
            //get min max
            for(int j=n-1;j>i;j--)
            {
                if(strcmp(s[j],s[i])>0)
                {
                    //do swap
                    swap(s,i,j);
                    // do reverse
                    reverse(s,i+1,n-1);
                    return 1;
                }
            }
        }
    }
    return 0;
}

int main()
{
	char **s;
	int n;
	scanf("%d", &n);
	s = calloc(n, sizeof(char*));
	for (int i = 0; i < n; i++)
	{
		s[i] = calloc(11, sizeof(char));
		scanf("%s", s[i]);
	}
	do
	{
		for (int i = 0; i < n; i++)
			printf("%s%c", s[i], i == n - 1 ? '\n' : ' ');
	} while (next_permutation(n, s));
	for (int i = 0; i < n; i++)
		free(s[i]);
	free(s);
	return 0;
}
```
