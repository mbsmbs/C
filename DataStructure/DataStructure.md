# 자료구조 기초

## 1. 배열 : 243 ~ 246
- 여러 자료들을 메모리 덩어리 안에 줄줄이 세워놓은 구조
- 색인으로 각 요소에 접근
```c
enum {MAX_NUMS = 8};      // 배열의 요소수 정하기
enum {INVALID_INDEX = -1} // 실패시 반환 
int s_nums[MAX_NUMS];     // 요소수만큼 배열 선언
size_t s_num_count = 0;   // 얼마나 찼는지
```

### 배열의 삽입 O(n)
- 끝에 넣는 경우
- 끝이 아닌 경우 : 삽입하려는 위치의 요소부터 마지막 요소를 모두 뒤로 밀고 삽입
```c
void insert_at(size_t index, int n)     // 해당 index에 요소 n 삽입 함수
{
  size_t i;                             // 반복문에 사용할 변수 선언
  
  assert(index <= s_num_count);         // 배열안에 구멍이 생기지 않는지
  assert(s_num_count < MAX_NUMS);       // 배열의 범위를 넘는지
  
  for(i = s_num_count; i > index; --i)  // 삽입해야하는 위치에 이미 요소수가 있다면 
  {
    s_nums[i] = s_nums[i-1];            // 그 뒤에 요소들을 전부 하나씩 민다
  }
  
  s_nums[index] = n;                    // 삽입할 위치가 비었다면 해당 index에 삽입
  ++s_num_count;                        // 요소수 카운트 증가
}
```

### 배열의 삭제 O(n)
- 삭제하는 색인을 기준으로 그 뒤의 값들을 한 칸씩 모두 앞으로
```c
void remove_at(size_t index)              // index에 있는 요소 지우기 함수
{
  size_t i;                               // 반복문에 사용
  
  assert(index < s_num_count);            // index가 비어있는 공간을 가르키지는 않는지
  
  --s_num_count;                          // 요소수 카운트 감
  for(i = index; i < s_num_count; ++i)    // 이미 요소가 존재한다면
  {
    s_nums[i] = s_nums[i + 1];            // 그 뒤에 있는 요소들을 전부 앞으로
  }
  
}
```

### 배열의 검색 O(n)
- 배열의 요소들을 처음부터 차례대로 방문하여 확인
  - 성공 : 해당 색인 반환
  - 실패 : -1 반환
```c
size_t find_index(int n)              // n이 어디 있는지 찾는 함수
{
  size_t i;                           // 반복문
  
  for(i = 0; i < s_num_count; ++i)    // 배열의 처음부터 끝까지
  {
    if(s_nums[i] == n)                // n을 찾는다면
    {
      return i;                       // 해당 인덱스 반환
    }
  }
  
  return INVALID_INDEX;               // 못찾았다면 -1(enum) 반환
}
```
  
### 배열의 접근 O(1)
- 이미 색인을 알고 있다면 곧바로 접근
```c
s_nums[3] = 10;
```

### 순서가 정해지지 않은 배열에서 빠른 삭제
- 삭제한 요소 뒤의 모든 요소를 앞으로 이동하는 대신에 맨뒤에 있는 요소를 삭제한 위치로 옮겨온다.


## 2. 스택 : 247 ~ 251
- LIFO : Last in First out
- 배열로 구현
- 배열 앞에서부터 데이터 추가
- 마지막 요소의 위티를 스택의 제일 위
```c
enum {TRUE = 1, FALSE = 0};
enum {MAX_NUMS = 8};

int s_nums[MAX_NUMS];
size_t s_num_count = 0;       // 스택의 위치
```

### 스택의 삽입 O(1)
```c
void push(int n)
{
  assert(s_num_count < MAX_NUMS);     // 스택이 꽉 찼는지
  s_nums[s_num_count++] = n;          // 스택에 요소 삽입 후 다음 인덱스값으로
}
```

### 스택의 제거 O(1)
```c
int is_empty(void)
{
  return (s_num_count == 0)
}

int pop(void)
{
  assert(is_empty() == FALSE);      // 스택이 비어있는지
  
  return s_nums[--s_num_count];     // 안비었다면 맨 위에 값 반환하고 요소수 
}
```

### 스택의 검색 O(n)
- 위에서부터 찾아야 함
- 다른 빈 스택에 찾을 때까지 옮기다가 찾으면 다시 원상복구 O(2n)

### 스택의 용도
- 자료들의 순서를 뒤집을 때
- 컴퓨터연산 순서에 맞게 자료 재정리


## 3. 큐 : 252 ~ 255
- FIFO : First in First out
- 맨 앞의 요소에만 접근 가능
```c
enum {TRUE = 1, FALSE = 0};
enum {MAX_NUMS = 8};          // 큐의 요소수

int s_num[MAX_NUMS];          // 요소수만큼 배열로 큐 선언
size_t s_front = 0;           // 맨 앞
size_t s_back = 0;            // 맨 뒤
size_t s_num_count = 0;       // 현재 요소수
```

### 큐의 삽입 O(1)
```c
void enqueue(int n)                   // 맨뒤에 요소 넣시 함수
{
  assert(s_num_count < MAX_NUMS);     // 범위 안에 있는지
  
  s_nums[s_back] = n;                 // 맨뒤에 요소 삽입
  
  s_back = (s_back + 1) % MAX_NUMS;   // 맨뒤값 업데이트
  
  ++s_num_count;                      // 현재 요소수 증가
}
```

### 큐의 삭제 O(1)
```c
int is empty(void)
{
  return (s_num_count == 0);
}

int dequeue(void)                       // 큐에서 요소 삭제 함수
{
  int ret;                              // 반환값
  
  assert(is_empty() == FALSE);          // 큐가 비어있는지 
  
  ret = s_nums[s_front];                // 맨앞의 요소를 반환값으로
  
  --s_num_count;                        // 큐의 요소수 다운
  s_front = (s_front + 1) % MAX_NUMS;   // 다음 요소를 맨앞요소로 새로 정의
  
  return ret;                           // 반환
}
```

### 큐의 검색 O(n)
- 모든 요소 제거 후 원상복구 O(2n)

### 큐의 용도
- 대기줄이 필요한 경우
- 데이터 유입 속도가 데이터 소모 속도보다 빠른 경우


## 4. 연결 리스트
- 메모리에 산재해 있는 노드들을 연결시킨다
- 노드는 데이터와 다른 데이터의 주소를 가리킬 수 있는 포인터가 있다.

### 연결 리스트의 삽입 O(1)
- 이미 삽입할 위치를 알면 O(1)

### 연결 리스트의 삭제 O(1)
- 이미 삭제할 위치를 알면 O(1)

### 연결 리스트의 검색 O(n)
- 첫 노드부터 찾을 때까지

### 연결 리스트 전체를 출력 코드
```c
typedef struct node
{
  int value;
  node_t* next;
} node_t;

void print_node(const node_t* head)
{
  node_t* p;
  
  p = head;
  while (p != NULL)
  {
    printf("%d", p->value);
    p = p->next;
  }
}
```

### 연결리스트 해제 코드
```c
void destroy(node_t head)       // 연결리스트 해제 코드 
{
  node_t* p = head;             // head대체
  
  while(p != NULL)              // p가 NULL이 아닐때까지
  {
    node_t* next = p->next;     // 다음 노드를 가리키고
    free(p);                    // 지금 노드의 메모리를 해제하고
    p = next;                   // p 이동
  }
}

/* main */
node_t* head = NULL;            // head 노드 선언 & 초기화

/*codes...*/

destroy(head);                  // head메모리 해제
head = NULL;                    // 안전하게 head에 NULL대입
```

### 연결리스트 삽입 코드
```c
void insert_front(node** phead, int n)    // (head 포인터를 가리키는 phead, 요소 n)을 받고 맨 앞에 삽입하는 함수
{
  node_t* new_node;                       // 새노드를 동적할당할 노드 준비
  
  new_node = malloc(sizeof(node_t));      // 새노드를 동적 할당
  new_node->value = n;                    // 새노드의 데이터 삽입
  
  new_node->next = *phead;                // head가 원래 가리키던걸 새노드가 가리키게 한다
  *phead = new_node;                      // phead는 새로운 head를 가리키게 한다
}

/* main */

node_t* head = NULL;

insert_front(&head, 3);
insert_front(&head, 5);
insert_front(&head, 2);
insert_front(&head, 0);

destroy(head);
head = NULL;
```
