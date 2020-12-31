# 기본 문법

## main(void)
```c
int main(void)
{
  return 0;
}
```
- 프로그램의 진입점
- C코드를 빌드해서 나온 실행파일(.exe or .out)을 실행하면 main(void)함수가 자동적으로 실행됨
- 꼭 int를 반환. 0 반환 : 프로그램에 문제가 없다

## comment
- C89는 /* ... */  이것만 지원

## 자료형

### 기본 자료형
- char : 최소 8비트
- short : 최소 16비트
- int : 최소 16비트 & short 이상
  - unsigned int num =  123456u;  'u' 붙이자
- long : 최소 32비트 & int 이상
  - unsigned long num = 1234567489ul; 'ul' 붙히자 
- float : 32 비트
  - float pi = 3.14f;
  - unsigned 없음
- double : 64비트 & float 이상
  - double num = 3.14;
  - unsigned 없음
- long double : double 이상
  - double 보다 정밀도 높음
  - long double num = 3.14;
  - unsigned int 없음
- unsigned & signed : 부호가 있고 없고 (예: unsigned int, int)

### bool 형 : C89 없음, C99부터

### 열거형 : enum
- C에서는 그냥 정수에 별명 붙이는 수준

### 변수 선언 위치
- 변수 선언은 반드시 블럭의 시작에서만 가능


## 연산자
- sizeof() : 피연산자의 크기를 바이트로 반환해주는 연산자
  - 컴파일 중에 찾음
  - size_t 형
- size_t : typedef한 것  보통 unsigned int
- 역 참조 연산자 * : 포인터 변수에만 사용. 값에 접근
- 주소 연산자 : 어떤 변수의 메모리 주소를 반환
- '.' 연산자 : 구조체, 공용체의 멤버변수에 접근
- '->' 연산자 : '.' + '*' 합친것. 

### 비교 연산자 & 논리 연산자
- 비교 : == != < <= > >=
- 논리 : && || !

### 조건 연산자 ternary operator
```c
int min = num1 < num2 ? num1 : num2;
```

## 조건문
- if, else, else i
- true/false반환이 아니라 0/1을 반환
- switch/case:
  - case에 사용가능한 데이터형 : int, char, enum
  - break 안넣으면 그 밑에 있는 코드들 실행 -> fall-through
    - 의도를 가지고 break를 안넣을 거면 주석으로 명시하자
    
## 반복문
### for
```c
int sum = 0;
size_t i;

for(i = 0; i < 10; ++i)
{
  sum += i;
}
```

### while
```c
size_t num = 30;

while(num > 0)
{
  if(num == 20)
  {
    continue;
  }
  
  printf("%d", num);
  --num;
  
  if(num < 15)
  {
    break;
  }
  
}
```

### do-while
```c
do
{
  y = f(x);
  x--;
} while (x > 0);
```


## Function
### 선언과 정의
- 선언과 정의를 분리하거나 하나로 합칠 수 있다.

### 연산자 우선순위와 평가 순서는 서로 아무 연관이 없음
- 어떤 함수가 먼저 평가될지는 모름
- &&, ||는 평가 순서를 강제하는 연산자 
- 삼항 연산자도 평가 순서 보장
- ;도 보장
- 함수를 실행하기 전에 모든 매개변수도 평가 됨


## 범위
- ({}) :
  - 안에서 밖은 접근
  - 밖에서 안은 안됨
- 함수 중간에 블록을 열고 변수 선언 가능
- static : 파일 안에서 접근 가능 (데이터 섹션에 있기 때문)
  - 전역 변수


## const
- 상수를 만들어준다.
- 모든 변수에 const 붙히자
- 정말 값이 변해야 한다면 const 생략


## goto
```c
goto label_name;

label_name:
  ...
```
- 순서를 어기고 label_name으로 점프
- 사용할 때 조심하자
- 아래쪽으로만 점프하자


## 배열
