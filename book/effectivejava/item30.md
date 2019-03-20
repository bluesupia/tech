## 아이템30. 이왕이면 제너릭 메서드로 만들라
* 제너릭 메서드
	* **(타입 매개변수들을 선언하는) 타입 매개변수 목록은 메서드의 제한자와 반환 타입 사이에 온다**
	* 타입 매개 변수의 명명 규칙은 제너릭 메서드나 제너릭 타입과 동일
	* 경고 없이 컴파일, 타입 안전, 쉬운 사용성
	* 아래 union 메서느는 집합3개(입력 2개, 반환 1개)의 타입이 ㅁㅗ두 같아야 함
		* 한정적 
```java
public static <E> Set<E> union(Set<E> s1, Set<E> s2) {
	Set<E> result = new HashSet<>(s1);
	result.addAll(s2);
	return result;
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc2MTIyNDYxNywyNDU4NjM2NjZdfQ==
-->