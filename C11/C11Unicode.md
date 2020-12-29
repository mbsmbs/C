# 유니코드 지원

## 다국어 지원의 역사 3탄 UTF-16 / UTF-32 : 317
- char16_t
- char32_t

## UTF-8를 C11에서는 아직 지원하지 않음

## 다국어 지원의 기본 원칙
1. 사용자에게 보여주지 않을 문자열은 전부 아스키로 하자
2. 다국어 지원의 사용자에게 보여줄 문자열만 하자
3. 파일로 저장할 때는 공통된 인코딩이 좋다 (요즘 최선은 UTF-8)
4. 제일 편한 건 ICU 라이브러리 쓰는 것

## 최상의 시나리오 (C89 이상) : 318
- 사용자 환경을 무조건 UTF-8로 만든자
  - 사실상 회사에서 사용할 때만 가능
- 멀티 바이트 문자도 UTF-8 인코딩이니 그대로 저장
- 다시 파일을 읽어서 보여줄 때도 UTF-8이므로 그대로 멀티바이트로 문자로 출력
- 어느 방향도 변환 없음
- ...