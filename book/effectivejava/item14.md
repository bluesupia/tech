
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
	* Comparable은 타입을 인수로 받는 제너릭 인터페이스이므로 채
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMjE5ODg1OF19
-->