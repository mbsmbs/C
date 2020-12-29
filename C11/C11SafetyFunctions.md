# 새로운 안전한 함수들
- 경계 점검 함수들
- 사용이 필수는 아니다

## 사용 : 319
- 다음의 매크로가 정의되어 있다면 이 기능을 지원하는 컴파일러
```c
#define __STDC_LIB_EXT1__
```

- 이 함수들을 활성화시키려면 다음의 매크로를 선언해야 함
  - 주의 : 관련 헤더 파일을 인클루드하기 전에 선언해야 함
```C
#define __STDC_WANT_LIB_EXT1__ 1
```

## gets()가 제거됨
- fgets() 쓰면 됨
- _s 함수로 더 안전하게 사용 가능 gets_s()


## gets_s() : 320
```c
char* gets_s(char* str, rsize_t n);
```
- str에 n-1개의 문자까지 저장
- 언제나 마지막에 널 문자를 붙여줌
- 실행중에 다음과 같은 오류들을 감지함
  - n이 0이거나 RSIZE_MAX보다 클 경우
  - str이 널 포인터인 경우
  - n-1개의 문자를 저장한 후에 줄바꿈이나 EOF가 발생하지 않을 때
- 오류가 감지되면?
  1. stdin으로부터 내용을 읽고 줄바꿈이나 EOF를 만날 때까지 문자를 버림
  2. 그 뒤에 등록된 핸들러 함수를 호출함
  
  
  ## spintf_s(), snprintf_S() : 321
  ```c
  int sprintf_s(char* restrict buffer, rsize_t bufsz, const char* restrict format, ...);
  ```
  
  ## fopen_s() : 322
  
  ## strlen_s() : 323
  
  ## strcpy_s() : 324
  
  ## strncpy_S() : 325
  
