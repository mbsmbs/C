# Type-Generic 함수 : 327
```c
_Generic(제어 표현식, 연관 목록)
```
- 컴파일 도중에 여러 가지 표현식 중 하나를 선택하는 방법
- 매크로 함수의 대체 목록으로 사용하는 게 일반적

```c
#define ceil(X) _Generic((X), \
                 long double: ceill, \
                 default : ceil,  \
                 float: ceilf)(X)
```
- 연관 목록에 그 형이 있으면 그에 대응하는 표현식으로 대체
- 연관 목록에 형이 없고 default: 가 있다면 그에 대응하는 표현식으로 대체
- default: 도 없다면 컴파일 되지 않음
