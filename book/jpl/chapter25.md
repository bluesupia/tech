

# 25. 표준패키지

# java.awt
* 플랫폼에 독립적인 그래픽 사용자 인터페이스를 작성할 수 있게 해주는 추상 윈도우 툴킷
* in my proj
	* BMT시 그래프

# java.math
* 수학 연산, 현재 이 패키지는 임의 정밀도 연산을 다루는 ~~세개~~ 클래스만 포함
	* 현재 6개 클래스 존재
* in my proj
	* BigInteger

# java.net
* 소켓, URL 등과 관련된 네트워크 클래스
* in my proj
	* HttpURLConnection, InetAddress, URI, URLEncoder 등등

# java.security와 관련 패키지
* 보안 기능과 관련된 유용한 도구들을 포함
* 디지털 서명, 메시지 다이제스트, 키관리, 암호키
* 하위 패키지
	* 인증 (java.security.cert)
	* RSA
	* DSA
	* 키와 알고리즘 매개변수 명세(java.security.spec)에 대한 추상 개념 정의
* 관련패키지
	* javax.crypto 암호문을 위해 많은 매커니즘 제공
* 보안 아키텍처는 보안 상요 작용의 추상화 제공
	* 암호화와 인증 같은 것들은 접근하는 방법은 많으며 앞으로 더 많이질 것으기 때문
	* providers는 이 추상화의 구현을 제공
	* 각각으
# java.sql
# 유틸리티 하위패키지
## 병렬 유틸리티-java.unil.concurrent
# javax.* - 표준확장

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTcyNjQ4NzY5M119
-->