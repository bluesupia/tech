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
* Runtime.exec으로 새로운 프로그램을 실행
* exec 호출 성공시, 현재 process 상에서 실행 중인 프로그램을 나타내는 새로운 Process 생성
* Process 객체 
	* 진행을 제어하기 위해 프로세스 상태를 질의하고 메소드를 호출하는 용도로 사용
	* 추상 클래스로서 이 클래스의 서브클래스는 각각에 시스템에 맞게 동장할 수 있는 추상 메소드 정의
# 셧다운
# Runtime의 나머지 기능
# 보안

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjk2MDk3MzEwLDExNzE1MDQzNTVdfQ==
-->