# 출력

## 서식 지정 (formatted)
### 3가지 종류 : 작동법은 동일
  - printf() : 콘솔창에 출력
  - fprintf() : 스트림에 출력
  - sprintf() : 문자열에 출력

### printf()는 일반 문자열, 서식 문자열을 매개변수로 받는다.

### 서식 문자열 (format string)
  - %로 시작하는 문자열
  - 소수점 이하 자리수, 자릿수 정렬, 어떤 데이터를 출력할지 알려준다.
  - 하나 이상의 데이터가 들어 갈 수 있고 순서대로 출력한다.

### [서식 문자열 형식](https://en.cppreference.com/w/c/io/fprintf)
  - % (플래그) (너비) (.숫자 정밀도 | .문자열 최소/최대 출력 개수) (길이) 서식 지정자
  - 서식 지정자만 빼고 다 선택사항
```c
printf("%%\n");                 // %를 출력
printf("%c\n");                 // char
printf("%s\n");                 // char[]
printf("%d\n", -10);            // int
printf("%u\n", 10);             // unsigned int
printf("%o\n", 10);             // unsigned int를 8진수로
printf("%x\n", 10);             // unsigned int를 16진수로 소문자로 출력
printf("%X\n", 10);             // unsigned int를 16진수로 대문자로 출력
printf("%f\n", 3.14);           // float, double
printf("%e\n", 3.14);           // 부동소수점을 지수 표기법으로 출력
printf("%E\n", 3.14);           // 부동소수점을 지수 표기법으로 출력
printf("%p\n", (void*)name);    // 포인터 값을 출력
```

### ASCII 표 그리기
```c
void print_Ascii_table(void)
{
  const int MIN_ASCII = 32;
  const int MAX_ASCII = 126;
  const int NUM_CHARS = MAX_ASCII - MIN_ASCII + 1;
  const int NUM_COLS = 3;
  const int NUM_ROWS = (NUM_CHARS + NUM_COLS -1) / NUM_COLS;
  
  int r;
  int ch;
  
  printf("Dec Hex Char\tDec Hex  Char\tDec Hex Char\n");
  
  for(r=0; r < NUM_ROWS - 1; ++r)
  {
    ch = MIN_ASCII + r;
    printf("%03d %#X %c\t, ch, ch, ch");
    
    ch += NUM_ROWS;
    printf("%03d %#X %c\t, ch, ch, ch");
     
    ch += NUM_ROWS;
    printf("%03d %#X %c\t, ch, ch, ch");
  }
  
  for(ch = MIN_ASCII + r; ch <=MAX_ASCII; ch += NUM_ROWS)
  {
    printf("%03d %#X %c\t", ch, ch, ch);
  }
}
```

