# Day #22 - Small Triangles, Large Triangles
## Problem

You are given **n** triangles, specifically, their sides **a<sub>i</sub>**, **b<sub>i</sub>** and **c<sub>i</sub>**. Print them in the same style but sorted by their areas from the smallest one to the largest one. It is guaranteed that all the areas are different.

The best way to calculate a area of a triangle with sides **a<sub>i</sub>**, **b<sub>i</sub>** and **b<sub>i</sub>** is Heron's formula:

S = $√p * (p - a) * (p - b) * (p - c)$
 where p = (a + b +c) / 2.

***Input Format***

The first line of each test file contains a single integer **n**. **n** lines follow with three space-separated integers, **a<sub>i</sub>**, **b<sub>i</sub>** and **c<sub>i</sub>**.

***Constraints***
+ 1 ≤ n ≤ 100
+ 1 ≤ **a<sub>i</sub>**, **b<sub>i</sub>**, **c<sub>i</sub>** ≤ 70

***Output Format***

Print exactly **n** lines. On each line print **3** space-separated integers, the **a<sub>i</sub>**, **b<sub>i</sub>** and **c<sub>i</sub>** of the corresponding triangle.

***Sample Input***
```

3
7 24 25
5 12 13
3 4 5

```
***Sample Output***
```

3 4 5
5 12 13
7 24 25

```
***Explanation***

The square of the first triangle is **84**. The square of the second triangle is **30**. The square of the third triangle is **6**. So the sorted order is the reverse one.

## Solution
```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

struct triangle
{
	int a;
	int b;
	int c;
};

typedef struct triangle triangle;

int square(struct triangle t)
{
    int a = t.a, b = t.b, c = t.c;
    return (a + b + c)*(a + b - c)*(a - b + c)*(-a + b + c);
}

void sort_by_area(struct triangle* tr, int n)
{
    for (int i = 0; i < n; i++)
        for (int j = i + 1; j < n; j++)
            if (square(tr[i]) > square(tr[j]))
            {
                struct triangle temp = tr[i];
                tr[i] = tr[j];
                tr[j] = temp;
            }
}

int main()
{
	int n;
	scanf("%d", &n);
	triangle *tr = malloc(n * sizeof(triangle));
	for (int i = 0; i < n; i++) {
		scanf("%d%d%d", &tr[i].a, &tr[i].b, &tr[i].c);
	}
	sort_by_area(tr, n);
	for (int i = 0; i < n; i++) {
		printf("%d %d %d\n", tr[i].a, tr[i].b, tr[i].c);
	}
	return 0;
}
```
