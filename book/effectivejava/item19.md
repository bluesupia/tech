## 아이템19. 상속을 고려해 설계하고 문서화하라. 그러지 않았다면 상속을 금지하라.
* 상속을 고려한 설계와 문서화란 ?
	* **상속용 클래스는 재정의할 수 있는 메서드들을 내부적으로 어떻게 이용하는지 문서로 남겨야 함**
		* implementation Requirements : 메서드의 내부 동작 방식을 설명하는 곳
	* **클래스의 내부 동작 과정 중간에 끼어들 수 있는 훅(hook)을 잘 선별하여 protected 메서드 형태로 공개해야 할 수도 있다**
		* 설계시 어떤 메서드를 protected로 노출해야할지 어떻게 결정 ?
			* 심/사/숙/고
			* **상속용 클래스를 시험하는 방법은 직접 하위 클래스를 만들어 보는 것이 유일**
	* **상속용 클래스의 생성자는 직접적으로든 간접적으로든 재정의 가능 메서드를 호출해서는 안된다**
```java
public class Super {
	// 잘못된 예! 생성자가 재정의 가능한 메서드를 호출
	public Super() {
		overrideMe();
	}
	
	public void overrideMe() {
	}
}
```
```java
public final class Sub extends Super {
	private final Instant instant;
	
	Sub() {
		instant = Instant.now();
	}

	// 재정의 가능 메서드, 상위 클래스의 생성자가 호출
	@Override
	public void overrideMe() {
		System.out.println(instant);
	}

	public static void main(String[] args) {
		Sub sub = new Sub();
		sub.overrideMe();
	}
}
```
* Clonable 과 Serializable 인터페이스의 상속용 설계
	* 둘 중 하나라도 구현한 클래스를 상속할 수 있게 설계하는 것은 일반적으로 좋지 않은 생각
	* 그 클래스를 확장하려는 프로그래머에게 엄청난 부담
	* **clone과 readObject 모두 직간접적으로 재정의 가능 메서드를 호출해서는 안됨**
		* readObject의 경우, 하위 클래스의 상태가 미처 다 역직렬화되기 전에 재정의한 메서드부터 호출
		* clone의 경우, 하위클래스의 clone 메서드가 복제본의 상태를 수정하기 전에 재정의한 메서드를 호출
	* Serializable을 구현한 상속용 클래스가 readResole나 writeReplace 메서드를 갖는다면 메서드들은 private가 아닌 protected로 선언해야함
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2MDk1OTIyMSw2NjE3NzI5NTBdfQ==
-->