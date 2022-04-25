# lab02



## C pointers and structs

### pointer

a pointer is a variable that contains address of another variable.

One common situatio is that any byte can ba a `char`,  on byte is a `char`, a pair of one-byte cells can be treated as `short` integer, and four adjacent bytes form a `long`. A pointer is a group of cells(often two or four) that can hold an address.

`&` gives address of a object

`*` dereferencing object, in a word, give the value that this address contains.



##### pointer types

In addition, the type `void *`(point to `void`) replaces `char *`as the proper type for a generic pointer.

- `malloc`

  Syntax of malloc()

  ```c
  ptr = (castType* )malloc(size);
  ```

  The `malloc()` function reserves **a block of memory** of the specified number of bytes. And, it returns a pointer of `void` which can be casted into pointers of any form. And the expression return a `NULL` pointer if the memory cannot be allocated.

- `calloc`

  Syntax of calloc()

  ```c
  ptr = (castType*)calloc(n, size);
  ```

  The `malloc()` function allocates memory and leaves the memory uninitialized, whereas the `calloc()` function allocates memory and initializes all bits to **zero**.

- `realloc()`

  If the dynamically allocated memory is insufficient or more than required, you can change the size of previously allocated memory using the `realloc()` function.

  Syntax of calloc()

  ```c
  pre = realloc(ptr, x);
  ```

  

- `free()`

  Dynamically allocated memory created with either `calloc()` or `malloc()` doesn't get freed on their own. You must explicitly use `free()` to release the space.

  ```c
  free(ptr)
  ```

```c
// Program to calculate the sum of n numbers entered by the user

#include <stdio.h>
#include <stdlib.h>

int main() {
  int n, i, *ptr, sum = 0;

  printf("Enter number of elements: ");
  scanf("%d", &n);

  ptr = (int*) malloc(n * sizeof(int));
 
  // if memory cannot be allocated
  if(ptr == NULL) {
    printf("Error! memory not allocated.");
    exit(0);
  }

  printf("Enter elements: ");
  for(i = 0; i < n; ++i) {
    scanf("%d", ptr + i);
    sum += *(ptr + i);
  }

  printf("Sum = %d", sum);
  
  // deallocating the memory
  free(ptr);

  return 0;
}
```

```c
// Program to calculate the sum of n numbers entered by the user

#include <stdio.h>
#include <stdlib.h>

int main() {
  int n, i, *ptr, sum = 0;
  printf("Enter number of elements: ");
  scanf("%d", &n);

  ptr = (int*) calloc(n, sizeof(int));
  if(ptr == NULL) {
    printf("Error! memory not allocated.");
    exit(0);
  }

  printf("Enter elements: ");
  for(i = 0; i < n; ++i) {
    scanf("%d", ptr + i);
    sum += *(ptr + i);
  }

  printf("Sum = %d", sum);
  free(ptr);
  return 0;
}
```

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
  int *ptr, i , n1, n2;
  printf("Enter size: ");
  scanf("%d", &n1);

  ptr = (int*) malloc(n1 * sizeof(int));

  printf("Addresses of previously allocated memory:\n");
  for(i = 0; i < n1; ++i)
    printf("%pc\n",ptr + i);

  printf("\nEnter the new size: ");
  scanf("%d", &n2);

  // rellocating the memory
  ptr = realloc(ptr, n2 * sizeof(int));

  printf("Addresses of newly allocated memory:\n");
  for(i = 0; i < n2; ++i)
    printf("%pc\n", ptr + i);
  
  free(ptr);

  return 0;
}
```



##### pointer arithmetic

###### plus

if `p` and `q` are in the same array. `pa + 1`means points to the next object, `pa + i`means points to the i-th object beyond `pa`.  So `pa[i] == *(pa + i)`in a C array.

###### subtract

if `p` and `q` point to elements of the same array, and `p < q`, then `q-p+1` is the number of elemnets from `p` to `q` inclusive. 

###### pointer & pointer

If and only if there in the same array.

###### pointer & integer

This is related with pointer and array, especially.

##### pointer comparasion

Pointers and integers are not interchangeable. Zero is the sole exception. But we use constant NULL often in place of zero, as a mnemonic to indicate more clearly that this is a special value for a pointer. 

Pointers are comparable(`==,!=,<=,>=`) under certain circumstances:

- if `p` and `q` point to members of the **same** array
- Any pointer can be maeninggully compared for quality or inequality with zero(NULL)

### struct

**struct is a ==collection== of variables under a single name**,就是说`struct`就是一组变量的集合,在内存里就是表现为将一组变量按照声明的顺序放在一段内存空间中。

```c
struct Student{
  int id1;
  int id2;
  char a;
  char b;
  float percentage;
}
```

<img src="lab02.assets/structure-members-are-stored-in-memory.png" alt="C structure members storage in memory" style="zoom:150%;" />

如上的这个结构体定义对应的虚拟内存中的放置就是上图这样的，图中有两个empty的位置，是因为要满足内存对齐的要求。

`struct`本质上是增加了一个人为定义的新的数据类型，比如这里就是多了一个`Student`数据类型的定义(你把它叫做复合数据类型也行)。在如上图所示定义完这个结构体后，我们就得到了一个新的数据类型(a derived type)`struct Student`，接下来就可以进行实例化这个数据结构，并操作它做些事情。

创建结构体变量的方式：

- ```c
  /* 用到时再进行创建，但这种方式要加上struct关键字
  *  因为我们定义的类型是“struct Student”不是“Student”这点要记住
  */
  int main(){
    struct Student person1; 
    struct Student person2;
    struct Student p[20];
   	return 0;
  }
  ```

- ```c
  /*这种方式就不容易漏到struct关键字，相当于是声明和定义一起做掉了，
  * 告诉使用者，现在person1，person2这些的数据类型就是Student
  */
  struct Student{
  	// define code
  }person1, person2, p[20];
  ```

In both cases,

- person1 and person2 are `struct Student` variables
- p[] is a `struct Student` array of size 20.



#### keyword `typedef`

"We use the `typedef` keyword to create an alias name for data types. It is commonly used with structures to simplify the syntax of declaring variables." 我们用`typedef`来创建一个**别名**，使得定义变得更简单，而这样是更为推荐的方法。

e.g.

```c
struct Distance{
  int feet;
  float inch;
};

int main() {
  struct Distance d1, d2;
}
```

Using typedef

```c
typedef struct Distance {
  int feet;
  float inch;
} distances;

int main() {
  distances d1, d2;
}
```

我们再来看一个例子应该就完全明白了，这些例子都是完整运行成功，可放心测验。

```c
#include <stdio.h>
struct Person{
    int id;
    char* name;
    float number;
};
typedef struct Person person;

typedef struct Distance{
    int feet;
    float inch;
}distances;


int main() {
    person p1;
    person p2;
    person p[20];
    p1.id = 10;
    printf("p1's id is %d", p1.id);

    distances d1;
    distances d2;
    d1.feet = 1314;
    printf("d1's feet is %d", d1.feet);
    return 0;
}
```

你会发现，整合了`typedef`后的语句就是把`struct Distance`这个数据类型用一个别名`distances`来表示了。于是以后所有`distances`就是`struct Distance`，这就好比你在`zsh`里用到`alias`命令，比如我们输入`alias 'll=ls -alh'`那以后我们输入的所有`ll`所执行的就一定是`ls -alh`这个命令，只不过我们觉得这条太长了就用一条更简单的来替代了。此处`typedef`的作用也正是如此，就是一个**别名**。



### C pointers to struct

```c
struct name {
    member1;
    member2;
    .
    .
};

int main()
{
    struct name *ptr, Harry;
}
```

`ptr` is a pointer to `struct`

To access members of a structure using pointers, we use the `->` operator.

e.g. list node as example

```c
#include <stdio.h>
#include <stdlib.h>
typedef struct node{
    int item;
    struct node* prev;
    struct node* next;
}node;

int main() {
    node* newNode = (node *) malloc(sizeof(node *));

    printf("node size:\t %lu\n", sizeof(newNode));
    printf("node->item size:\t %lu\n", sizeof(newNode->item));
    printf("node pointer's size:\t %lu\n", sizeof(newNode->next));
    printf("node address is:\t %lu\n", newNode);
    printf("value of node before initialized is:\t %d\n", newNode->item);
    newNode->item = 10;
    printf("node address is:\t %lu\n", newNode);
    printf("value of node after defined is:\t %d\n", newNode->item);
    node* secondNode = (node *)malloc(sizeof(node *));
    secondNode->item = 11;
    newNode->next = secondNode;
    printf("node pointer's size:\t %lu\n", sizeof(newNode->next));
    return 0;
}
```

<img src="lab02.assets/image-20220425221742177.png" alt="image-20220425221742177" style="zoom: 100%;" />

![IMG_1519](lab02.assets/IMG_1519.jpg)



<img src="lab02.assets/Screen%20Shot%202022-04-25%20at%2022.27.36.png" alt="Screen Shot 2022-04-25 at 22.27.36" style="zoom:150%;" />
