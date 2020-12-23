# 파일 입출력

## 파일 관련 연산
1. 파일을 열어서 파일 스트림을 가져옴
2. 그 파일 스트림을 사용해서 하고 싶은 걸 함
3. 그 파일을 닫아줌

## 파일 열기
```C
#include <stdio.h>

#define LENGTH (1024)

FILE* stream;
char list[LENGTH];

stream = fopen("hello.txt", "r");

if(fgets(list , LENGTH, stream) != NULL)
{
  printf("%s", list);
}
```

```c
FILE* fopen(const char* filename, const char* mode);
```
- filename으로 지정된 파일 열기
- mode: 모드 설정 (읽기 모드, 이진 파일 등)
- 반환 값 : 파일 스트림 포인터

### 열기 모드:
- r (read): 읽기 전용
  - 성공 : 파일 첫부분 부터 읽는다.
  - 실패 : 열기에 실패한다.
- w (write): 쓰기 전용
  - 성공 : 파일의 내용을 모두 없앤다.
  - 실패 : 새 파일 생성
- a (append): 이어 쓴다
  - 성공 : 파일의 끝부분부터 읽는다.
  - 실패 : 새 파일 생성
- r+ (read extension): 읽기/쓰기용 파일 열기
  - 성공 : 파일 첫부분 부터 읽는다.
  - 실패 : 오류
- w+ (write extension): 읽기/쓰기용 파일을 생성
  - 성공 : 파일 내용을 모두 없앤다.
  - 실패 : 새 파일 생성
- a+ (append extended): 읽기/쓰기용으로 파일 열기
  - 성공 : 파일의 끝부분부터 읽는다.
  - 실패 : 새 파일을 생성

- b를 붙이면 이진 모드로 파일을 연다 : rb, wb, ab, r+b, w+b, a+b
- 이진 모드:
  - 유닉스 계열: r == rb
  - 윈도우 : 새 줄 문자 처리하는 것만 달라짐

## 파일 쓰기
```c
size_t fwrite(const void* buffer, size_t size, size_t, count, FILE* stream);
```
- 첫번째 인자는 void*다
- 그냥 비트 패턴이 줄줄이 들어옴
- void*이기 때문에 데이터크기에 의미가 없어짐
- 0xOA가 '\n'인지 '10'인지 float인지 알 수 가 없기때문
- 따라서 fflush()가 유일한 해결법
```c
#include <stdio.h>
#include <string.h>

#define LENGTH (5)

void write_file(char* filename)
{
  FILE* stream;
  int scores[LENGTH] = {20, 74, 99, 49, 63};

  stream = fopen(filename, "wb");

  fwrite(scores, sizeof(scores[0]), LENGTH, stream);
  fflush(stream);
}
```

## 파일 읽기
```C
#include <stdio.h>
#include <string.h>

#define LENGTH (6)

void read_file(const char* filename)
{
  FILE* stream;
  char data[LENGTH];
  
  stream = fopen(filename, "rb");
  
  while(TRUE)
  {
    if(fgets(data, LENGTH, stream) == NULL)
    {
      break;
    }
    printf("%s\n", data);
  }
}
```

## 파일에 이어서 쓰기
```c
#include <stdio.h>
#include <string.h>

#define LENGTH (1024)

void append_file(const char* filename)
{
  FILE* stream;
  char data[LENGTH];
  
  stream = fopen(filename, "ab");
  
  if(fgets(data, LENGTH, stdin) != NULL)
  {
    fwrite(data, strlen(data), 1, stream);
  }
}
```

## 제일 중요한거 : 파일 닫기!!
- 내가 연파일 내가 닫아야 한다.
- 파일은 운영체제가 열어주는데 우리가 언제 다 썼는지를 모른다
- 자동으로 닫아주지 않는다.
- 운영체제가 뻗을 수 있음

### 파일 닫기
```c
int fclose(FILE* stream);
```
- 파일을 닫음
- 반환 값:
  - 성공 : 0 
  - 실패: EOF
- 버퍼링 중인 스트림 동작:
  - 출력스트림 : 버퍼에 남아있는 데이터는 파일로 다 보냄
  - 입력 스트림 : 무시
  
### 파일 열기 실패할 때 오류 처리
```c
#include <stdio.h>

#define LENGTH(1024)

void open_file(const char* filename)
{
  FILE* stream;
  char data[LENGTH];
  
  stream = fopen(filename, "rb");
  if(stream == NULL)
  {
    fprintf(stderr, "error while opening %s", filename);
    return;
  }
  
  if(fgets(data, LENGTH, stream) != NULL)
  {
    printf("%s", data);
  }
  
  if(fclose(stream) != 0)
  {
    fprintf(strerr, "error while closing");
  }
}
```

### stderr
- strout이랑 비슷
- 오류관련 메시지를 출력하는 전용 스트림
- 버퍼링을 안 씀 왜냐하면 오류는 바로바로 보여줘야 하니까

### 실패의 이유를 알고 싶을 때
- errno 매크로 : <errno.h> : 에러번호만 알려줌
```c
#include <stdio.h>
#inclde <errno.h>

void open_file(const char* filename)
{
  FILE* stream = fopen(filename, "rb");
  if(!stream)
  {
    fprintf(stderr, "[%d] error while opening %s", errno, filename);
    return;
  }
}
```
- 에러를 설명해주는 함수 :  
```c
#include <string.h>

char* strerror(int errnum);   // errno를 넣으면 친정한 설명이
```

### 상세한 오류정보
```c
#include <stdio.h>
#inclde <errno.h>

void open_file(const char* filename)
{
  FILE* stream = fopen(filename, "rb");
  if(!stream)
  {
    fprintf(stderr, "%s - %s", filename, strerror(errno));
    return;
  }
}
```

### 더 간단한 오류처리 설명 함수
```c
void perror(const char* s);
```
- 내부적으로는 위에 오류처리 함수들 다 호출해준다.
```c
#include <stdio.h>
#inclde <errno.h>

void open_file(const char* filename)
{
  FILE* stream = fopen(filename, "rb");
  if(!stream)
  {
    perror("error while opening");
    return;
  }
}
```

### C에서의 오류처리
- 함수가 곧바로 오류코드를 반환
- 내부적으로 오류코드를 전역변수로 들고 있다가 검사

### 파일 읽고, 쓰고 이어쓰는 함수들 완성
- 파일 닫기
- 오류처리

## 파일 복사하기
```c
void copy_file(const char* src, const char* dst)
{
  FILE* src_file;
  FILE* des_file;
  intc;
  
  src_file = fopen(src, "rb");
  if(src_file == NULL)
  {
    perror("error while opening source file");
    return;
  }
  
  dst_file = fopen(dst, "wb");
  if(dst_file == NULL)
  {
    perror("error while creating target file");
    goto close_source;
  }
  
  c = fgetc(src_file);
  while (c != EOF)
  {
    fputc(c, dst_file);
    c = fgetc(src_file);
  }
  
  if(fclose(dst_file) == EOF)
  {
    perror("error while closing target file");
  }
  
  close_source:
    if (fclose(src_ffile) == EOF)
    {
      perror("error while closing source file");
    }
}
```
