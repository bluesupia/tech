## 아이템36. 비트 필드 대신 EnumSet을 사용하라
* 비트 필드
	* 비트별 OR를 사용해 여러 상수를 하나의 집합으로 모음
	* 장점 ?
		* 비트별 연산을 사용해 합집합과 교집합 같은 집합 연산을 효율적으로 수행
	* 단점!
		* 정수 열거 상수의 단점 그대로..
		* 그대로 출력시 정수 열거 상수때 보다 해석이 더 어렵다
		* 비트 필드 하나에 녹아 있는 모든 원소 순회가 까다로움
* **EnumSet으로 대체!**
	* text.applyStyles(**EnumSet.of(Style.BOLD, Style.ITALIC)**);
```java
public class Text {
	public enum Style { BOLD, ITALIC, UNDERLINE, STRIKETHROUGH }

	public void applyStyles(Set<Style> styles) { ... }
}
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MzA0NDAwNjldfQ==
-->