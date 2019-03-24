## 아이템32. 제너릭과 가변인수를 함께 쓸 때는 신중하라

* 가변인수 (vargars) 메서드와 제너릭
	* 자바 5에 함께 추가되었으나 궁합이 맞지 않음
	* 가변인수 구현방식의 허점
		* 호출시 가변인수를 담기 위한 배열이 자동으로 하나 만들어짐
		* 내부로 감춰야했을 이 배열이 클라이언트에 노출
		* 그 결과 varargs	매개변수에 제너릭이나 매개변수화 타입이 포함되면 알기 어려운 컴파일 경고가 발생

* 제너릭과 vargars를 혼용하여 타입 안정성이 깨지는 예
	* **제너릭 vargargs 배열 매개변수에 값을 저장하는 것은 안전하지 않다**
```java
static void dangerous(List<String> ...stringLists) {
	List<Integer> intList = List.of(42);
	Object[] objects = stringLists;
	objects[0] = intList; // 힙 오염 발생!
	String s = stringLists[0].get(0); // ClassCastException
}
```

* @SafeVarargs 애너테이션
	* **매서드
https://brunch.co.kr/@oemilk/202
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIyNDQxNjQyNSw4MTMwNTYxMzNdfQ==
-->