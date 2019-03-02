
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
	* 가변 ㅏㅅㅇ태를 
	* Stack의 elements

```java
public class Stack {
	private Object[] elements;
	private int size = 0;
	private static final int DEFAULT_INITIAL_CAPACITY = 16;
...

}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzQzMTE1ODI3XX0=
-->