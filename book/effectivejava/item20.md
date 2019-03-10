## 아이템20. 추상 클래스보다는 인터페이스를 우선하라
* 자바가 제공하는 다중 구현 매커니즘
	* 인터페이스
	* 추상클래스
	* 공통점
		* 인스턴스 메서드를 구현 형태로 제공
	* 차이점
		* 추상클래스가 정의한 타입을 구현하는 클래스는 반드시 추상클래스의 하위 클래스가 됨
		* 인터페이스가 선언한 메서드를 모두 정의하고 그 일반 규약을 잘 지킨 클래스라며 어떤 클래스를 상속했든 같은 타입으로 취급
* 인터페이스의 장점
	* **기존 클래스에도 손쉽게 새로운 인터페이스를 구현해 넣을 수 있음**
		* ex. Comparable, Iterable, AutoClosable
		* 반면 기존 클래스 위에 새로운 추상 클래스를 끼워넣기는 어려운게 일반적
	* **믹스인(mixin) 정의에 안성맞춤**
		* 믹스인이란, 클래스가 구현할 수 있는 타입으로, 믹스인 클래스에 원래의 '주된 타입' 외에도 특정 선택적 행위를 제공한다고 선언하는 효과
		* ex. Comporable
	* **계층구조가 없는 타입 프레임워크를 만들 수 있다**
	* 래퍼 클래스 관용구 사용시,**기능을 향상시키는 안전하고 강력한 수단**이 된다
		* 타입을 추상 클래스로 정의해두면 기능 추가 방법은 상속뿐
		* 상속해서 만든 클래스는 래퍼 클래스보다 활용도가 떨어지고 깨지기는 더 쉽다
	* 메서드 중 구현방법이 명백한 것이 있다면 디폴트 메서드로 제공해 개발자의 일감을 덜어줌
* 인터페이스의 제약
		* 디폴드 메서드 제약
			* equals, hashCode와 같은 Object 메서드는 제공해서는 안됨
		* 인스턴스 필드를 가질 수 없고 public이 아닌 정적 멤버도 가질 수 없다
		* 만들지 않은 인터페이스에는 디폴트 메서드를 추가할 수 없다
* 템플릿 메서드 패턴
	* 인터페이스와 추상 골격 구현(skeletal implement) 클래스를 함께 제공하는 방법
	* 인터페이스로는 타입을 정의하고 필요에 따라 디폴트 메서드 제공
	* 골격 구현 클래스는 나머지 메서드들 구현
	* 관례상 이름은 Abstract*Interface*
* 골격 구현을 사용해 완성한 구체 클래스
	* 추상클래스처럼 구현을 도와주는 동시에 추상 클래스로 타입을 정의할 때 따라오는 심각한 제약에서 자유롭다
```java
static List<Integer> intArrayAsList(int[] a) {
	Objects.requireNonNull(a);
	return new AbstractList<>() {
		@Override
		public Integer get(int i) {
			return a[i];
		}

		@Override
		public Integer set(int i, Integer val) {
			int oldVal = a[i];
			a[i] = val;
			return oldVal;
		}

		@Override
		public int size() {
			return a.length
		}
	}
}
```
* 골격 구현 작성 방법
	* 인터페이스를 살펴 기반 메서드들을 선정
		* 골격 구현에서는 추상 메서드
	* 기반 메서드들을 사용해 직접 구현할 수 있는 메서드들을 모두 디폴트 메서드로 제공
		* equals, hashCode 와 같은 Object의 메서드는 디폴트 메서드로 제공하면 안된다
	* 인터페이스를 구현하는 골격 구현 클래스를 만들어 남은 메서드들을 작성
* Map.Entry 인터페이스
	* 기반 메서드 : getKey, getValue
	* 선택적으로 setValue도 포함할 수 있다
	* equals, hashCode의 동작 방식 정의
* **단순 구현(Simple implementation)**
	*  골격 구현의 작은 변종 
	* AbstractMap.SimpleEntry
	* 골격 구현과 같이 상속을 위해 인터페이스를 구현
	* 추상 클래스가 아니다
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMjg0MTgwODMsMTA0ODY4NzM2N119
-->