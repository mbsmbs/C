# C 문자열 : 널 문자로 끝나는 배열

## 문자열의 길이는 정해져 있지 않다.

## char형 배열

## C 문자열은 자동으로 '\0' 널 캐릭터로 끝을 표시한다. 즉 길이는 문자들의 길이 + 1

```c
char str1[] = "abc";    // 스택에 저장

char* str2 = "abc";     // 데이터 섹션에 저장
```

## 널문자를 안넣어 주는 경우:
```c
char str[] = {'a', 'b', 'c'};
```

## 장점 : 
1. 가장 최소한의 메모리
2. 한가지 데이터형으로 문자열과 길이를 다 표현


## 단점 :
- 문자열의 길이를 알기위해서 배열 안을 다 봐야함 O(N)


## 문자열 구하는 알고리즘
1. char 배열을 처음부터 읽는다
2. 널문자를 만나면 스톱
3. 카운터 반환

```c
size_t get_str_length(const char* str)
{
      size_t i;
      for(i = 0; str[i] == '\0'; ++i)
      {
      }
      
      return i;
}
```

### 개선된 코드
```c
size_t get_str_length(const char* str)
{
    const char* p = str;
    while(*p++ != '\0')
    {
    }
    
    return p - str - 1;
}
```

```c
size_t get_str_length(const char* str)
{
    size_t count = 0;
    const char* p = str;
    
    while (*p++ != '\0')
    {
        ++count;
    }
    
    return count;
}
```


### 널문자가 없는 경우:
```C
char str[] = {'M', 'I', 'N'};
```


## 문자열 조작 manipulation
## 두 문자열 비교 : 아스키 코드로 비교
### 알고리듬
1. 두 문자열에서 문자를 하나씩 읽음
2. 두 문자를 비교:
 - 왼쪽이 작으면 -1
 - 오른쪽이 작으면 1
 - 둘다 같고 널문자면 0
3. 다음문자로

```c
int compare_string(const char* str0, const char* str1)
{
      while (*str0 != '\0' && *str0 == *str1)
      {
            ++str0;
            ++str1;
      }
      
      return *str0 - *str1;
}
```

```c
int compare_string(const char* str0, const char* str1)
{
      while (*str0 != '\0' && *str0 == *str1)
      {
            ++str0;
            ++str1;
      }

      if(*str0 == *str1)
      {
            return 0;
      }
      
      return *str0 > *str1 ? 1 : -1;
}
```

### 더 효율적인 문자열 비교 : strcmp(), strncmp()


### 대소문자 구분 없는 문자열 비교
```c
#include <ctype.h>

int string_case_insensitive_compare(const char* str0, const char* str1)
{
      int c1;
      int c2;
      
      c1 = tolower(*str0);
      c2 = tolower(*str1);
      
      while(*str0 != '\0' && c1 == c2)
      {
            c1 = tolower(*++str0);
            c2 = tolower(*++str1);
      }
      
      if(c1 == c2)
      {
            return 0;
      }
      
      return c1 > c2? 1: -1;
}
```

## 문자열 복사
```c
void copy_String(char* dest, const char* src)
{
      while(*src != '\0')
      {
            *dest++ = *src++;
      }
      
      *dest = '\0'
}
```

### C 제공 함수 strcpy()
```c
char* strcpy(char* dest, const char* src);
```
- 이미 매개변수를 포인터로 받았고 그 안에 내용을 바꿨데 char*를 반환하는게 이상한 함수.
- dest가 src보다 짧으면 소유하지 않는 메모리를 쓰게 되서 문제다. 덮어쓰기때문

### 비교적 안전한 문자열 복사: strncpy() 
```c
char* strncpy(char* dest, const char* src, size_t count);
```
- 최대 count만큼 복사
- 널 문자를 먼저 만나면 그전에 끝냄

1. src가 count보다 짧거나 같으면
- 남은걸 다 '\0'으로 채워준다

2. src가 count보다 길다면 
- count만큼 복사함
- 널 문자를 붙일 곳이 없음
- 따라서 안붙여줌

####함수를 호출뒤 안정성을 위해 널문자를 추가해주는 경우도 많다.
```c
strcpy(dest, src, DEST_SIZE);
dest[DEST_SIZE - 1] = '\0';
```


## 문자열 합치기
### strcat()
```c
# include <string.h>

char* strcat(char* dest, const char* src);
```
- dest에 널문자가 있는 곳부터 추가
- dest의 길이가 문제 방지를 위해 충분해야함.

### 보다 안전한 함수: strncat()
```c
char* strncat(char* dest, const char* src, size_t count);
```
- count만큼 dest뒤에 붙힌다.
- count+1에 널문자 넣는다

### 문자열 버퍼를 이용한 출력
```c
#include <stdio.h>
#include <string.h>

#include "buffered_print.h"

#define BUFFER_LENGTH (32)

static size_t_buffer_index = 0u;
static char s_buffer[BUFFER_LENGTH];

void buffer_print(const char* src)
{
      size_t num_left;
      const char* p = src;
      
      num_left = strlen(src);
      
      while(num_left > 0)
      {
            size_t copy_count = BUFFER_LENGTH - 1 - s_buffer_index;
            
            const int buffer_empty = s_buffer_index == 0;
            
            if(num_left < copy_count)
            {
                  copy_count = num_left;
            }
            
            s_buffer_index += copy_count;
            num_left -= copy_count;
            
            if(buffer_empty)
            {
                  strncpy(s_buffer, p, copy_count);
                  s_buffer[s_buffer_index] = '\0';
            } 
            else 
            {
                  strncat(s_buffer, p, copy_count);
            }
            
            p+= copy_count;
            
            if(s_buffer_index == BUFFER_LENGTH -1)
            {
                  printf("%s\n", s_buffer);
                  s_buffer_index = 0;
            }
      }
}
```


## 문자열 찾기
### 없는 문자열 찾는 경우
```c
#include <string.h>

int main(void)
{
      char msg[] = "I love string!, I love C! I love programming!";
      
      char* result = strstr(msg, "int");
      printf("result: %s\n", result == NULL? "(null) : result");        // null pointer를 출력하지 않도록

      return 0;
}
```

```c
char* strstr(const char* str, const char* substr);
```
- char*를 반환
- substr이 있다면 substr의 시작 주소를 반환
- substr이 없다면 NULL을 반환


## 문자열 토큰화
### strtok()
```c
#include <string.h>

char msg[] = "Hi, there. Hello. Bye";
const char delims[] = ",. ";                    //뭘로 구분할지 구분 문자를 넣어준다

char* token =  strtok(msg, delims);
while(tokeen != NULL)
{
      printf("%s\n", token);
      token = strtok(NULL, delims);
}
```
- 토큰화하는 문자열은 const가 아니기 때문에 원본이 바뀐다.
- 함수 매개변수로 NULL이 들어오면 그전에 받았던 msg를 사용하는데 이 msg는 함수내 정적 변수에 저장된다.
```c
char* strtok(char* str, const char* delims);
```


## C문자열 함수들의 특징
1. 꽤 많은 함수들이 문자열을 변경 안함. 그래서 const char*를 사용한다.
2. 문자열을 변경하더라도 원본은 변경 안 하려 한다. 사본만 변경
      - strtok() 예외
      - 원본을 지키려면 호툴하는 함수에서 사본을 만든뒤 strtok()를 호출해야함.
3. 절대 새로운 메모리를 만들어 주지 않는다.


## 문자열 예
```c
int is_alpha(int c)
{
      return (c >= 'A' && c <= 'Z') || (c >= 'a' && c <= 'z');
}


int to_upper(int c)
{
      if(is_alpha(c))
      {
            return c & ~0x20;
      }
      
      return c;
}

int to_lower(int c)
{
      if(is_alpha(c))
      {
            return c | 0x20;
      }
      
      return c;
}

void string_toupper(char* str)
{
      while(*str != '\0')
      {
            *str = to_upper(*str);
            ++str;
      }
}

void string_tolower(char* str)
{
      while(*str != '\0')
      {
            *str = to_lower(*str);
            ++str;
      }
}
```
