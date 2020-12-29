# 배열 색인 안의 static 키워드 : 307
- 매개변수이름 [static 한정자 표현식]
  - 배열을 매개변수로 전달할 때만 사용가능
  - (선택) static : 배열에 최소 몇 개의 요소가 있는지 알려줌
  - (선택) 한정자 (qualifier) : 배열 자체 (요소가 아님)에 붙는 한정자
    - const
    - restrict
    - ...

## 함수에 배열을 매개변수로 전달 할때 문제점
```c
int sum(int nums[], size_t count)
{
  ...
}
```
- 포인터말고 배열을 매개변수로 전해주면
  - int* const 불가능
  - restrict 불가능
  
## 예
```c
int sum(int nums[static 8], size_t count)
{
  assert(count >= 8);
}
```
- nums 배열에 최소 8개의 요소가 들어있다고 알려줌
- 컴파일러는 이 힌트에 맞춰 최적화를 할 수 있음
- 실행 중에 그 보다 작은 배열이 들어오면 정의되지 않은 결과
- assert()를 잘 써서 이런 경우를 빨리 찾는 게 좋은 습관

## const
```c
void copy_nums(int dest[const], const int src[], size_t count)
{
  for(size_t i = 0; i < count; ++i)
  {
    dest[i] = *src++;
  }
  
  dest++;       // compile error
  src[0] = 0;   // compile error
}
```
- dest : int* const
- src : const int*

## restrict
```c
void copy_nums(int dest[const restrict], const int src[restrict], size_t count);
```
- dest : int* const restrict
- src : const int* restrict
