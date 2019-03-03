
## 아이템14. Comparable을 구현할 지 고려하라
* CompareTo
	* Comparable 인터페이스의 유일무이한 메서드
	* Object의 메서드가 아님
	* vs equal
		* compareTo는 단순 동치성 비교에 더해 *순서*까지 비교
		* 제너릭
* Comparable
	* 구현했다는 것은 그 클래스의 인스턴스들에는 자연적인 순서가 있음을 의미
	* 검색, 극단값 계산, 자동 정렬되는 컬렉션 관리도 쉽게 가능
	* 자바 플랫폼

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTc4MjQ1NzMwXX0=
-->