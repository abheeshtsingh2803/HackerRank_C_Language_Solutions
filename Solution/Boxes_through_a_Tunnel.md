# Day #20 - Boxes through a Tunnel
## Problem

You are transporting some boxes through a tunnel, where each box is a [parallelepiped](https://en.wikipedia.org/wiki/Parallelepiped), and is characterized by its length, width and height.

The height of the tunnel **41** feet and the width can be assumed to be infinite. A box can be carried through the tunnel only if its height is strictly less than the tunnel's height. Find the volume of each box that can be successfully transported to the other end of the tunnel. Note: Boxes cannot be rotated.

***Input Format***

The first line contains a single integer **n**, denoting the number of boxes.
**n** lines follow with three integers on each separated by single spaces  **-length<sub>i</sub>**, **width<sub>i</sub>** and **height<sub>i</sub>** which are length, width and height in feet of the **i**-th box.

***Constraints***
+ 1 ≤ n ≤ 100
+ 1 ≤ ≤ **length<sub>i</sub>**, **width<sub>i</sub>**, **heigth<sub>i</sub>**

***Output Format***

For every box from the input which has a height lesser than **41** feet, print its volume in a separate line.

***Sample Input***
```

4
5 5 5
1 2 40
10 5 41
7 2 42

```
***Sample Output***
```

125
80

```
***Explanation***

The first box is really low, only **5** feet tall, so it can pass through the tunnel and its volume is **5 * 5 * 5 = 125**.

The second box is sufficiently low, its volume is **1 * 2 * 40 = 80**.
  
The third box is exactly **41** feet tall, so it cannot pass. The same can be said about the fourth box.

## Solution
```c
#include <stdio.h>
#include <stdlib.h>
#define MAX_HEIGHT 41

struct box
{
	/**
	* Define three fields of type int: length, width and height
	*/
    int length, width, height;
};

typedef struct box box;

int get_volume(struct box box) {
	/**
	* Return the volume of the box
	*/
    return box.height * box.width * box.length;
}

int is_lower_than_max_height(struct box b) {
	/**
	* Return 1 if the box's height is lower than MAX_HEIGHT and 0 otherwise
	*/
    return b.height < 41 ? 1 : 0;
}

int main()
{
	int n;
	scanf("%d", &n);
	box *boxes = malloc(n * sizeof(box));
	for (int i = 0; i < n; i++) {
		scanf("%d%d%d", &boxes[i].length, &boxes[i].width, &boxes[i].height);
	}
	for (int i = 0; i < n; i++) {
		if (is_lower_than_max_height(boxes[i])) {
			printf("%d\n", get_volume(boxes[i]));
		}
	}
	return 0;
}
```
