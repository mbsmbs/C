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
      if(*str0 == *str1)
      {
            return 0;
      }
      
      return *str0 > *str1 ? 1 : -1;
}
```


