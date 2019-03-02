
## 아이템13. clone 재정의는 주의해서 진행하라
* Cloneable ?
	* 복제해도 되는 클래스임을 명시하는 용도의 믹스인 인터페이스이나 의도한 목적을 제대로 이루지 못함
	* 가장 큰 문제는, clone 메서드가 선언된 곳이 Cloneable이 아닌 Object 이고 그마저도 projected 
	* Cloneable 인터페이스가 하는일
		* Object의 protected 메서드인 clone 의 동작 방식을 결정
		* Cloneable 구현 클래스의 인스턴스에서 clon
		* e을 호출하면 그 객체의 필드들을 하나하나 복사한 객체를 반환
		* 그렇지 않은 클래스의 인스턴스에서 호출하면 CloneNotSupportedException
	* 상위클래스에 정의된 protected 메서드의 동작방식을 변경
	* 명세는 아니나, **Cloneable을 구현한 클래스는 clone메서드를 public으로 제공하며, 사용자는 복제가 제대로 이뤄지리라 기대**
* Cloneable 구현1
	*  제대로 동작하는 clone 메서드를 가진 상위클래스를 상속
	* 	공변 반환 타이핑(Covariant return typing)
	*  비검사 예외 (unchecked exception) 
```java
@Override
public PhoneNumber clone() {
	try {
		return (PhoneNumber) super.clone();
	} catch (CloneNotSupportedException e) {
		throw new AssertionError();
	}
}
```
* Cloneable 구현2
	* 가변 객체를 참조
	* super.clone 만 할 경우
		* size 필드는 올바른 값을 갖지만, elements 필드는 원본 Stack인스턴스와 똑같은 배열을 참조
	* **clone 메서드는 생성자와 같은 효과를 낸다**
		* **clone은 원본 객체에 아무런 해를 끼치지 않는 동시에 복제된 객체의 불변식을 보장** 해야함
	* elements.clone의 결과를 Object[]로 형변환 하지 않음
		* 배열의 clone은 런타임타입과 컴파일 타입 모두가 원본 배열과 똑같은 배열을 반환
		* 따라서 배열 복제시, clone 메서드 사용을 권장
		* *배열은 clone 기능을 제대로 사용하는 유일한 예*
	* elements 필드가 final이 였다면 ?
		* 아래 방식은 작동하지 않음
		* final 필드에는 새로운 값을 할당할 수 없기 때문에
		* **Cloneable 아키텍처는 '가변 객체를 참조하는 필드는 final로 선언하라'는 일반 용법과 충돌**
```java
public class Stack {
	private Object[] elements;
	private int size = 0;
	private static final int DEFAULT_INITIAL_CAPACITY = 16;
	...
	@Override
	public Stack clone() {
		try {
			Stack result = (Stack) super.clone();
			result.elements = elements.clone();
			return result;
		} catch (CloneNotSupportedException e) {
			throw new AssertionError();
		}
	}
}
```
* Cloneable 구현3
	* clone을 재귀적으로 호출하는 것만으로 불충분 할때
	* 해시테이블용 clone
```java
public class HashTable implements Cloneable {
	private Entry[] buckets = ...;
private static class Entry {

}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExODY3NzkwNDddfQ==
-->