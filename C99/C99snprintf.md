# snprintf()
```c
int snprintf(char* restrict buffer, size_t bufsz, const char* restrict format, ...);
```
- sprintf는 버퍼의 범위를 넘어서서 계속 쓸 수 있는 문제를 보완
- 최대 bufsz - 1개의 문자열을 출력
- 나머지 하나는 바로 널 문자용
  - 언제나 붙여준다
  - strncpy()와 다르다
- 마지막 요소에 널 문자 넣는 코드가 많이 보임. strncpy()처럼
