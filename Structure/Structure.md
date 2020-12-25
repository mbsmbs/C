# 구조체
- 구조체는 값형

## 필요성
- 매개변수 순서가 바뀔때
- 묵시적으로 변환 가능한 자료형이 여러개일 때
- 매개변수 목록이 길어질 때
- 누가 매개변수를 바꿀 때

### 선언
```c
struct date
{
  int year;
  int month;
  int day;
};
```
- 세미콜론 잊지말자
- 새로운 형을 만든다
- 그안에 데이터들이 들어간다.

### 사용
```c
struct typeName date;

int is_monday(struct date date_input);
```
- 함수의 매개변수로도 사용 가능.
- 멤버 변수 접근법 : '.'을 사용한다.  var_name.data1 = value;

### typedef
- 어떤 자료형의 별명을 지어줌
```c
struct date
{
  int year;
  int month;
  int day;
};

typedef struct date date_t;
```
```c
date_t date;
```
- 다른 방법들
```c
typedef struct date
{
  int year;
  int month;
  int day;
} date_t;
```
```c
typedef struct
{
  int year;
  int month;
  int day;
} date_t;
```
- C에서 _t로 끝나는 자료형은 typedef를 사용한 것
- 선언과 동시에 초기화 안됨
- 초기화 : 직접 대입 or date_t = date = {0, };
- 요소 나열법은 쓰지 말자

### 구조체 매개변수
- 원본 바꾸려면 구조체 포인터 사용
```c
void increase_year(date_t* date)
{
  (*date).year = (*date).year + 1;  //  () 연산자 우선순위때문에
  // date->year = date->year + 1;   same 
  // date->year++;                  same  
}
```
- 구조체가 값이면 '.' 사용
- 구조체가 포인터면 '->' 사용

### 베스트 프랙티스
- 구조체안의 모든 데이터를 복사하기에는 너무 오래걸리니까 주소로 전달
- 원본을 지키려면 const 사용
- 여러개의 변수보다 구조체 하나 전달하는게 좋다

### 함수 반환
- 다른 함수와 같음
- 여러데이터를 한번에 반환하고 싶을 때 구조체를 사용하면 됨

### 대입도 된다

### 구조체 배열도 만들 수 있음


## 얕은 복사, 깊은 복사

### 얕은 복사
- 데이터가 아니라 주소를 복사하는 것
- 주소를 저장하는 건 소용이 없다.

### 깊은 복사
- 구조체 변수마다 독자적인 메모리 공간은 만들고 거기에 복사해야 함

### 구조체안에는 포인터가 없는게 좋다. 

### 배열을 복사하고 싶은 경우에 구조체 안에 배열을 선언한다. 그리고 함수 매개변수로 넘겨주면 복사된다.

### 구조체안에 다른 구조체도 들어갈 수 있다.

### 구조체 바이트 정렬
- 하드웨어마다 읽어야 되는 바이트 수가 정해져 있다. (예: 4 바이트씩)
- 정해져 있는 바이트 수보다 적으면 안쓰는 바이트를 넣어서 그 바이트수를 채운다.
- 그걸 padding이라고 한다.
- padding도 줄일 수 있다.
- 베스트 프랙티스 : assert 사용, 구조체에 패딩을 명시적으로 넣기도 함
```c
#include <assert.h>

assert(sizeof(user_info_t) == 76);
```

### 점, 선 , 사각형 : 200
