# Type Generic 수학 : 304
- 각 자료형에 맞게 알아서 동작
- 수학 함수 (math.h) 및 복소수 (complex.h)용 제네릭 매크로 함수
  - 프로그래머는 매크로 함수를 호출
  - 그러면 매개변수의 자료형에 맞는 함수가 최종적으로 호출
- 이 매크로 함수들은 <tgmat.h>에 들어 있음

## float, long double, double, int
- 이 순서로 최종 호출할 함수가 결정됨
