
## 아이템14. Comparable을 구현할 지 고려하라
* CompareTo
	* Comparable 인터페이스의 유일무이한 메서드
	* Object의 메서드가 아님
	* vs equal
		* compareTo는 단순 동치성 비교에 더해 *순서*까지 비교
		* 제너릭
* Comparable
	* Comparable 구현했다는 것은 그 클래스의 인스턴스들에는 자연적인 순서가 있음을 의미
	* 검색, 극단값 계산, 자동 정렬되는 컬렉션 관리도 쉽게 가능
	* 자바 플랫폼 라이브러리의 모든 값 클래스와 열거타입이 Comparable을 구현
	* 알파벳, 숫자, 연대 같이 순서가 명확한 값 클래스라면 Comparable 인터페이스를 구현하라!
* compareTo 규약
	* 반사성 : 두 객체 참조의 순서를 바꿔 비교해도 예상한 경과가 나와야한다
	* 대칭성 : 첫번째가 두번째보다 크고, 두번째가 세번째 보다 크면, 첫번째는 세번째 보다 커야한다
	* 추이성 : 크기가 같은 객체들끼리는 어떤 객체와 비교하더라도 항상 같아야 한다
	* compareTo 메서드로 수행한 동치성 테스트의 결과가 equals와 같아야 한다
		* 정렬된 컬렉션들은 동치성 비교시, equals 대신 compareTo를 사용하기 때문
			* new BigDecimal("1.0") 과 new BigDecimal("1.00") 
				* HashSet은 원소 두개를 가짐
				* TreeSet은 한개의 원소를 가짐
* compareTo 작성요령
	* equals 와 비슷
	* 차이점
		* 입력 인수의 타입을 확인하거나 형변환할 필요가 없다
			* Comparable은 타입을 인수로 받는 제너릭 인터페이스이므로 compareTo 메서드의 인수 타입은 컴파일 타임에 정해짐
		* null을 인수로 넣어 호출하면 NullPointerException을 던져야함
* 객체 참조 필드 비교
	* compareTo 메서드는 각 필드의 순서를 비교 (동치 비교가 아님)
	* 비교를 위해서는 compareTo 메서드를 재귀적으로 호출
	* Comparable을 구현하지 않은 필드나 표준이 아닌 순서로 비교해야 한다면 비교자(Comparator)를 대신 사용
	* 정수 기본 타입 필드를 비교시, 관계연사자를 사용하는 이전방식을 더이상 추천하지 않음
	* 핵심필드가 여러개 일때, 가장 핵심적인 필드부터 비교
		* 비교 결과가 0이 아니라면 결과를 바로 반환
		* 똑같지 않은 필드를 찾을 때까지 그다음으로 중요한 필드를 비교
		* 비교자 생성 메서드 (Comparator construction method)와 메서드 연쇄 방식으로 비교자를 생성하면 간결하나 성능저하
* Comparator
	* 수많은 보조 생성 메서드들 존재
	* comparingInt, thenComparingInt ...
	* 객체 참조용 비교자 생성메서드도 준비
		* comparing : 정적 메서드 2개가 다중정의
			* 첫번째 : 키 추출자를 받아서 그 키의 자연적 순서를 사용
			* 두번째 : 키 추출자 하나와 추출된 키를 비교할 비교자까지 총 2개의 인수를 받음
		* thenComparing : 정적 메서드 3개 다중정의
			* 첫번째 : 비교자 하나만 인수도 받아 그 비교자로 부처 순서를 정함
			* 두번째 : 키 추출자를 인수로 받아 그 키의 자연적 순서로 보조 순서를 정함
			* 세번째 : 키 추출자 하나와 추출된 키를 비교할 비교자까지 총 2개의 인수를 받음
* 주의!! '값의 차'를 기준으로 하는 비교자 
	* 추이성을 위해!
	* 정수 오버플로를 일으키거나 IEEE 754 부동소수점 계산 방식에 따른 오류를 낼 수 있다
	 ```java
	 static Comparator<Object> hashCodeOrder = new Comparator<>() {
		 public int compare(Object o1, Object o2) {
			 return o1.hashCode() - o2.hashCode();
		 }
	 }
	```
	* 대신 정적 compare 메서드를 활용한 비교자나 비교자 생성 메서드를 활용한 비교자를 사용하자!
	```java
	// 정적 compare 메서드 활용
	 static Comparator<Object> hashCodeOrder = new Comparator<>() {
		 public int compare(Object o1, Object o2) {
			 return Integer.compare(o1.hashCode() - o2.hashCode());
		 }
	 }
	```
	```java
	// 비교자 생성 메서드를 활용한 비교자
	 static Comparator<Object> hashCodeOrder = Comparator.comparingInt(o -> o.hashCode());
	```

### in my case
* Comparator와 Comparable이 항상 헷갈렸다. 
* 값의 차로 비교자 (Comparator)을 생성해서 사용한 적이 있었던 것 같다.

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzk5NDk0OTgzXX0=
-->