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
	* **매서드 작성자가 그 메서드가 타입 안전함을 보장하는 장치**

* 자신의 제너릭 매개변수 배열의 참조를 노출하는 예
	* 메서드가 반환하는 배열의 타입은 이 메서드에 인수를 넘기는 컴파일타임에 결정되나, 그 시점에는 컴파일러에게 충분한 정보가 주어지지 않아 타입을 잘못 판단 할 수 있다
	* 따라서 자신의 varargs 매개변수 배열을 그대로 반환하면 힙 오염을 호출하는 쪽의 콜 스택으로까지 전이하는 결과
	* 컴파일은 되나 실행시 ClassCastException 
	* pickTwo의 반환값을 attributes에 저장하기 위해 String[]로 형변환하는 코드를 컴파일러가 자동을 생성!
	* Object[]가 String[]의 하위타입이 아니므로 이 형변환은 실패!
	* **제너릭 varargs	매개변수 배열에 다른 메서드가 접근하도록 허용하면 안전하지 않다**
	* 예외
		* @SafeVarargs로 제대로 애노테이트된 또 다른 varargs 메서드에 넘기는 것은 안전
		* 배열 내용의 일부 함수를 호출만 하는 (varargs를 받지 않는) 일반 메서드로 넘기는 것은 안전
```java
static <T> T[] toArray(T... args) {
	return args;
}

// 구체적예
static <T> T[] pickTwo(T a, T b, T c) {
	switch(ThreadLocalRandom.current().nextInt(3)) {
		case 0: return toArray(a, b);
		case 1: return toArray(a, c);
		case 2: return toArray(b, c);
	}
	throw new AssertionError();
}

// pickTwo 사용하는 main
public static void main(String[] args) {
	String[] attributes = pickTwo("좋은", "빠른", "저렴한");
}
```
* 제너릭 varargs 매개변수를 안전하게 사용하는 전형적인 예
```java
@SafeVarargs
static <T> List<T> flatten(List<? extend T>... lists) {
	List<T> result = new ArrayList<>();
	for (List<? extends T> list : lists)
		result.addAll(list);
	return result;
}
```
* @SafeVarargs 에너테이션을 사용해야할 때를 정하는 규칙
	* **제너릭이나 매개변수화 타입의 vargars 매개변수를 받는 모든 메서드에 @SafeVarargs를 달라**
	* 안전하지 않은 varargs 메서드는 절대 작성해서는 안된다
	* 안전한 조건!
		* varargs 매개변수 배열에 아무것도 저장하지 않는다
		* 그 배열(혹은 복제본)을 신뢰할 수 없는 코드에 노출하지 않는다
* 제너릭 varargs 매개변수를 List로 대체한 예
	* 타입 안전
	* 정적팩터리메서드 List.of를 활용하여 임의개수의 인수를 넘길 수 있다
	* 가능한 이유는 List.of에도 @SafeVarargs가 달려있기 떄문
	* 장점
		* @SafeVarargs을 직접 달지 않아도 됨
		* 실수도 안전하다고 판단할 걱정없음
	* 단점
		* 클라이언트 코드가 살짝 지저분해짐
		* 속도가 조금 느려질 수 있다
```java
static <T> List<T> flatten(List<List<? extends T>> lists) {
	List<T> result = new ArrayList<>();
	for (List<? extends T> list : lists)
		result.addAll(list);
	return result;
}
```

* Effective java 매거진
https://brunch.co.kr/@oemilk/202
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY0NzA4NTU2Miw4MTMwNTYxMzNdfQ==
-->