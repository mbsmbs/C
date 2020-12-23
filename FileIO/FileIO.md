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
