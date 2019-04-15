## 아이템44. 표준 함수형 인터페이스를 사용하라
* 템플릿 메서드 패턴의 현대적 해법
	* 같은효과의 함수객체를 받는 정적 팩터리나 생성자 제공
	* 이때 함수형 매개변수 타입을 올바르게 선택
```java
protected boolean removeEldestEntry(Map.Entry<K,V> eldest) { 
	return size() > 100;
}

// 불필요한 함수형 인터페이스 : 대신 표준 함수형 인터페이스를 사용하라!
@FuntionalInterface
interface EldestEntryRemoveFunction<K,V> {
	boolean remove(Map<K,V> map, Map.Entry<K,V> eldest);
}
```
* 자바 표준 라이브러리에 이미 같은 모양의 인터페이스가 존재
* ** 필요한 용도에 맞는게 있다면, 직접 구현하지 말고 표준 함수형 인터페이스를 활용**
* 기본 함수형 인터페이스


| 인터페이스 | 함수 시그니처 | 예|
|--|--|--|
|UnaryOperator&lt;T&gt;  |T apply(T t)  |String::toLowerCase |
|BinaryOperator&lt;T&gt;  |T apply(T t1, T t2)  |BigInteger::add |
|Predicate&lt;T&gt;  |boolean apply(T t)  |Collection::isEmpty |
|Func&lt;T&gt;  |R apply(T t)  |Arrays::asList |
|Supplier&lt;T&gt;  |T get(T t)  |Instant::now |
|Consumer&lt;T&gt;  |void accept(T t)  |System.out::println |

* 기본 함수형 인터페이스의 변형
	* 기본 타입인 int, long, double용으로 3개씩 변형
		* I
	* Function 인터페이스에는 기본 타입을 반환하는 변형이 총 9개 더 존재
	* 기본  함수형 인터페이스 중 3개에는 인수를 2개씩 받는 변형
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMTA0MDUwNjhdfQ==
-->