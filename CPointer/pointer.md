# 포인터

 - 포인터는 메모리 어딘가에 주소를 저장하는 **변수**   

 - 메모리 주소 -> 보통 16진수로 표현, 크기는 보통 4바이트    

 - 주소 연산자 '&' : 변수의 주소를 가져오고 싶을때 사용 -> &변수명  

 -  이 때 그 주소로 부터 몇바이트를 읽어야 하는지를 하드웨어한테 알려줘야 함.  
 -  int, float, char등 데이터형을 명시해주면 그 크기만큼 읽어옴

 - 데이터형* 변수명 = &데이터; 


```c
int num = 5;                    // int형 변수를 선언하고 5로 초기화
int* num_address = &num         // 변수 num의 주소를 int형 포인터 변수에 저장 데이터형의 크기만큼 읽어온다.
```

 - 읽는 방법 : 오른쪽에서 왼쪽으로
> int* -> pointer to an int  
> char* > pointer to a char


 - 역참조 연산자 '*': 포인터 변수에 저장되어 있는 주소의 값에 접근 
 > 간접적으로 값에 접근한다고 하여 간접(indirect) 연산자라고도 함
```c
int num2 = 10;                                  // int형 변수를 선언 & 초기화
int* num2_address = &num2;                      // int형 포인터 변수에 num2의 주소를 저장
printf("Address is %p\n", %num2_address);       // 포인터 변수에 저장된 주소를 출력 -> Address is 0x7ffc877abbd8
printf("Data is %d\n", *num2_address);          // 초인터 변수에 저장된 주소의 데이터를 출력 -> Data is 5
```

 - 참조와 역참조의 차이
     - 참조: 변수를 직접 가져와서 사용하는 것이 아니라 그 변수의 위치를 가리킴
     - 역참조 : 저장되어 있는 주소에 가서 저장되어 있는 값에 접근


## 포인터를 사용한 swap()

```c
void swap(int* num1, int* num2)
{
    int tmp;
    
    tmp = *num1;
    *num1 = num2;
    *num2 = tmp;
}

/* main */
int num1 = 5;
int num2 = 10;
swap(&num1, &num2);
```


## 참조에 의한 전달 VS 값에 의한 전달
 - 참조에 의한 전달 : 원본에 접근
 - 갑에 의한 전달 : 원본을 복사


## 포인터를 사용한 최소 & 최대 값 구하기
```c
// minmax.h 선언
void get_min_max(const int nums[], const size_t nums_length, int* min, int* max);
```

```c
// minmax.c 구현
#include <assert.h>
#include "minmax.h"

void get_min_max(const int nums[], const size_t nums_length, int* min, int* max)
{
    assert(length >=1);
    
    *min = nums[0];
    *max = nums[0];
    
    for(size_t i = 1; i < nums_length; ++i)
    {
        if(*min > nums[i])
        {
            *min = num[i];
        }
        
        if(*max < nums[i])
        {
            *max = num[i];
        }
    }
}
```

```c
// main.c
#include <stdio.h>
#include "minmax.h"

#define ARRAY_LENGTH (5)

int main(void)
{
    int nums[ARRAY_LENGTH] = {1, 15, 3, 7, 9};
    int min, max;
    
    get_min_max(nums, ARRAY_LENGTH, &min, &max);
    
    printf("min : %d\n", min);
    printf("max : %d\n", max);

    return 0;
}
```

## 함수에서 포인터를 반환할때 조심해야할 것 : Dangling Pointer
- Dangling Pointer는 유효하지 않은 메모리 주소를 가리키는 포인터
- 예 : 스택에서 사라진 함수의 지역변수를 가리키고 있는 경우
 
## 함수에서 포인터를 반환해도 되는 경우
- 모든 전역변수 (파일, 함수)
- 힙 메모리에 생성한 데이터



## NULL Pointer : 아무것도 안가리키는 포인터

```c
#define NULL ((void*)0)    //NULL POINTER 매크로로 표현 
```

- 포인터와 NULL 비교 가능함
```c
int* ptr;

if(ptr == NULL){...}       // NULL pointer 인지

if(ptr != NULL){...}       // 아닌지
```

- 함수는 기본적으로 NULL이 안들어온다고 가정하고 함수를 작성하는것이 좋음
> assert로 검증

- 만약 NULL이 들어온다면 매개변수명에서 분명히 밝힐것
> 예: something_or_null

- 반환값은 기본적으로 NULL을 반환 안하는걸로
- 만약 반환을 해야한다면 함수 이름에 명시하는 것이 좋음

- 널 포인터 사용의 경우: 존재하지 않는 메모리 주소에서 값을 읽으면 문제가 발생하는것을 막을때
 1. 바로 사용하지 않는 포인터 변수 NULL로 초기화
 2. 포인터가 유효한 주소를 참조하는지 확인하고 싶을때
 - NULL을 역참조 하면 문제가 되기 
 3. 댕글링 포인터를 막기 위해
 - 더이상 필요없는 메모리를 해체 했는데도 포인터가 그 메모리를 가리킨다면 문제가 되니 그 포인터를 NULL로 초기화 해줌
 
 
 ## 포인터 비교: 비교 연산자들로 할 수 있음
 
 
 ## 포인터에 배열을 대입할 수 있음. 왜냐하면 배열의 이름은 배열의 시작주소를 의미하니까
 
 
 ## 첫번째 주소와 자료형의 크기만 알면 다음 데이터들의 주소가 어디있는지 알 수 있음
 
 
 ## 포인터에 1을 더하면 다음 데이터의 주소로 이동
 ```c
 int* ptr1 = nums + 2;     // ptr1 --> nums[2] 

 int* ptr2 = &nums[2];     // ptr2 --> nums[2]
 ```
 - nums[1] = ptr[1] = *(ptr+1)    같다 하지만 가독성을 위해서는 배열을 쓰는게 낫다
 
 
 ## 주소에는 소수점을 더하거나 뺄 수는 없다.
 
 
 ## 두 주소간에 덧셈, 곱셈 그리고 나눗셈은 안되고 뺄샘만 된다. 왜냐하면 첫번째요소와 마지막요소를 알면 배열의 크기를 알 수 있기 때문. 
 
 
 ## 포인터와 배열의 차이:
 1. sizeof() :
  - sizeof(배열) --> 배열의 총크기 반환
  - sizeof(포인터) --> 포인터의 크기 반환
  
 2. 문자열 초기화
  - 배열 : 차례대로 문자들이 들어가고 마지막에 '\0' 문자가 들어감. 함수안에서 사용하면 데이터 섹션에서 복사해와서 스텍 메모리에 저장됨
  
  - 포인터 : 변수는 스택에 저장. 문자열은 데이터 섹션에 저장됨.
  
  ### 스택에 저장된 문자열은 수정해도 되지만 데이터 섹션에 저장된 문자열을 수정하면 '결과가 정의되지 않음'.
  ### 즉 데이터 섹션의 문자열은 읽기 전용
  
 3. 대입
  - 포인터에는 배열을 대입할 수 있지만 배열에는 포인터를 대입할 수 없다.
  
 4. 포인터 산술 연산
  - 포인터는 산술 연산이 가능, 배열은 불가능
  
  
  ## 연산자 결합법칙 주의 하자. 너무 헷갈리면 괄호를 쓰자
  
  
  ## 포인터가 배열보다 개미 눈꼽만큼 더 빠르다
  
  
  ## 포인터와 const :
  - 변수 수정을 못하게 함.
  - const가 보호하는 2가지:
   1. 주소 : 메모리 주소를 바꿀 수 없음
   ```c int* const ptr = &data;```
   
   2. 값 : 데이터 변경 불가능 ( 이게 제일 중요!!)
   ```c 
   const int* ptr = &data;    // 이것을 더 많이 씀
   int const * ptr = &data;
   ```
   3. 둘다
   ```c const int* const ptr = &data;```

  ## const 제거 하지 말자
  
  
  ## 포인터의 용도:
   1. 큰 데이터를 매개변수로
   2. 반환값이 2이상일 때
   3. 동적 메모리 할당
   4. 자료구조 : 연결리스트, 트리등
   
   
   ## 포인터 배열: 포인터를 담는 배열
   ```c
   int* ptr_array[5];
   ```
   
   ### 함수에서 사용 예:
   ```c
   void print_Array(int* const data[], const size_t size, const size_t lengths[])
   {
       size_t i;
       size_t j;
       const int* p;
       
       for(i = 0; i < size; ++i)
       {
           p = data[i];
           printf("nums[%d]:", i);
           
           for(j = 0; j< lengths[i]; ++j
           {
               printf(" %d", p[j]);
           }
           printf(\n);
       }
   }
   ```
   
   ## 2차원 배열과 포인터 배열은 다르다
   
   
   
   # 정리:
   1. 포인터
   2. 주소 연산자, 역 참조 연산자
   3. 널 포인터
   4. 포인터와 두 가지 const
   5. 포인터 산술 연산
   6. 포인터와 배열
   7. 포인터 배열
