# 동적 메모리

## 3가지 단계:
1. 메모리 할당
2. 메모리 사용
3. 메모리 해제

### 메모리 할당
- 힙 관리자에게 메모리를 몇 바이트만큼 달라고 요청하면 그 해당 바이트만큼 연속된 메모리를 찾아서 메모리 주소를 반환

### 메모리 사용
- 원하는데로 사용

### 메모리 해제
- 잊지말고 다 썼으면 돌려주자


## 함수

### 할당  <stdlib.h>
- malloc()
  ```c
  void* malloc(size_t size);
  ```
  - size만큼 메모리를 반환해줌
  - 초기화 안해줌. 즉 쓰레기 값
  - 메모리가 할당 실패하면 NULL
  
- calloc()
  ```c
  void* calloc(size_t num, size_t size);
  ```
  - 메모리를 할당할 때 자료형의 크기(size)와 수(num)를 따로 지정
  - 모든 바이트를 0으로 초기화 해 줌
  - 잘 안씀
  - malloc() + memset()과 비슷 : 이걸 많이 씀

### 해제  <stdlib.h>
- free() : malloc()쓰고 곧바로 이거 쓰자
  ```c
  void free(void* ptr);
  ```
  - 할당 받은 메모리를 해제하는 함수
  - 엉뚱한 주소 해제 하면 문제
  - 메모리 할당하는 포인터와 포인터 연산에 사용하는 포인터는 분리하자
  - 해제한 메모리 또 해제하면 문제
  - 해제한 메모리를 사용해도 문제

- 코딩 표준
  - free()한 뒤에 변수에 NULL을 대입해서 초기화
    > 안 그러면 해제된 메모리인지 나중에 모르니까
    > 널 포인터를 free()의 매개변수로 전달해도 안전
  - 할당 후 곧바로 free() 잊지말자
  
- free()는 몇 바이트를 해제할지 어떻게 알지?
  - 보통 malloc(32)하면 그것보다 조금 큰 메모리를 할당한 뒤, 제일 앞부분에 어떤 데이터들을 채워 놓음
  - 그리고 그 주소에서 채워놓은 데이터만큼 오프셋 값을 더한 주소 값을 돌려줌
  - 그리고 거기서부터 32바이트를 사용
  - 메모리 해제시 free()는 다시 오프셋만큼 빼서 그 앞 주소를 본 뒤, 실제 몇 바이트가 할당됐었는지 확인 후 해제

### 재할당  <stdlib.h>
- realloc()
```c
void* realloc(void* ptr, size_t new_size);
```
- 이미 존재하는 메모리(ptr)의 크기를 new_size 바이트로 변경
- 새로운 크기가 허용하는 한 기존 데이터를 그대로 유지

- 크기가 커져야할 때:
  ```c
  str = malloc(LENGTH * sizeof(char));
  str = realloc(str, 2 * LENGTH * sizeof(char));
  ```
  1. 지금 갖고 있는 메모리 뒤에 충분한 공간이 없으면 새로운 메모리를 할당한 뒤, 기존 내용을 복사하고 새 주소 반환
  
  2. 지금 공간이 충분하다면 그냥 기존 주소를 반환. 그리고 추가된 공간을 쓸 수 있게 됨. (보장은 안된다)
 
 - 크기가 작아질 때:
  1. 기존 주소가 반환되거나 새로운 주소가 반환된다.
  
- realloc()에서 메모리 누수가 날 수 가 있다:
  
  1. 반환값:
    - 성공 : 새롭게 할당된 메모리의 시작 주소를 반환하며 기존 메모리는 해제 됨
    - 실패 : NULL을 반환하지만 기존 메모리는 해제되지 않음
  
  2. 실패시 메모리 누수가 발생할 수 있음
  ```c
  void* nums;
  
  nums = malloc(SIZE);
  
  nums = realloc(nums, 2 * SIZE); /* NULL 반환 */
  ```
  > 원래 nums에 저장되어있던 주소가 사라짐
  > NULL이 반환됐다는 얘기는 재할당에 실패했다는 의미
  > 따라서 기존 메모리는 해제지 않았음
  > 그리고 그 주소를 읽어버려서 해제 불가능 -> 메모리 누수
  
  3. 올바른 재할당:
  ```c
  void* nums;
  void* tmp;
  
  nums = malloc(SIZE);
  
  tmp = realloc(nums, 2 * SIZE);
  if(tmp != NULL)
  {
    nums = tmp;
  }
  ```
  
  4. realloc()은 malloc() + memcpy() + free()를 사용한 것과 비슷하다.
  ```c
  void* nums;
  void* tmp;
  
  nums = malloc(LENGTH);
  
  tmp = malloc(LENGTH *2);
  if(tmp != NULL)
  {
    memcpy(tmp, nums, LENGTH);
    free(nums);
    nums = tmp;
  }
  
  free(nums);
  ```

### malloc(), free() 사용 예 : 232

### memset() : <string.h>
```c
void* memset(void* dest, int ch, size_t count);
```
- char로만 초기화
- 그 외의 자료형으로 초기화하려면 직접 for문을 써야함
- 다음과 같은 경우 결과가 정의되지 않음
  - count가 dest의 영역을 넘어설 경우 (소유하지 않은 메모리에 쓰기)
  - dest가 널 포인터일 경우 (널 포인터 역참조)

### memcpy() : <string.h>
```c
void* memcpy(void* dest, const void* src, size_t count);
```
- src의 데이터를 count바이트 만큼 dest에 복사
- 다음과 같은 경우 결과가 정의되지 않음:
  - dest의 영역뒤에 데이터를 복사할 경우 (소유하지 않은 메모리에 쓰기)
  - src나dest가 널 포인터일 경우 (널 포인터 역참조)
  
### memcmp()
```c
int memcmp(const void* lhs, const void* rhs, size_t count);
```
- count바이트 만큼의 메모리를 비교하는 함수
- 널 문자를 만나도 계속 진행
- 결과가 정의되지 않는 경우:
  - lhs와 rhs의 크기를 넘어서서 비교할 경우 (소유하지 않은 메모리에 쓰기)
  - lhs나 rhs이 널 포인터일 경우 (널 포인터 역참조)
- 구조체 2개를 비교할 때 유용
  - 단 구조체가 포인터 변수를 가질 경우에는 다르다.
  
### 정적 VS 동적
- 정적 메모리 사용 우선
- 안 될때 동적

### 함수안에서 동적 메모리 할당을 할 경우 이름에 표시하자
