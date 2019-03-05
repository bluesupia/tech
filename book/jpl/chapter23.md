# System 클래스
* 시스템 상태를 조작하거나 시스템 자원을 조작하기 위한 정적 메소드를 제공
* 제공하는 기능
	* 표준 I/O 스트림
	* 시스템 속성 조작
	* 유틸리티와 현재의 Runtime 접근을 위한 편의 메소드
	* 보안
## 표준 I/O 스트림
* 표준 입력, 출력, 오류 스트림은 System 클래스의 정적 필드로 사용
	* public static final InputStream in
	* public static final OutputStream out
	* public static final PrintStream err
## 시스템 속성
* 시스템 환경을 정의
* Properties 객체에 저장
## 유틸리티 메소드

# 프로세스 생성
* 실행 중인 시스템은 여러 스레드를 실행할 수 있으며 자바 가상 머신을 탑재한 대부분의 시스템들도 여러 프로그램을 실행할 수 있다
* Runtime.exec
# 셧다운
# Runtime의 나머지 기능
# 보안

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIwNTI1NzE0OCwxMTcxNTA0MzU1XX0=
-->