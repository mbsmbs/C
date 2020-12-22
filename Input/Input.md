# 입력

## 입력의 출처
- 스트림
- 문자열

## 입력처리 전략
- 한 글자씩 읽기
- 한 줄씩 읽기
- 한 데이터씩 읽기
- 한 블록씩 읽기 (이진 데이터)

## 한 글자씩 쓰기

### 불완전한 한 글자씩 읽기
1. 한 글자를 읽어온다
2. 그 글자를 필요한 곳에 사용
3. 1번 단계러 되돌아 간다
```c
#include <stdio,h>

int c;

while(TRUE)     // #define
{
  c = getchar();  
  putchar(c);
}
```
1. getchar() 함수가 아직 반환하지 않음
2. a를 입력 후 엔터를 누르면
3. 버퍼로 부터 한글자를 읽어옴
4. 읽은 문자 한 개를 출력
5. 버퍼에 문자가 남아 있으면 3번으로 돌아감
6. 마지막 \n을 만나면 출력하고 다음줄로 이동

```c
int getchar(void);  //   == fgetc(stdin)
int fgetc(FILE* stream);
```

### 읽는것을 어떻게 멈추나?
- 읽는것을 성공하면 문자를 실패하면 EOF를 반환
- EOF는 음수
- char는 부호가 있을 수 도 있고 없을 수도 있다.
- char에 언제나 음수 값을 담는게 불가
- 그래서 getchar가 int를 반환하는 것임.

### getchar()
- 키보드(stdin)으로부터 문자를 하나 읽음
- 반환 값
  - 성공 : 읽은 문자를 반환
  - 실패 : EOF를 반환
- fgetc(stdin)와 같음

### 한 글자씩 읽는 알고리듬
1. 한 글자(char)를 읽어온다.
2. 글자를 읽어오는데 실패했다면 (EOF) 프로그램 종료
3. 아니라면 그 글자를 필요한 곳에 사용
4. 1번으로 돌아간다.
```c
#include <stdio.h>

int c;

c = getchar();

while(c != EOF)
{
  putchar(c);
  c = getchar();
}
```

### EOF 어떻게 치나?
- window : ctrl + z
- unix : ctrl + d

### 한 글자씩 읽어오는 방법
- 가장 간단한 방법
- 입력이 문자/문자열일때 매우 좋음
- 쓸데없이 메모리에 입력값을 저장해 두지 않아도 됨

### 공백(whitespace)과 줄세기
```c
#include <ctype.h>
#include<stdio.h>

void print_whitespace_stat(void)
{
  int c;
  size_t num_whitespaces = 0u;
  size_t num_newlines = 0u;
  
  c = getchar();
  while(c != EOF)
  {
    if(issapce(c)
    {
      ++num_whitespaces;
      
      if(c == '\n')
      {
        ++num_newlines;
      }
    }
    
    c = getchar();
  }
  
  printf("# whitespaces: %5d\n", num_whitespaces);
  printf("# new lines: %5d\n", num_newlines);
}
```

## 한 줄씩 읽기

### 한 줄씩 읽는 알고리듬
1. 한 줄을 읽어온다.
2. 한 줄을 읽어오는데 실패하면 프로그램을 종료
3. 성공했다면 한 줄 읽어온 데이터를 필요에 따라 사용
4. 1번 단계로 되돌아 간다.

### gets() : 위험한 함수. 쓰지 말자.
```c
char* gets(char* str);
```
- stdin에서 '\n' 아니면 EOF를 만날 때까지 계속 문자들을 읽어서 str 배열에 저장.
- 마지막 문자 뒤에 널 문자 넣어줌.
- stdin에서 새 줄 문자를 제거하지만 버퍼에 저장하지는 않음.
- 반환:
  - 성공 시, str
  - 실패 시, NULL (pointer)
- 매우매우매우 위험한 함수
- C11에서는 완전 제거
- 버퍼 오버플로가 발생
- 올바르지 않은 메모리 주소에 입력값을 써버림.

### fgets(): 안전한 함수
```c
char* fgets(char* str, int count, FILE* stream);
```
- <stdio.h>에 있음
- 널 문자를 포함하기 때문에 최대 count-1 개의 문자열을 읽어서 str에 저장
- 새 줄을 만나지 않아도 이 함수가 반환될 수 있음
- str에 새 줄 문자까지 넣어줌
- stream: 데이터를 읽어올 스트림.
- 키보드 입력을 읽어오고 싶으면 stdin을 넣어주면 됨.
- FILE형: 스트림을 제어하기 위해 필요한 정보를 담고 있는 자료형
  1. 파일 위치 표시자
  2. 스트림이 사용하는 버퍼의 포인터
  3. 읽기/쓰기 중에 발생한 오류를 기록하는 오류 표시자
  4. 파일의 끝에 도달했음을 기록하는 EOF 지시자
  5. 플랫폼마다 이 자료형을 구현하는 방식이 다름
  6. 입력 및 출력 스트림은 오직 FILE 포인터로만 접근 및 조작 가능
  7. 이름이 FILE이라 파일만 될 것 같지만 다른 스트림도 모두 표현 가능
  8. 나중에 실제 파일을 열어서 거기에 쓰거나 읽을 때도 이 자료형을 사용할 것임.
- 반환 값:
  - 성공 : str
  - 실패 : NULL
```c
#include <stdio.h>

#define LINE_LENGTH(10)

char line[LINE_LENGTH];

while (fgets(line, LINE_LENGTH, stdin) != NULL)
{
  printf("%s", line);
}
```
- '\n'을 만날때까지 문자들을 count-1만큼 읽어오고 출력
- '\n'을 만나면 그 다음칸에 널 문자도 넣어주고 출력
- 그 전에 들어온 문자들은 지워지지 않음.
- 언제나 배열의 크기는 충분히 크게 잡을 것
- 버퍼 초기화 필요없음
