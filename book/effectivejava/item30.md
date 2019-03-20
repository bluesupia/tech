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
* 제너릭 싱글턴 팩터리 패턴 예, 항등함수(identity function) 구현
	* IDENTITY_FN을 UnaryOperator&lt;T&gt;로 형변환시 비검사 형변환 경고 발생
		* T가 어떤 타입이든 UnaryOperator&lt;T&gt;가 아니기 떄문
	* 항등함수란 입력값을 수정없이 그대로 반환하는 특별함수이므로, T가 어떤 타입이든 UnaryOperator&lt;T&gt;를 사용해도 타입이 안전
	* 따라서 경고를 숨겨도 안심!
```java
private static UnaryOperator<Object> IDENTITY_FN = (t) -> t;

@SuppressWarnings("unchecked")
public static <T> UnaryOperator<T> identityFunction() {
	return (UnaryOperator<T>) IDENTITY_FN;
}
```
* 재귀적 타입 한정
	* 드문경우이나, 자기 자신이 들어간 표현식을 사용하여 타입 매개변수의 허용 범위를 한정
	* 주로 타입의 자연적 순서를 정하는 Comparable 인터페이스와 함께 쓰임
```java
public static <E extends Comparable<E>> E max(Collection<E> c);

public static <E extends Comparable<E>> E max(Collection<E> c) {
	if (c.isEmpty())
		throw new IllegalArgumentException("컬렉션이 비어 있습니다.");
	E result = null;
	for (E e : c) 
		if (result == null ||
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU3MDg3NjkyMywyNDU4NjM2NjZdfQ==
-->