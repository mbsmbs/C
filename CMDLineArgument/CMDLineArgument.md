# 커맨드 라인 인자
- 커맨드 라인에서 프로그램 실핼할 때 인자들을 넣엊주는 방법
- >> filecopy.exe a.txt b.txt
- 이렇게 들어온 인자들을 main함수에서 읽어올 수 있음
```c
int main(int argc, const char* argv[])
{
}
```
- argc : 들어온 인자의 수
- argv는 char 포인터 배열

### 입출력 리디렉션하고는 다르다.
>> a.exe < hello.txt
- 여기서 hello.txt는 커맨드라인 인자가 아니다.
