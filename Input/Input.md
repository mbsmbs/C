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
  7. 이름이 FILE이라 파일만 될 것 같지만 다른 스트림도 모두 표현 가능 (stdin, stdout, stderr)
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

### 한 줄씩 읽는 방법이 유용한 경우
- 단어 하나씩 읽는 것보다 더 빠름
- cpu를 벗어나 외부 구성요소로부터 뭔가를 읽어올 때는 한 번에 많이 읽어오는게 빠름
- 따라서 버퍼 크기는 충분이 큰게 좋다
- 하지만 버퍼 오버플로 조심


## 한 데이터씩 읽기

### 3 가지 버전
1. scanf() : 키보드(stdin)으로 부터 읽음
```c
int scanf(const char* format, ...);
```
2. fscnaf() : 파일 스트림으로 부터 읽음
```c
int fscanf(FILE* stream, const char* format, ...);
```
3. sscanf : C 스타일 문자열로부터 읽음.
```c
int sscanf(const char* buffer, const char* format, ...);
```

### printf 처럼 받아올 데이터 형을 지정할 수 있음.

### 반환 값 : 
- 몇 개의 데이터를 읽었는지 반환 (int)
- 첫 데이터를 읽기 실패하면 EOF를 반환

### [서식 문자열 형식](https://en.cppreference.com/w/c/io/fscanf)
- % 뒤에 최대 4개의 지정자 : %(*) (너비) (길이 수정자) 서식 지정자

### %s를 사용시 배열 크기보다 큰 무자열이 들어오면 버퍼 오버플로.

### 정수 읽으려 했는데 문자가 있어서 읽기 실패하면 무한루프에 빠질 수 있다.
```c
int num;
int sum = 0;

while(TRUE)
{
  if(scanf("%d", &num) == 0)
  {
    printf("Error!\n");
    continue;
  }
  
  if(num == 0)
  {
    break;
  }
  
  sum += num;
}
```

### 해결법 fgets()와 sscanf()를 같이 써야 함.

### 무한 루프 문제 없이 숫자 읽기 예
```c
#define LINE_LENGTH (1024)

int sum = 0;
int num;
char line[LINE_LENGTH];

while(TRUE)
{
  if(fgets(line, LINE_LENGTH, stdin) == NULL)
  {
    clearerr(stdin);
    break;
  }
  
  if(sscanf(line, "%d", &num) == 1)
  {
    sum += num;
  }
}

// 출력코드
```

### 버퍼 오버플로 문제 없이 문자열 읽기
```c
#define LENGTH (4096)

char line[LENGTH];
char word[LENGTH];

while(TRUE)
{
  if(fgets(line, LENGTH, stdin) == NULL
  {
    clearerr(stdin);
    break;
  }
  
  if(sscanf(line, "%s", word) == 1)
  {
    printf("%s\n", word);
  }
}
```

### clearerr() : 셋팅된 오류 표시자 제거
```c
void clearerr(FILE* stream);
```

### 한 데이터씩 읽는 방법이 유용한 경우
- 텍스트를 다른 자료형으로 곧바로 읽어오는 가장 간단한 방법
- 문자열로 들어올 경우 정수로 바꾸기 너무 힘든일
- 사용자 입력 받을 때


## 한 블럭씩 읽기

### fread()
```c
size_t fread(void* buffer, size_t size, size_t count, FILE* stream);
```
- size바이트짜리 데이터를 총 count개수만큼 읽음
- 그리고 buffer에 저장
- EOF만나면 멈춤
- 그러면 count보다 적은 수를 읽을 수도 있단 이야기
- 그래서 실제로 읽은 개수를 반환
- 참고로 읽는 게 있으면 당연히 쓰는 것도 있음
```c
size_t fwrite(const void* buffer, size_t size, size_t count, FILE* stream);
```

### 파일 스트림이 있다면 작동하는 코드
```c
int nums[64];
size_t num_read;
FILE* fstream;

num_read = fread(nums, sizeof(num[0]), 64, fstream);
fwrite(nums, sizeof(nums[0]), 64, fstream);
```
- int 블록을 저장 : sizeof(int) * 64

### 한 블록씩 읽는 방법이 유용한 경우
- 이진 데이터를 읽기 위해

### 한 블록씩 읽을 때 주의할 점
- 기본 데이터형의 크기는 시스템마다 다름
- 정확히 파일에 저장할 데이터의 크기를 고정해 두는 게 좋음.
