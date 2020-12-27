# 전처리기
- 다른 파일들 include
- 매크로를 다른 텍스트로 대체 #define, #undef...
- 소스 파일의 일부를 조건부로 컴파일 #if, #ifdef, #ifndef, #else, #elif, #endif
- 일부러 오류를 발생시킴 #error

## 매크로 대체 : #define
```c
#define 식별자 대체_목록(선택)
```
- #define A (10):
  - 전처리기가 코드를 보다가 A가 보이면 모두 (10)으로 바꾼다
  
- #define A:
  - 전처리기가 지시어로 A가 정의 돼 있는지 판단 가능

- true/false:
  - #define TRUE (1)
  - #define FALSE (0)

- 매크로 함수

- #undef 식별자:
  - 이미 지정된 식별자를 없앤다.
  
- 미리 정의되어 있는 #define:
  - __FILE__ : 현재 파일의 이름을 문자열로 표시
  - __LINE__ : 현재 코드의 줄 번호를 정수형으로 표시
  

## 조건부 컴파일
- #if 표현식
- #ifdef 식별자 or #if defined (식별자)
- #ifndef 식별자 or #if !defined (식별자)
- #elif 표현식
- #else
- #endif

- 인클루드 가드
```c
#ifndef FOO_H   // 만약 FOO_H가 정의되지 않았다면
#define FOO_H   // FOO_H를 정의
...             // 코드
#endif          // 끝
```

### 어떤 식별자가 ##define되어 있는지 판단
```c
#ifndef NULL
#define NULL (0)
#endif

#if !defined(NULL)
#define NULL (0)
#endif

#if defined(NULL)
#undef NULL
#endif

#define NULL (0)
```

### 주의
```c
#define A             // A == 0

#if defined(A)        // 참
#define LENGTH(10)
#endif

#if A                 // 거짓
#define LENGTH (10)
#endif
```

- 버전 관리용으로 사용 : 274


### 컴파일 오류 발생 : #error 메세지
```c
#if something
#error "error message"
#endif
```

### 컴파일 중에 매크로 정의하기, 배포용으로 컴파일하기 : 275


## 매크로 함수
```c
#define SQUARE(A) a * a
#define ADD(a, b) a + b
```

### 매크로 여러 줄일때
- \를 사용하면 매크로를 여저 줄로 나눌 수 있다 (한 줄이라고 인식됨)

### 매크로 함수의 활용
- 어써드 재정의 : 나만의 어써트 만들 수 있음 : 277

### 전처리기 명령어 : #, ## 명령어 : 277

### 매크로 함수의 장점과 단점:
- 장점:
  - 함수 호출이 아닌 곧바로 코드를 복붙
  - 함수 호출에 따른 과부하가 없음
  - C에서 불편한 것들 중 일부는 매크로 꼼수로 해결 가능

- 단점:
  - 디버깅이 매우 어려움
  - 중단점 사용 불가
  

### 튜플 : 278

### getter : 279
