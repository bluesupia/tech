## 아이템28. 배열보다는 리스트를 사용하라
* 배열과 제너릭 타입의 차이
	* 공변과 불공변
		* 배열은 공변 (함께 변한다)
			* Sub가 Super의 하위 타입이라면 배열 Sub[]는 배열 Super[]의 하위 타입
		* 제너릭은 불공변
			* 서로 다른 타입 Type1, Type2가 있을 때 `List<Type1>`과 `List<Type2>`는 상 하위 타입이 아님
	* 실체화와 비실체화
		* 배열은 실체화(reify)
			* 런타임에도 자신이 담기로 한 원소의 타입을 인지하고 확인
		* 제너릭은 타입 정보가 런타임에는 소거(erasure)
			* 원소타입을 컴파일타임에만 검사하며 런타임에는 알수 없다
			* 소거
				* 제러닉이 지원되기 전의 레거시 코드와 제너릭 타입을 함께 사용할 수 있게 해주는 매커니즘
* 제너릭 배열 ? 
	* 타입이 안전하지 않기 떄문에 만들지 못함
	* 자동 생성한 형변환 코드에서 런타임에  ClassCastException이 발생할 수 있음
	* 제너릭 타입 시스템의 취지에 어긋남
* 실체화 불가 타입 (non-reifiable type)
	* E, List< E >. List< String > 
	* 실체화되지 않아서 런타임에는 컴파일타임보다 타입 정보를 적게 가지는 타입
*  매개변수화 타입 중 실체화 가능 타입
	* List < ? > ,  Map < ? > 과 같은 비한정적 와일드 카드 타입
	* 소거 매커니즘 때문에 
* 제너릭 배열 불가로 발생하는 단점
	* 제너릭 컬렉션에는 자신의 원소 타입을 담은 배열을 반환하는 게 보통은 불가능
	* 제너릭 타입과 가변인수 메서드를 함께 쓰면 해석하기 어려운 경고 메시지를 받음
		* @SafeVarargs 애너테이션으로 대처
* 배열로 형변환 시 제너릭 배열 생성 오류나 비검사 형변환 경고가 뜰 경우
	* 대부분은 배열 E[] 대신 List< E >를 사용하면 해결
	* 코드가 조금 복잡해지고 성능이 살짝 나빠지나,
	* 타입 안정성과 상호운용성은 좋아진다
* 제너릭을 도입해야하는 예
```java
public class Chooser {
	private final Object[] choiceArray;
	
	public Chooser(Collection choices) {
		choiceArray = choices.toArray();
	}

	public Object choose() {
		Random rnd = ThreadL젠ocalRandom.current();
		return choiceArray[rnd.nextInt(choiceArray.length)];
	}
}
```
* 제너릭 도입
```java
public class Chooser<T> {
	private final List<T> choiceList;
	
	public Chooser(Collection<T> choices) {
		choiceList = new ArrayList<>(choices);
	}

	public T choose() {
		Random rnd = ThreadL젠ocalRandom.current();
		return choiceList.get(rnd.nextInt(choiceList.size()));
	}
}
```

### 핵심정리
* 배열과 제너릭은 매우 다른 타입 규칙이 적용됨
	* 배열은 공변이고 실체화 됨
	* 제너릭은 불공변, 타입 정보가 소거
* 그 결과 배열은 런타임에는 타입이 안전하나 컴파일 타임에는 안전하지 않다
* 제너릭은 그 반대
* 따라서 둘은 함께 쓰기 쉽지 않다
* 둘이 함께 할 때 컴파일 오류
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMTI5MTk5MzFdfQ==
-->