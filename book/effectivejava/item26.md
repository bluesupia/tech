## 아이템26. 로 타입은 사용하지 말라

* 제너릭 타입 (Generic type)
	* 클래스와 인터페이스 선언에 타입 매개변수가 쓰이면, 제너릭 클래스 혹은 제너릭 인터페이스
	* List 원소의 타입을 나타내는 타입 매개변수 E
		* 인터페이스의 완전한 이름은 List<E>
		* 짧게 List

* 매개변수화 타입 (parameterized type)
	* 먼저 클래스 (or 인터페이스) 이름
	* 이어서 꺾쇠괄호 안에 실제 타입 매개변수 나열
	* ex. List<String>
		* 원소의 타입이 String인 리스트를 뜻하는 매개변수화 타입
		* 정규타입 매개변수 E에 해당하는 실제 타입 매개변수

* **로 타입 (raw type)**
	* 제너릭 타입을 정의하면 로 타입(raw type)도 함께 정의
		* 로 타입 : 제러닉 타입에서 타입 매개변수를 전혀 사용하지 않을 때
			* ex. List<E>의 로 타입은 List
		* 타입 선언에서 제너릭 타입 정보가 전부 지워진 것처럼 동작
		* 제너릭 도래 전 코드와 호환하기 위한 궁여지책
	* 로 타입시 아무 오류 없이 컴파일이 되고 실행된다 (경고 메시지는 보여줄 수 있다)
	* *오류는 가능한 한 발생 즉시, 이상적으로는 컴파일 할 때 발견하는 것이 좋다!*
	* 제너릭 사용시 타입 정보가 선언 자체에 녹아듬
	* **로 타입을 쓰면 제너릭이 안겨주는 안전성과 표현력을 모두 잃게 됨**
* 임의 객체를 허용하는 매개변수화 타입은 사용가능 
	* ex. List<Object>
	* runtime에서 실패! (unsafe 메서드가 로 타입을 사용)
```java
public static void main(String[] args) {
	List<String> strings = new ArrayList<>();
	unsafeAdd(strings, Integer.valueOf(42));
	String s = strings.get(0);
}

private static void unsafeAdd(List list, Object o) {
	list.add(o);
}
```
	* unsafe 메서드가 매개변수화 타입 List<Object>사용시
		* 컴파일에서 실패
* 원소의 타입을 몰라도 되는 로 타입을 사용하고자 할때
	* 동작은 하지만 로 타입을 사용해 안전하지 않다
	* 비한정적 와일드카드 타입을 대신 사용
		* 제너릭 타입을 쓰고 싶지만 실제 타입 매개변수가 무엇인지 신경 쓰고 싶지 않을 때 물음표(?)
		* ex. 제너릭 타입 Set<E>	의 비한정적 와일드 카드 타입은 Set<?>
		* 어떤 타입이라도 담을 수 있는 가장 범용적인 매개변수화 Set 타입
		* 비한정적 와일드카드 타입인 Set<?> 과 로 타입인 Set의 차이
			* 와일드카드 타입은 안전하고 로 타입은 안전하지 않다
			* 로 타입 컬렉션에는 아무 원소나 넣을 수 있으니 타입 불변식이 훼손되기 쉬움
			* **Collection<?>에는 (null외에는) 어떤 원소도 넣을 수 없다**
```java
static int numElementsInCommon(Set s1, Set s2) {
	int result = 0;
	for (Object o1 : s1) 
		if (s2.contains(o1))
			result++;
	return result;
}
```
* 로 타입을 쓰는 예외
	* **class 리터럴에는 로타입을 써야함**
		* List.class, String[].class, int.class 허용
		* List<String>.class, List<?>.class 허용 안함
	* instanceof 연산자
```java
if (o instanceof Set) { // 로 타입
	Set<?> s = (Set<?>) o; // 와일드카드 타입
}
```

|한글용어  |영문영어  |예  |아이템  |
|--|--|--|--|
|매개변수화 타입  |parameterized type|  |  |
|실제 타입 매개변수|actual type parameter|  |  |
|재너릭 타입  |generic type |  |  |
|정규 타입 매개변수|formal type parameter|  |  |
|비한정적 와일드카드 타입|unbounded |  |  |
|로 타입|  |  |  |
|한정적 타입 매개변수 |  |  |  |
|재귀적 타입 한정|  |  |  |
|한정적 와일드카드 타입|  |  |  |
|제너릭 메서드|  |  |  |
|타입 토큰|  |  |  |

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4ODcyMzEyMzIsODgxMjQ4Mzk3XX0=
-->