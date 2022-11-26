# Call by Value & Call by Reference

- Call by Value

  - 함수 호출시 전달되는 변수의 값을 복사하여 함수의 인자로 전달
  - 복사된 인자는 함수 안에서 지역적으로 사용되는 `local value` 의 특성을 가진다.
  - 함수 안에서 인자의 값이 변경되어도, 외부 변수의 값은 변경되지 않는다.

<br>

- Call by Reference(= Call by Address)

  - 함수가 호출될 때, 메모리 공간 안에서는 함수를 위한 별도의 임시 공간이 생성된다.
    - stack frame
  - 함수 호출시 인자로 전달되는 변수의 레퍼런스를 전달한다.

<br>

- Java의 경우 함수에 전달되는 인자의 데이터 타입에 따라서 (원시자료형 / 참조자료형) 함수 호출 방식이 달라진다.

  - 원시 자료형 (primitive type) : call-by-value 로 동작 (int, short, long, float, double, char, boolean )
  - 참조 자료형 (reference type): call-by-reference 로 동작 (Array, Class Instance)

<br>

- ➕ Call by Assignment

  파이썬에서는 모든 값이 객체이다.

  1. immutable object
     - int, float, str, tuple
     - immutable 객체가 함수의 arguments로 전달되면 처음에는 call by reference로 받지만, 값이 변경되면 call by value로 동작한다.즉 함수 내에서 formal parameter 값이 바뀌어도, actual parameter에는 영향이 없다.
  2. mutable object
     - list, dict, set
     - mutable 객체가 함수의 argument로 넘어가면 call by reference로 동작한다. 즉, object reference가 전달되어 actual parameter의 값에 영향을 미칠 수 있다.

<br>
<br>

---

# 포인터

- 메모리 주소를 담을 수 있는 변수
- `*` : 역참조 연산자
- `&` : 참조 연산자
- 포인터 변수의 값을 바꿀 수는 있지만 포인터 변수가 참조하는 메모리 값 자체를 바꿀 수 없다.
  - _이게 가능하면 `p + 0.5` 도 가능해야함..!_

## NULL 포인터

- 포인터가 NULL을 가리키는 것
- 포인터가 NULL을 가리키면 포인터는 아무것도 가리키지 않는 것을 의미
- NULL 포인터를 쓰는 이유
  - 포인터를 NULL로 선언한 다음에 if 문을 통해서 포인터가 선언이 됐나 안됐나 검사하면 오류를 줄일 수 있다.
  - NULL 포인터로 초기화된 포인터와 초기화되지 않은 포인터를 다르다.
  - NULL 포인터로 초기화하면 해당 포인터는 메모리상에 어떠한 값도 가리키지 않는다. → 초기화되지 않은 포인터는 어떠한 값이라도 포함될 수 있고 참조될 수 있다.
  - 포인터를 사용하다가 사용하지 않을 때 NULL로 선언하면 된다.

## void 포인터

- 포인터는 포인터인데 즉 주소값을 저장하는 변수는 변수인데, 어떤 변수의 주소값을 담을 지 그 타입이 없다는 뜻이다.

- 현재는 타입이 없지만 캐스팅(casting)을 통해 그 타입을 바꿔줄 수 있다.

- void 포인터는 순수하게 메모리의 주소만 가지고 있는 포인터이며, 어떤 대상을 가리키는지는 정해져있지 않은 것이다.

  - _만약 void 포인터 변수에 10이라는 값이 들어있다고 했을 때, 해당 위치에 가면 무언가는 있을 것이다. 그러나, `char`가 있는지, `int` 가 있는지, `struct` 가 있는지 어떠한 정보도 알 수 없다._

<br>
<br>

---

# 함수

- 형식 : `[return 타입] 함수명 ([input 타입 변수명])`

- 함수의 이름 자체는 상수형 포인터 : 실제로 메모리의 특정 주소값을 가리키고 있는 포인터

  - input 인자의 type, return 인자의 type 정보가 함께 제공되어야한다.

1. 함수의 이름은 함수가 저장된 메모리공간을 가리키는 상수형 포인터이다.

- 함수 이름 자체는 상수형 타입으로 변경이 불가하다.

2. 함수이름의 의미하는 '주소값' 은 "함수 포인터 변수"를 선언해 저장가능

- 함수의 주소값을 가리키는 변수 선언 가능하다.

3. 함수포인터변수를 선언하려면, 사용할 함수의 반환, 매개변수 Type(자료형)을 알아야 한다.

```c
int fct(int a, int b)
{
    int c = 0;

    c = a - b;

    return c;
}

void ShowString(char* str)
{
    printf("%s \\n", str);
}

int main()
{
    char* str = "ich liebe dich";

    int(*fptr) (int, int) = fct;

//  fptr = fct;

    // 상수 함수 포인터로 출력
    printf("%d \\n", (fct(5, 7)));

    // 변수 함수 포인터로 출력
    printf("%d \\n", (fptr(10, 15)));

    void(*ptrstring)(char*);

    ptrstring = ShowString;

    ShowString(str);
    ptrstring(str);

    return 0;
}
```

<br>
<br>

---

# ERROR

- bus error

  ```bash
  int main()
  {
     int i = 1;
     int j = 3;
     int *p = &i;
     int *q = &j;
     int *r;
     printf("%p, %p \\n", p, q);
     *q = *p;
     printf("%p, %p, %d \\n", p, q, *q);
     *r = *p; // bus error
     // printf("%p\\n", r);
     return 0;
  }
  ```

  - 일반적으로 존재하지 않는 메모리에 액세스하는 것 시도할 때 발생한다.

- segmentation fault
  - 허용되지 않은 메모리에 액세스하려고 시도할 때 발생한다.

```bash
Bus errors are rare nowadays on x86 and occur when your processor cannot even attempt the memory access requested, typically:

using a processor instruction with an address that does not satisfy its alignment requirements.
Segmentation faults occur when accessing memory which does not belong to your process. They are very common and are typically the result of:

using a pointer to something that was deallocated.
using an uninitialized hence bogus pointer.
using a null pointer.
overflowing a buffer.
```

<br>
<br>

---

## ✨ Summary

- 함수 자체를 호출한다.
  - 함수의 파라미터는 호출하는 것이 아니라 넘겨주는 것이다.
- 포인터는 ‘가리키는 것’ 자체를 말한다.
  - ‘화살표(Arrow)’ 라고 생각하면 편할 듯!
  - 무엇을 가리키는 지 **대상**을 바꿔준다!
  - 하지만 이미 컴퓨터 시스템에 존재하는 메모리주소를 바꾸는 건 불가하다.
- 함수의 인자로 배열 넘겨주는 방법 : `arr[], *arr`

<br>
<br>

---

## 🙋‍♂️ Question

- 배열의 크기를 알 수 있는가
  - 배열 원소의 개수를 아는 방법 : **sizeof**(배열명) / **sizeof**(자료형)

<br>
<br>

---

## 🔗 Reference

- **C언어 NULL포인터란?**

  (https://wowon.tistory.com/153)

- **c - What is a bus error? Is it different from a segmentation fault? - Stack Overflow**

  (https://stackoverflow.com/questions/212466/what-is-a-bus-error-is-it-different-from-a-segmentation-fault#comment80046289_212466)

- **C언어 이중포인터(double pointer)**

  (https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=sharonichoya&logNo=220496504183)

- **[C언어] 함수포인터 (함수형 포인터) 이해하기 예제 :: 안산드레아스**

  (https://ansan-survivor.tistory.com/1291)

- **강의노트 12. 함수 호출방식(call-by-value, call-by-reference, call-by-assignment) · 초보몽키의 개발공부로그**

  (https://wayhome25.github.io/cs/2017/04/11/cs-13/)
