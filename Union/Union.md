# 공용체
- 똑같은 메모리 위치를 다는 변수로 접근하는 방법
- 공용체 안에 있는 여러 변수들이 같은 메모리를 공유

```c
typedef union
{
  unsigned char value;
  struct
  {
    unsigned char b0 : 1;
    unsigned char b1 : 1;
    unsigned char b2 : 1;
    unsigned char b3 : 1;
    unsigned char b4 : 1;
    unsigned char b5 : 1;
    unsigned char b6 : 1;
    unsigned char b7 : 1;
  } bits;
} bitflags_t;
```
