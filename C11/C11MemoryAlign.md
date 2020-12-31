# 메모리 정렬 : 331
- 특정 바이트로 정렬을 하면 성능 향상이 되는 경우
- 정렬을 하지 않으면 아예 작동하지 않은 경우

## aligned_alloc() : 332
```c
void* aligned_alloc(size_T alignment, size_t size);
```
- alignment : 메모리 시작 주소가 정렬돼야 하는 바이트
- size : 할당할 바이트의 크기. 반드시 alignment의 배수여야 함
- 반환 값:
  - 성공 : 할당된 메모리 주소
  - 실패 : 널 포인터
- 실패 조건:
  1. alignment가 구형에서 유효하지 않거나 지원하지 않는 크기일 때
  2. size가 alignment의 배수가 아닐 때
    - 단, 첫 C11 버전에서는 널 포인터 반환이 아님 (결과가 정의되지 않음)
    - 수정 버전 (DR 460)부터 널 포인터 반환
    
## _Alignas : 333
```c
_Alignas(표현식)
```
- <stdalign.h>를 인클루드하면 alignas로 사용 할 수 있음


## 구조체 정렬할 때 더 많이 사용 : 334
- 2 가지 방법:
  - 각 멤버를 따로 정렬
  - 모든 멤버를 일괄적으로 같은 크기로 정렬
- 구조체 변수를 선언할 때, 그 변수의 시작 주소도 정렬 가능


## _Alignof()
```c
_Alignof(자료형)
```
- <stdalign.h>를 인클루드하면 alignof()로 사용
