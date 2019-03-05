# 아이템16. public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라
* **패키지 바깥에서 접근할 수 있는 클래스라면 접근자를 제공**
* **package-private 클래스 혹은 private 중첩 클래스라면 데이터 필드를 노출해도 문제 없음**
	* 클래스가 표현하는 추상 개념만 올바르게 표현하면 됨
	* 클래스 선언 면에서나 이를 사용하는 클라이언트 면에서나 접근자 방식보다 깔끔
* 자바 플랫폼 라이브러리에서 위 규칙을 어기는 사례 존재
	* java.awt.package의 Point와 Dimension
* public 클래스의 필드가 불변이라면 직접 노출할 때의 단점이 줄어들지만 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzA5MjYwMDAwXX0=
-->