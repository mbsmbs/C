# C99 새로운 자료형

## long long int
- 최소 64비트이고 long이상의 크기
- 리터럴: 
  - 대문자, 소문자 상관없지만 가독성을 위해서 대문자로 쓰자
  - 부호 있는 경우 : LL
  - 부호 없는 경우 : ULL
- long long int의 서식 문자
  - 부호 있는 경우 : %lli
  - 부호 없는 경우 : %llu
  
## Bool형
- 2가지 방법:
  - _Bool:
    - 거짓 : 0, 참 : 1
    - char/int/float과 같은 값을 _Bool에 넣을 경우:
      - 0에 해당하면 0
      - 그 외의 값은 1로 변환
    - 여전히 참과 거짓은 숫자로 표현
    
  - bool : 
    - 헤더 인클루드 필요
    - <stdbool.h>헤더에 정의 되어 있음
    - bool: _Bool을 다시 정의
    - true: 1로 정의
    - false: 0으로 정의
  - 왜 2가지? 기존방식의 코드들과 충돌나지 않게 하려고 했던것
  
## 고정 폭 정수형 (fixed-width integer type)
- <stdint.h>
- int8_t / uint8_t
- int16_t / uint16_t
- int32_t / uint32_t
- int64_t / uint64_t

### 최솟값과 최댓값:
- 최소:
  - INT8_MIN
  - INT16_MIN
  - INT32_MIN
  - INT64_MIN

- 최대:
  - INT8_MAX / UINT8_MAX
  - INT16_MAX / UINT16_MAX
  - INT32_MAX / UINT32_MAX
  - INT64_MAX / UINT64_MAX
  
## 허수를 표현하는 자료형 <complex.h>
### _Imaginary
  - 허수를 나타내는 키워드
  - 일부 컴파일러는 지원 안함
  ```c
  float _Imaginary
  double _Imaginary
  long long _Imaginary
  ```
### _Complex
  - 복소수를 나타내는 키워드
  - 일부 컴파일러는 다은 이름을 사용
  ```c
  float _Complex
  double _Complex
  long long _Complex
  ```
- 함수도 있다


## IEEE 754 부동 소수점 정식 지원
- float은 IEEE 754 32비트 부동 소수점
- double은 IEEE 754 64비트 부동 소수점
- long double은 IEEE 754 확장 정밀도 부동 소수점
- 사칙 연산과 제곱근의 올림을 IEEE 754에서 정의한대로 처리
