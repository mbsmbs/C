# C99

## 인라인 함수
```c
inline 반환형 함수_이름(매개변수 목록){}
```
- 컴파일러에게 최적화 해달라고 알려주는 힌트
  - 매크로 함수처럼 코드를 복붙해줌
  - 함수 호출이 사라짐
  - 힌트일 뿐이라 컴파일러가 무시할 수도 있음
  - inline이 없어도 컴파일러가 알아서 최적화를 해줄 수도 있음
- 복붙을 할 수 있으려면?
  - 인라인 함수를 호출하는 코드를 컴파일할 때 그 함수의 구현을 알아야 함
  - 따라서 인라인 함수의 구현은 소스 파일이 아니라 헤더 파일에 둠
  
### 인라인 함수 작동법
- 헤더 파일에 구현되야함
```c
/* 헤더파일 */
inline int add(int op1, int op2)
{
  return op1 + op2;
}

/* 메인파일 */
#include "*.h"
int result = add(10, 20) * add(30, 50);
```
- 매크로는 전처리기가 코드를 무식하게 토씨 하나 안 틀리게 복붙 함
- 인라인 함수는 컴파일러가 컴파일 중에 함수 호출을 코드로 바꿔줌

### 링킹 오류가 나는 이유
- 모든 .o 파일에 add()가 들어있음
- 동일한 이름의 함수가 2개나 있음
- 그중 어떤 add()와 링킹을 해야 하는지 몰라서 오류 발생

### 해결 방법
- inline 키워드 사용 : 링커는 모르게 해달라
- 컴파일러에게 호출용 함수가 아니라 코드 교체용이라 알려줌
- 그 결과 링커가 볼 수 있는 함수 심볼을 만들지 않음

### 컴파일러가 해당 함수를 인라인화 한다는 보장이 없음
- 되면 문제 없고
- 안되면 문제 왜?
  - inline을 쓰면 컴파일러에게 해당함수를 함수로 쓰지 말고 교체용으로 쓰라고 함. 
  - 그러다보니 컴파일러는 링커가 볼 수 있는 함수 심볼을 만들지 않음.
  - 인라인이 안되면 함수 심볼마저 없고
  - 따라서 링커는 해당 함수를 찾을 방법이 없음
  
### 링킹 오류 해결법
- 인라인 함수롸 똑같이 구현된 일반 함수가 어딘가에 존재하면 됨 (c파일에)
  - 링커가 찾을 수 만 있으면 문제 없음
  - 그러나 쓸데 없는 코드 중복
 
- 올버른 해결법 : extern
  - 인라인이 되면 인라인 사용 안되면 일반 함수 사용
  - extern을 붙히면 링커가 찾을 수 있는 함수도 만들어 줌
  
- 최종 결론:
  1. .h 파일 안에 인라인 함수를 구현
  ```c
  /* simple.h */
  #ifndef SIMPLE_MATH_H
  #define SIMPLE_MATH_H
  
  inline int add(int op1, int op2)
  {
    return op1 + op2;
  }
  
  #endif /* SIMPLE_MATH_H */
  ```
  
  2. 그에 대응하는 .c파일을 만듦
  
  3. 그 파일에서 인라인 함수가 들어있는 .h파일을 인클루드
  
  4. 그 파일에서 인라인 함수를 extern 인라인 함수로 다시 선언
  ```c
  /*  simple_math.c */
  #include "simple.h"
  
  extern inline int add(int op1, int op2);
  ```
  - 이렇게 하면 그 .c파일 안에서만 함수의 심볼이 바옴
  - 이제 컴파일 중 인라인이 되면 헤더 파일에 있는 구현을 사용
  - 인라인이 안 되면 링커가 .c파일에서 나온 심볼을 이용해서 링킹
  
  
  ### 정적 인라인 함수 : 291