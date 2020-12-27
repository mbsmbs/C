# 다중 포인터

## 이중 포인터
- 주소이 주소를 저장
- 2차원 자료
- 2D 배열을 더 많이 씀
- 메인 함수의 매개변수 argv

## 삼중 포인터
- 3 차원 자료

### 다중 포인터 예
```c
void swap(int** n1, int** n2)
{
  int* tmp = *n1;
  
  *n1 = *n2;
  *n2 = tmp;
}
```
```c
int num1 = 10;
int num2 = 20;

int* p;
iny* q;

p = &num1;
q = &num2;

swap(&p, &q);
```
