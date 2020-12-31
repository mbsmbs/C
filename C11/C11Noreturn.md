# _Noreturn : 330

```c
_Noreturn 반환형 함수이름(매개변수)
```
- <stdnoreturn.h>를 인클루드 하면 noreturn을 대신 사용 가능
```c
noreturn void cross_the_river()
{
  ...
}
```

- void와는 달리 noreturn은 그 함수에서 원래 호출자로 돌아가지 않는다는 의미

- 멀티 쓰레딩을 할 때는 필요한 경우가 있다
- 대부분 무언가를 종료하는 함수들
