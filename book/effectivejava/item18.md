## 아이템18. 상속보다는 컴포지션을 사용하라
* 상속
	* 코드 재사용의 강력한 수단이나 항상 최선은 아님
	* 잘못 사용시 오류를 내기 쉬운 SW
	* 상위 클래스와 하위 클래스를 모두 같은 프로그래머가 통제하는 패키지 안이나 확장할 목적으로 설계되고 문서화도 잘 된 클래스이면 안전
	* 일반적인 구체 클래스는 패키지 경계를 넘어 상속하는 경우는 위험
	* **캡슐화를 깨뜨린다**
		* 상위 클래스가 어떻게 구현되느냐에 따라 하위 클래스의 동작에 이상이 생길 수 있음
		* 구체적 예. HashSet 처음 생성된 이후 원소가 몇개 더해 졌는지 ?
			* 결과는 6을 반환
			* addAll은 각 원소를 add 메서드를 호출해 추가하므로..
			* 해결방법 ?
				* addAll 메서드를 재정의지 않거나 다른 식으로 재정의
				* 다른 오류들이 발생가능
```java
public class InstrumentedHashSet<E> extends HashSet<E> {
	// 추가된 원소의 수
	private int addCount = 0;
	public InstrumentedHashSet(int initCap, float loadFactor) {
		super(initCap, loadFactor);
	}
	
	@Override
	public boolean add(E e) {
		addCount++;
		return super.add(e);
	}

	@Override
	public boolean addAll(Collection<? extends E> c) {
		addCount += c.size();
		return super.addAll(c);
	}
	 
	public int getAddCount() {
		return addCount;
	}
}
```
```java
InstrumentedHashSet<String> s = new InstrumentedHashSet<>();
s.addAll(List.of("틱", "택", "토"));
```
		
* 컴포지션 (Composition, 구성)
	* 기존클래스를 확장하는 대신, 새로운 클래스를 만들고 private 필드로 기존 클래스의 인스턴스를 참조

* 전달 (forwarding)
	* 새 클래스의 인스턴스 메서드들을 기존 클래스의 대응하는 메서드를 호출해서 그 결과를 반환
	* 전달 메서드 : 새 클래스의 메서드들
	*
* 상속대신 컴퍼지션을 사용한 InstrumentedSet
```java
public class InstrumentedSet<E> extends ForwardingSet<E> {
	private int addCount = 0;
	
	public InstrumentedSet(Set<E> s) {
		super(s);
	}

	@Override
	public boolean add(E e) {
		addCount++;
		return super.add(e)
	}
	
	@Override
	public boolean addAll(Collector<? extends E> c) {
		addCount += c.size();
		return super.addAll(c);
	}

	public int getAddCount() {
		return addCount;
	}
}
```
* 재사용할 수 있는 전달클래스
```java
public class ForwardingSet<E> implements Set<E> {
	private final Set<E> s;
	public ForwardingSet(Set<E> s) {
		this.s = s;
	}

	public void clear() { s.clear(); }
	...
}
```
* 래퍼클래스
	* 다른 인스턴스를 감싸고 있다는 뜻에서 래퍼 클래스
	* 위 InstrumentedSet
	* 다른 Set에 계측 기능을 덧씌운다는 뜻에서 데코레이터 패던이라고도 함
	* 컴포지션과 전달의 조합은 넒은 의미로 위임(delegation)
* 래퍼클래스의 단점
	* 콜백 프레임워크와 어울리지 않음
* 컴포지션 대신 상속을 쓰려면 자문하라!
	* 확장하려는 클래스의 API에 아무런 결함이 없는가?
	* 결함이 있다면, 하위 클래스의 API까지 전파되어도 괜찮은가?
	* 컴포지션으로는 이런 결함을 숨기는 새로운 API를 설계할 수 있으나 상속은 결함까지도 승계!

## 핵심정리
* 상속은 강력하나 캡슐화를 해치는 문제가 있다
* 상속은 순수 is-a 관계 일 때만 써야함
* is-a 관계더라도 주의
	* 하위 클래스의 패키지와 상위 클래스가 다르고, 상위 클래스의 확장을 고려하여 설계되지 않았다면 문제
* 상속의 추약점을 피하려면 대신 컴포지션과 전달을 사용
	* 래퍼 클래스로 구현할 적당한 인터페이스가 있다면 더욱더 사용!
	* 하위 클래스보다 견고하고 강력
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU1OTE1NTE3XX0=
-->