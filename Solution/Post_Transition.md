# Day #23 - Post Transition
## Problem

We live in a big country. This country has `towns_count` towns in it. Each town has some post offices in which packages are stored and transferred.

Post offices have different inner structure. Specifically, each of them has some limitations on the packages it can store - their weight should be between `min_weight` and `max_weight` inclusively, where `min_weight` and `max_weight` are fixed for each office.

Packages are stored in some order in the office queue. That means, that they are processed using this order when sending and receiving.

Sometimes two post offices, even in different towns, may organize the following transaction: the first one sends all its packages to the second one. The second one accepts the packages that satisfy the weight condition for the second office and rejects all other ones. These rejected packages return to the first office back and are stored in the same order they were stored before they were sent. The accepted packages move to the tail of the second office's queue in the same order they were stored in the first office.

You should process several queries in your program. You'll be provided with structures `package`, `post_office` and `town`. in order to complete this task, you should fill the following functions:

`print_all_packages` - given the town , print all packages in this town. They should be printed as follows:
```

Town_name:
    0:
        id_0
        id_1
        ...
    1:
        id_2
        id_3
        ...
    ...

```
where **0**, **1** etc are the numbers of post offices and **id<sub>0</sub>**, **id<sub>1</sub>**... are the ids of packages from the **0th** post office in the order of its queue, **id<sub>2</sub>**, **id<sub>3</sub>** are from the **1st** one etc. There should be one '\t' symbol before post office numbers and two '\t' symbols before the ids.

`send_all_accepted_packages` - given the towns **source** and **target** and post office indices **source_office_index** and **target_office_index**, manage the transaction described above between the post office #**source_office_index** in town **source** and the post office #**target_office_index** in town **target**.

`town_with_most_packages` - given all towns, find the one with the most number of packages in all post offices altogether. If there are several of them, find the first one from the collection **towns**.

`find_town` - given all towns and a string **name**, find the town with the name **name**. It's guaranteed that the town exists.

***Input Format***

First line of the input contains a single integer **towns_count**. **towns_count** blocks follow, each describing a town.

Every town block contains several lines. On the first line there is a string **town_count** - the name of the town. On the second line there is an integer **offices_count** - the number of the offices in the town. **offices_count** blocks follow then, each describing an office.

Every office block also contains several lines. On the first line there are three integers separated by single spaces: **packafges_count**(the number of packages in the office), **min_weight** and  **max_weight**(described above). **packages_count** blocks follow, each describing a package.

Every package block contains exactly two lines. On the first line there is a string **id** which is an id of the package. On the second line there is an integer **weight** which is the weight of the package.

Then, there is a single integer **queries** on the line which is the number of queries. **queries** blocks follow, each describing a query.

Every query block contains several lines. On the first line there is an integer **1**, **2** or **3**. If this integer is **1**, on the second line there is a string  **town_name** - the name of town for which all packages should be printed. If this integer is **2**, on the second line there are string **source_name**, integer **source_office_index**, string **target_name** and integer **target_office_index** separated by single spaces. That means transactions between post office #**source_office_index** in the town **source_name** and post office #**target_office_index** in the town **target_name** should be processed.

If the integer is **3**, no lines follow and the town with the most number of packages should be found.

Constraints

+ All integer are between **0** and **10**.
+ `towns_count` > 0, `offices_count` > 0.
+ All strings have length ≤ **5**.
+ All towns' names have only uppercase english letters and are unique.
+ All packages' ids have only lowercase english letters and are unique.
+ For each post office, `max_weight` ≤ `min_weight` .
+ All queries are valid, that means, towns with the given names always exist, post offices with the given indices always exist.
Output Format

For queries of type **1**, print all packages in the format provided in the problem statement. For queries of type **3**, print "Town with the most number of packages is **town_name**" on a separate line.

***Sample Input***
```

2
A
2
2 1 3
a 2
b 3
1 2 4
c 2
B
1
4 1 4
d 1
e 2
f 3
h 4
5
3
2 B 0 A 1
3
1 A
1 B

```
***Sample Output***
```

Town with the most number of packages is B
Town with the most number of packages is A
A:
    0:
        a
        b
    1:
        c
        e
        f
        h
B:
    0:
        d

```
***Explanation***

Before all queries, town B has **4** packages in total, town **A** has **3**. But after transaction all packages from B's 0th post office go to the 1st post office of A, except package d because it's too light.

## Solution
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h> //Added
#define MAX_STRING_LENGTH 6

struct package
{
    char* id;
    int weight;
    struct package* next;   //Added
};
typedef struct package package;

struct post_office
{
    int min_weight;
    int max_weight;
    package* packagesHead;  //Added
    package* packagesTail;  //Added
    //package* packages;    //Removed
    int packages_count;
};
typedef struct post_office post_office;

struct town
{
    char* name;
    post_office* offices;
    int offices_count;
};
typedef struct town town;

void func_InsertPackages(post_office* office){  //Added
    if(office->packagesHead==NULL){
        office->packagesHead = (package*)malloc(sizeof(package));
        office->packagesTail = office->packagesHead;
    }
    else{
        office->packagesTail->next = (package*)malloc(sizeof(package));
        office->packagesTail = office->packagesTail->next;
    }
    office->packagesTail->next = NULL;
    office->packagesTail->id = malloc(sizeof(char) * MAX_STRING_LENGTH);
    scanf("%s", office->packagesTail->id);
    scanf("%d", &office->packagesTail->weight);
    return;
}

void func_AppendPackage(package** Head, package** Tail, package* node){ //Added
    if((*Head)==NULL){
        *Head = node;
        *Tail = node;
    }
    else{
        (*Tail)->next = node;
        *Tail = node;
    }
}

void func_RemovePackage(package** Head,package** Tail,package* prev,package* current){  //Added
    if((*Head)==prev && prev==current)
        *Head = current->next;
    else if((*Tail)==current)
        (*Tail) = prev;
    prev->next=current->next;
}

void print_all_packages(town t) {
    printf("%s:\n",t.name);
    for(int i=0; i<t.offices_count; i++){
        printf("\t%d:\n",i);
        package* node = t.offices[i].packagesHead;
        while(node!=NULL){
            printf("\t\t%s\n",node->id);
            node = node->next;
        }
    }
}

void send_all_acceptable_packages(town* source, int source_office_index, town* target, int target_office_index) {
    post_office* sourceOffice = source->offices+source_office_index;
    post_office* targetOffice = target->offices+target_office_index;

    package* travelS = sourceOffice->packagesHead;
    package* tempPreviousS = travelS;
    package* tempDeleteS = travelS;
    package** sourceHead = &sourceOffice->packagesHead;
    package** sourceTail = &sourceOffice->packagesTail;
    package** targetHead = &targetOffice->packagesHead;
    package** targetTail = &targetOffice->packagesTail;

    while(travelS!=NULL){
        if((travelS->weight>=targetOffice->min_weight) && (travelS->weight<=targetOffice->max_weight)){
            func_AppendPackage(targetHead,targetTail,travelS);
            func_RemovePackage(sourceHead,sourceTail,tempPreviousS,travelS);
            travelS = travelS->next;
            (*targetTail)->next = NULL;
            targetOffice->packages_count++;
            sourceOffice->packages_count--;
        }
        else{
            tempPreviousS = travelS;
            travelS = travelS->next;
        }
    }
}

town town_with_most_packages(town* towns, int towns_count) {
    int twmpi = 0; //Town With Most Packages Index
    int mpnt = 0; //Maximum Packages Number of a Twon
    int pnt = 0; //Packages Number of a Town
    for(int i=0; i<towns_count; i++,pnt=0){
        for(int j=0; j<towns[i].offices_count; j++)
            pnt+= towns[i].offices[j].packages_count;
        if(pnt>mpnt){
            mpnt = pnt;
            twmpi = i;
        }
    }
    return towns[twmpi];
}

town* find_town(town* towns, int towns_count, char* name) {
    int townIndex = 0;
    for(int i=0; i<towns_count; i++)
        if(strcmp(towns[i].name,name)==0){
            townIndex = i;
            break;
        }
    return &towns[townIndex];
}

int main()
{
    int towns_count;
    scanf("%d", &towns_count);
    town* towns = malloc(sizeof(town)*towns_count);
    for (int i = 0; i < towns_count; i++) {
        towns[i].name = malloc(sizeof(char) * MAX_STRING_LENGTH);
        scanf("%s", towns[i].name);
        scanf("%d", &towns[i].offices_count);
        towns[i].offices = calloc(towns[i].offices_count,sizeof(post_office));
        for (int j = 0; j < towns[i].offices_count; j++) {
            scanf("%d%d%d", &towns[i].offices[j].packages_count, &towns[i].offices[j].min_weight, &towns[i].offices[j].max_weight);
            //towns[i].offices[j].packages = malloc(sizeof(package)*towns[i].offices[j].packages_count);    //Removed
            for (int k = 0; k < towns[i].offices[j].packages_count; k++) {
                func_InsertPackages(&towns[i].offices[j]);  //Added
                /*towns[i].offices[j].packages[k].id = malloc(sizeof(char) * MAX_STRING_LENGTH);
                scanf("%s", towns[i].offices[j].packages[k].id);
                scanf("%d", &towns[i].offices[j].packages[k].weight);*/ //Removed
            }
        }
    }

    int queries;
    scanf("%d", &queries);
    char town_name[MAX_STRING_LENGTH];
    while (queries--) {
        int type;
        scanf("%d", &type);
        switch (type) {
        case 1:
            scanf("%s", town_name);
            town* t = find_town(towns, towns_count, town_name);
            print_all_packages(*t);
            break;
        case 2:
            scanf("%s", town_name);
            town* source = find_town(towns, towns_count, town_name);
            int source_index;
            scanf("%d", &source_index);
            scanf("%s", town_name);
            town* target = find_town(towns, towns_count, town_name);
            int target_index;
            scanf("%d", &target_index);
            send_all_acceptable_packages(source, source_index, target, target_index);
            break;
        case 3:
            printf("Town with the most number of packages is %s\n", town_with_most_packages(towns, towns_count).name);
            break;
        }
    }
    return 0;
}

```
