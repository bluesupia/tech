## 아이템30. 이왕이면 제너릭 메서드로 만들라
* 제너릭 메서드
	* **(타입 매개변수들을 선언하는) 타입 매개변수 목록은 메서드의 제한자와 반환 타입 사이에 온다**
	* 타입 매개 변수의 명명 규칙은 제너릭 메서드나 제너릭 타입과 동일
	* 경고 없이 컴파일, 타입 안전, 쉬운 사용성
	* 아래 union 메서느는 집합3개(입력 2개, 반환 1개)의 타입이 모두 같아야 함
		* 한정적 와일드 카드 타입(아이템31)를 사용하면 더 유연하게 개선 가능
```java
public static <E> Set<E> union(Set<E> s1, Set<E> s2) {
	Set<E> result = new HashSet<>(s1);
	result.addAll(s2);
	return result;
}
```
* 제너릭 싱글턴 팩터리
	* 불변 객체를 여러 타입으로 활용할 수 있게 만들어야 할 때
	* 제너릭은 런타임에 타입 정보가 소거되므로 하나의 객체를 어떤 타입으로든 매개변수화 가능하나 이 때 요청한 타입 매개 변수에 맞게 매번 그 객체의 타입을 바꿔주는 정적 팩터리를 만들어야 함
	* ex,  Collections.reverseOrder 와 같은 함구 객체(아이템42)나 Collections.emptySet 같은 컬렉션용으로 사용
* 항등함수(identity function) 구현예
	* IDENTITY_FN을 UnaryOperator&lt;T&gt;로 형변환시 비검사 형변환 경고 발생
		* T가 어떤 타입이든 UnaryOperator&lt;T&gt;가 아니기 떄문
	* 항등함수란 입력값을 수정없이 그대로 반
```java
private static UnaryOperator<Object> IDENTITY_FN = (t) -> t;

@SuppressWarnings("unchecked")
public static <T> UnaryOperator<T> identityFunction() {
	return (UnaryOperator<T>) IDENTITY_FN;
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYxNDkzNDc4MSwyNDU4NjM2NjZdfQ==
-->