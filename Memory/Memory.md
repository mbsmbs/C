# 메모리의 종류
- 프로그램 실행 중에 여러 데이터가 공유하는 메모리는 크게 2로 나눔:
  - 스택 메모리
  - 힙 메모리
  - 데이터 섹션, 코드 섹션은 특정 코드 및 데이터용으로 고정
  
  
## 스택 메모리
- 함수가 반환하면 그 안에 있던 데이터가 다 날아감
- 보존 하려면 static을 사용해야 함
- 정적 메모리


## 레지스터
- cpu가 사용하는 저장 공간 중에 가장 빠른 저장 공간
- cpu가 연산을 할 때 보통 레지스터에 저장되어 있는 데이터를 사용
- 그 연산 결과도 레지스터에 다시 저장하는 것도 보통
- 메모리는 아님

### x86 아키텍쳐에서 사용하는 레지스터
- 8 general-purpose register : ESP, EBP, EAX, EBX, ECX, EDX..
- 6 segment register
- 1 flags register
- 1 instruction pointer
- etc...

### register
- register 자료형 변수명;
- 레지스터 사용 요청
- 컴파일러가 사용할 지 말지 결정
- 메모리가 아님


## 힙 메모리
- 범용적 메모리
- 자동적으로 메모리 관리를 안 해줌
- 사용(할당)하고 반납(해제)해야함
- 사용할 수 있는 용량이 크다
- 프로그램 직접 수명 제어
- 해제를 안하면 누구도 그 메모리를 쓸 수 없음
- 빌려간 쪽에서 메모리 주소를 잃어버리면 메모리 누수
- 스택에 비해 느리다
- 동적 메모리