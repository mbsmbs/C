# 오류 처리
- C에서는 예외처리를 지원하지 않음
- 버그와 오류는 다르다 :
  - 버그 : 컴파일중
  - 오류 : 실행중
- 오류를 처리해주는 함수/코드에서 오류가 있음을 알려줘야 함

## 전략
1. 기본적으로 내가 작성하는 모든 함수에 들어오는 데이터는 유효하다 가정하고 어서트를 많이 쓸 것
2. 그렇지 않은 함수는 매개변수나 함수 이름에서 그렇지 않다는 사실을 명백히 표시할 것
3. 오류 상황을 처리하는 장소는 최소한으로 할 것
4. 어떤 함수가 오류 처리를 한다는 사실을 반환형 등을 통해 확실히 보여줄 것