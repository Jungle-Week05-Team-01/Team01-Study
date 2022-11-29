## C의 메모리 관리

- 헤더파일로 `<stdlib.h>` 나 `<malloc.h>` 를 포함시켜야한다.
- 메모리 할당 함수 : `malloc`
- 메모리 할당 및 초기화 : `calloc`
- 메모리 추가 할당 : `realloc`
- 메모리 해제 함수 : `free`
- 동적할당 : 프로그램 실행 중에 동적으로 메모리를 할당하는 것


![image](https://user-images.githubusercontent.com/67156494/204577744-eb636d70-62b0-4f86-bed5-df75b102c25c.png)







- 동적으로 메모리를 할당할 때는 ‘Heap’영역에 할당한다.
  - 정적 메모리 할당 방법(static) : 데이터를 저장할 때 변수를 선언하고 사용하는 방식
  - 프로그램이 시작하기 전에 사용할 메모리공간을 정해놓고 시작



<br>



- 정적 메모리 할당의 문제점
  - 메모리의 크기에 따라 메모리가 낭비되거나 필요에 따른 메모리 공간이 부족해 프로그램을 다시 짜야하는 경우가 발생


<br>


<br>


## malloc

- `void* malloc(size_t size);`
  - malloc이 `void*` 를 리턴하는 이유
    - 동적으로 메모리를 할당할 때 정수를 저장할 지, 배열을 저장할 지, 구조체를 저장할 지 실질적으로 모르기 때문에 무한한 자료형에 맞춰진 malloc함수를 모두 만들어 놓을 수가 없다.
  - 메모리 할당에 실패하면 `null` 을 반환한다.


![image](https://user-images.githubusercontent.com/67156494/204578381-2f1fac0d-a6ce-4ba8-81d5-cb903f8a8ef9.png)


<br>


```c
int* pi = (int* p) malloc(sizeof(int) * 10);
char* pc = (char*) malloc(sizeof(char) * 1));

typedef struct book
{
	char name[10];
	int isbn;
	int price;
	int year;
	struct book* pb;
}BOOK;

BOOK* p = (BOOK*) malloc (sizeof(BOOK) * 30;
// BOOK 구조체 30개를 저장할 수 있는 메모리 공간 할당
```

- malloc param : 메모리 공간
- return type : 좌측의 포인터 타입과 malloc 함수가 리턴하는 포인터의 타입은 동일해야한다.



<br>

<br>


## calloc


- `void* calloc(size_t num, size_t size);`
  - 메모리를 모두 0으로 초기화 한다.
  - 만약 성공적으로 메모리가 할당된다면, 메모리 블럭을 맨 앞 주소를 반환한다.



<br>



- malloc 🆚 calloc

  - malloc은 할당된 메모리를 초기화해주지 않기 떄문에 dummy값이 들어있다.
  - calloc은 할당된 메모리를 모두 0으로 초기화한다.

  

<br>



- ex_malloc_and_calloc.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(void)
{
    
    int *p1 = malloc(4*sizeof(int));  // allocates enough for an array of 4 int
    int *p2 = malloc(sizeof(int[4])); // same, naming the type directly
    int *p3 = malloc(4*sizeof *p3);   // same, without repeating the type name
 
    if(p1) {
        for(int n=0; n<4; ++n) // populate the array
            p1[n] = n*n;
        for(int n=0; n<4; ++n) // print it back out
            printf("p1[%d] == %d\\n", n, p1[n]);
    }
 
    
    
    int *p4 = calloc(4, sizeof(int));    // allocate and zero out an array of 4 int
    int *p5 = calloc(1, sizeof(int[4])); // same, naming the array type directly
    int *p6 = calloc(4, sizeof *p3);     // same, without repeating the type name
 
    
    if(p2) {
        for(int n=0; n<4; ++n) // print the array
            printf("p2[%d] == %d\\n", n, p2[n]);
    }
 
    free(p1);
    free(p2);
    free(p3);
    free(p4);
    free(p5);
    free(p6);
}
```



<br>



- output

```bash
>> p1[0] == 0
>> p1[1] == 1
>> p1[2] == 4
>> p1[3] == 9
>> p2[0] == 0
>> p2[1] == 0
>> p2[2] == 0
>> p2[3] == 0
Program ended with exit code: 0
```



<br>

<br>

## realloc



- `void* realloc(void* ptr, size_t new_size);`
  - 이미 할당받은 메모리에 추가로 메모리를 할당한다.
  - 이전 메모리 주소는 사라진다.



<br>



- ex_realloc.c

```c
#include <assert.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void print_storage_info(const int* next, const int* prev, int ints) {
    if (next) {
        printf("%s location: %p. Size: %d ints (%ld bytes).\\n",
               (next != prev ? "New" : "Old"), (void*)next, ints, ints * sizeof(int));
    } else {
        printf("Allocation failed.\\n");
    }
}
 
int main(void)
{
    const int pattern[] = {1, 2, 3, 4, 5, 6, 7, 8};
    const int pattern_size = sizeof pattern / sizeof(int);
    int *next = NULL, *prev = NULL;
 
    if ((next = (int*)malloc(pattern_size * sizeof *next))) { // allocates an array
        memcpy(next, pattern, sizeof pattern); // fills the array
        print_storage_info(next, prev, pattern_size);
    } else {
        return EXIT_FAILURE;
    }
 
    // Reallocate in cycle using the following values as a new storage size.
    const int realloc_size[] = {10, 12, 512, 32768, 65536, 32768};
 
    for (int i = 0; i != sizeof realloc_size / sizeof(int); ++i) {
        if ((next = (int*)realloc(prev = next, realloc_size[i] * sizeof(int)))) {
            print_storage_info(next, prev, realloc_size[i]);
            assert(!memcmp(next, pattern, sizeof pattern));  // is pattern held
        } else {  // if realloc failed, the original pointer needs to be freed
            free(prev);
            return EXIT_FAILURE;
        }
    }
 
    free(next); // finally, frees the storage
    return EXIT_SUCCESS;
}
```



<br>



- output

```bash
>>> New location: 0x600000202e80. Size: 8 ints (32 bytes).
>>> New location: 0x600000c00990. Size: 10 ints (40 bytes).
>>> Old location: 0x600000c00990. Size: 12 ints (48 bytes).
>>> New location: 0x10080b400. Size: 512 ints (2048 bytes).
>>> New location: 0x108008000. Size: 32768 ints (131072 bytes).
>>> Old location: 0x108008000. Size: 65536 ints (262144 bytes).
>>> Old location: 0x108008000. Size: 32768 ints (131072 bytes).
Program ended with exit code: 0
```

<br>


<br>

## free


- `void free(void* ptr);`
  - 할당한 메모리를 해제한다.
  - 할당한 메모리를 제대로 해제해주지 않으면, 메모리 누수가 발생할 수 있다.



<br>



💡 메모리 누수 잡는 방법

- `gcc -g3 -fsanitize=address` 로 컴파일
- `valgrind` :  메모리 누수 검사 프로그램


<br>


💡 `EXIT_SUCCESS` , `EXIT_FAILURE`

: **`EXIT_SUCCESS`** 및 **`EXIT_FAILURE`** 상수는 및 `_exit` 함수에 `exit` 대한 인수이며 및 함수의 `atexit_onexit` 반환 값

- Ref. https://learn.microsoft.com/ko-kr/cpp/c-runtime-library/exit-success-exit-failure?view=msvc-170


<br>


<br>



- thread-safe
  - 멀티 스레드 프로그래밍 환경에서 여러 스레드로부터 동시에 접근이 이루어져도 프로그램의 실행에 문제가 없음을 뜻한다.






<br>



## malloc 선언 시, 타입 캐스팅

- Bad Usage : `int *sieve = (int *) malloc(sizeof(int) * length);`
- Good Usage :  `int *sieve = malloc(sizeof *sieve * length);`



<br>



- 타입 캐스팅은 안좋은 프로그래밍 관습의 일례이다.
- 특히 포인터 캐스트의 경우 더더욱 안좋다.
- 타입 캐스팅을 할 경우, `int` 오류를 일으킬 수 있는 잠재적인 위험이 많다.
- `void*` 의 경우, 자동으로 다른 포인터 유형으로 안전하게 변환된다.
  - 암시적 형 변환이 자동으로 일어난다.
  - 명시적 형 변환은 위험하다.
- Ref. https://stackoverflow.com/questions/605845/do-i-cast-the-result-of-malloc



<br>

<br>

<br>



------

## 🔗 Reference

- **[C언어] 동적할당 정리2 (malloc, free 예제)**

  (https://blockdmask.tistory.com/290)

- **C언어 동적메모리할당(malloc, calloc, realloc, free)**

  (https://m.blog.naver.com/sharonichoya/220501158281)

- **c - Do I cast the result of malloc? - Stack Overflow**

  (https://stackoverflow.com/questions/605845/do-i-cast-the-result-of-malloc)

- **c - Is malloc thread-safe? - Stack Overflow**

  (https://stackoverflow.com/questions/855763/is-malloc-thread-safe)
  

<br>

<br>

<br>
