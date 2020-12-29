# 가변 인자 매크로 : 309
```c
#define 식별자(매개변수들, ...) 대체_목록
#define 식별자(...) 대체_목록
```
- 일반 함수와는 달리 매개변수 목록에 가변 인자만 있어도 됨
- ...로 전달받은 가변 인자는 __VA_ARGS__매크로로 사용 가능
  - 단, 다른 함수에 전달하는 용도로 밖에 사용 못함
  - #을 앞에 붙여서 문자열을 만들 수도 있음
  
## 다른 함수에 가변 인자를 전달라는 용도
- 일반 함수와 달리 가변 인자는 다른 함수로 다시 전달하는 용도 정도
```c
#define LOG_ERROR() fprintf(stderr, __VA_ARGS__);

LOG_ERROR("failed to load: %s\n", filename); --> fprintf(stderr, "failed to load: %s\n", filename); // 이렇게 바뀜
```

## 명령어랑 같이 쓰는 경우
- #를 매크로 함수의 인자 앞에 붙히면 ""로 감싸는 효과
```c
#define PRINT_LIST() puts(#__VA_ARGS__)

PRINT_LIST()                    --> puts("");

PRINT_LIST(17, "str", double);  --> puts("17, \"str\", double");
```
