# 입력

## 입력의 출처
- 스트림
- 문자열

## 입력처리 전략
- 한 글자씩 읽기
- 한 줄씩 읽기
- 한 데이터씩 읽기
- 한 블록씩 읽기 (이진 데이터)

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
