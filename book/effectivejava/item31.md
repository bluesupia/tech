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
			* Stack&lt;Number&gt;로 선언 후 pushAll(intVal) 호출시 오류발생!
				* 매개변수화 타입이 불공변이기 때문!!
		*  해결책
			* 한정적 와일드카드 타입 사용
			* **유연성을 극대화하려면 원소의 생산자나 소비자용 입력 매개변수에 와일드카드 타입을 사용**
```java
public void pushAll(Iterable<E> src) {
	for (E e: src) 
		push(e);
}
```
```java
public void pushAll(Iterable<? extends E> src) {
	for (E e: src) 
		push(e);
}
```
* **펙스(PECS)**
	* producer-extends, consumer-super
	* 매개변수T가 생산자라면 &lt;? extends T&gt;
	* 매개변수T가 소비자라면 &lt;? super T&gt;
	* 와일드카드 타입을 사용하는 기본 원칙
	* 겟풋원칙(Get and Put Principle)
* **클래스 사용자가 와일드카드 타입을 신경써야 한다면 그 API엔 결함 가능성이 크다**
* 명시적 타입인수
	* 자바 7까지는 타입추론 능력이 충분하지 못해 문맥에 맞는 반환 타입(혹 목표 타입)을 명시
	* 목표 타이핑은 자바 8부터 지원시작
```java
Set<Number> numbers = Union.<Number>union(integers, doubles);
```
* 와일드카드 필요 예- max(item30-7)
	* 매개변수 목록
		* E 인스턴스를 생성하므로 extends!
	* 타입 매개변수
		* 원래 선언은 E가 Comparable&lt;E&gt; 확장한다고 정의
		* 이때 Comparable&lt;E&gt;는 E인스턴스를 소비
		* 그러므로 super!
	* Comparable은 언제나 소비자이므로, 일반적으로 **`Comparable<E>`보다는 `Comparable<? super E>`**
	* Comparator도 동일
	* 복잡한 선언이나 오직 수정된 max에서만 처리할 수 있는 예가 발생!
		* Comparable(혹은 Comparator)을 직접 구현하지 않고, 직접 구현한 다른 타입을 확장한 타입을 지원하기 위해 와일드카드 필요!
```java
public static <E extends Comparable<E>> E max(List<E> list)

public static <E extends Comparable<? super E>> E max(List<? extends E> list)
```
* 와일드카드 필요 예- swap
	* 타입 매개변수와 와일드카드에는 공통부분이 있어 메서드를 정의할 때 둘 중 어느것을 사용해도 괜찮을 때 어느 방법으로 ?
	* public API라면 두번째
	* **메서드 선언에 타입 매개변수가 한 번만 나오면 와일드카드로 대체**
	* 두번째 선언의 문제
		* 구현 코드의 컴파일 오류
		* List&lt;?&gt;에는 null외에는 어떤 값도 넣을 수 없다
		* 와일드카드 타입의 실제 타입을 알려주는 메서드를 private 도움 메서드로 따로 작성
```java
public static <E> void swap(List<E> list, int i, int j);
public static void swap(List<?> list, int i, int j);
```
```java
public static void swap(List<?> list, int i, int j) {
	swapHelper(list, i, j);
}
// 와일드카드 타입을 실제 타입으로 바꿔주는 private 도우미 메서드
private static <E> void swapHelper(List<E> list, int i, int j) {
	list.set(i, list.set(j, list.get(i)));
}
```

### 핵심정리
* 조금 복잡하더라도 와일드카드 타입을 적용하면 API는 훨씬 유연
* 널리 쓰일 라이브러리 작성시엔 반드시 와일드카드 타입을 적절히 사용해줘야 함
	* PECS 공식을 기억!
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU2MDMwMjIwNSwtMTQ3NjEzNzQyXX0=
-->