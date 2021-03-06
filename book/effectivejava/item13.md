
## 아이템13. clone 재정의는 주의해서 진행하라
* Cloneable ?
	* 복제해도 되는 클래스임을 명시하는 용도의 믹스인 인터페이스이나 의도한 목적을 제대로 이루지 못함
	* 가장 큰 문제는, clone 메서드가 선언된 곳이 Cloneable이 아닌 Object 이고 그마저도 projected 
	* Cloneable 인터페이스가 하는일
		* Object의 protected 메서드인 clone 의 동작 방식을 결정
		* Cloneable 구현 클래스의 인스턴스에서 clone을 호출하면 그 객체의 필드들을 하나하나 복사한 객체를 반환
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
* Cloneable 구현3-1
	* clone을 재귀적으로 호출하는 것만으로 불충분 할때
	* 해시테이블용 clone
	* 복제본은 자신만의 버킷배열을 갖지만, 이 배열은 원본과 같은 연결리스트를 참조하여 오류 발생 가능
```java
public class HashTable implements Cloneable {
	private Entry[] buckets = ...;
	private static class Entry {
		final Object key;
		Object value;
		Entry next;
		Entry(Object key, Object value, Entry next) {
			this.key = key;
			this.value = value;
			this.next = next;
		}
	}
	...
	// ERROR!!
	@Override
	public HashTable clone() {
		try {
			HashTable result = (HashTable) super.clone();
			result.buckets = buckets.clone();
			return result;
		} catch (CloneNotSupportedException e) {
			throw new AssertionError();
		}
	}
}
```
* Cloneable 구현3-2
	* 깊은복사(deep copy)를 지원하도록 보강
	* 리스트가 길면 스택 오버플로를 일으킬 위험이 있다
```java
public class HashTable implements Cloneable {
	private Entry[] buckets = ...;
	private static class Entry {
		final Object key;
		Object value;
		Entry next;
		Entry(Object key, Object value, Entry next) {
			this.key = key;
			this.value = value;
			this.next = next;
		}
		// 재귀적 복사
		Entry deepCopy() {
			return new Entry(key, value, next == null ? null : next.deepCopy());
		}
	}
	...
	@Override
	public HashTable clone() {
		try {
			HashTable result = (HashTable) super.clone();
			result.buckets = new Entry[buckets.length];
			for (int i = 0; i < buckets.length; i++) {
				if (buckets[i] != null) {
					result.buckets[i] = buckets[i].deepCopy();
				}
			}
			return result;
		} catch (CloneNotSupportedException e) {
			throw new AssertionError();
		}
	}
}
```
* Cloneable 구현3-3
	* 깊은복사(deep copy)를 지원하도록 보강
	* 재귀 호출 대신 반복자를 써서 순회하도록 수정
```java
Entry deepCopy() {
	Entry result = new Entry(key, value, next);
	for(Entry p = result; p.next!=null; p = p.next) {
		p.next = new Entry(p.next.key, p.next.value, p.next.next);
	}
	result result'
}
```
* Cloneable 구현3-4
	* super.clone을 호출하여 얻은 객체의 모든 필드를 초기 상태로 설정
	* 원본 객체의 상태를 다시 생성하는 고수준 메서드들을 호출
	* HashTable
		* buckets필드를 새로운 버킷배열로 초기
		* 원본데이터의 담긴 모든 키-값 쌍 각각에 대해 복제본 테이블의 put(key, value) 메서드를 호출
	* 간단하고 우아하나, 느리다
* clone!
	* 재정의될 수 있는 메서드를 호출하지 않아야함 (생성자도 동일함)
	* CloneNotSupportedException을 던진다고 선언했으나 재정의한 메서드를 그렇지 않음
		* **public인 clone메서드에서는 throw절을 없애야함**
* 주의!
	* 상속용 클래스는 Cloneable을 구현해서는 안됨
	* Cloneable을 구현한 스레드 안전클래스를 작성시, clone 메서드 역시 적절히 동기화 필요

* 이런데도 써야하나 ?
	* **복사 생성자와 복사 팩터리**
```java
public Yum(Yum yum) {...};

public static Yum newInstance(Yum yum) {..};
```

## 핵심정리
* Cloneable을 구현하는 모든 클래스는 clone을 재정의 해야한다
* 접근 제한자는 public으로 반환 타입은 클래스자신으로 변경
* 가장 먼저 super.clone 호출 후 필요한 필드를 적절히 수정
	* 객체 내부 '깊은 구조' 숨어있는 가변 객체를 복사하고 복제본이 가진 객체 참조 모두가 복제본을 가리키게 함
	* 내부복사는 clone을 재귀적으로 호출해서 구현하나 최선은 아님
* 새로운 인터페이스를 만들 때 절대 Cloneable을 확장하지 말고, 새로운 클래스도 이를 구현하지 않음
* final 클래스라면 위험이 크지 않으나, 성능 최적화 관점에서 검토후 드물게 허용
* 복제 기능은 생성자와 팩터리를 이용하는게 최고
* 배열은 clone메서드 방식이 가장 깔끔한, 규칙의 합당한 예외
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTUzNjU5MzgyXX0=
-->