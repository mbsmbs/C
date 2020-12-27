# va_copy() : 복사가 가능

```c
va_copy(dest, src)
```
- C99에 추가
- 가변 인자 목록을 복사하는 매크로 함수
- dest를 다 사용한 후에는 반드시 va_end()를 호출해야 함
