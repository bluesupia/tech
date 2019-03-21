## 아이템31. 한정적 와일드카드를 사용해 API 유연성을 높이라

* 매개변수화 타입
	* 불공변
	* List&lt;String&gt;은 List&lt;Object&gt;의 하위타입이 될 수 없다
		* 리스코프 치환 원칙([아이템10](./item1~11.md))에 어긋
	* 불공변 방식보다 유연한 무언가 필요
	* 아이템29 stack의 pushAll
		* 와일드카드 타입을 사용하지 않는 메서드 - 결함이 있다!
			* 깨끗하게 컴파일되지만 완벽하지않다!
			* Iterable src의 원소타입이 스택의 원소타입과 일치하면 잘 동작
			* Stack&lg
```java
public void pushAll(Iterable<E> src) {
	for (E e: src) 
		push(e);
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAxNjQ0ODgzNV19
-->