## 아이템34. int 상수 대신 열거 타입을 사용하라

* 정수 열거 패턴
	* 타입 안전을 보장할 방법이 없고 표현력이 좋지 않다
	* 정수 상수는 문자열로 출력하기가 다소 까다롭다
	* 출력하거나 디버거로 살펴보면 (의미가 아닌) 단지 숫자로만 보여서 도움이 되지 않는다
* 문자열 열거 패턴
	* 문자열 상수를 사용하는 변형 패턴
	* 상수의 의미를 출력할 수 있다
	* 하드코딩한 문자열에 오타가 있어도 컴파일러는 확인할 길이 없으니 런타임 버그가 생긴다
	* 문자열 비교에 따른 성능 저하 
* 열거타입
	* 자바의 열거 타입은 완전한 형태의 클래스
	* 강력하다
	* 아이디어
		* 열거 타입 자체는 클래스
		* 상수 하나당 자신의 인스턴스를 하나씩 만들어 public static final 필드로 공개 
		* 열거 타입은 밖에서 접근할 수 있는 생성자를 제공하지 않으므로 사실상 final
		* 열거 타입은 인스턴스 통제
			* 클라이언트가 인스턴스를 직접 생성하거나 확장할 수 없으므로 열거 타입 선언으로 만들어진 인스턴스들은 딱 하나씩만 존재함이 보장
		* 열거 타입은 싱글턴을 일반화한 형태
	* 컴파일타임 타입 안정성을 제공
* 데이터와 메서드를 갖는 열거 타입
	* **열거 타입 상수 각각을 특정 데이터와 연결지으려면 생서자에서 데이터를 받아 인스턴스 필드에 저장**
* 상수별 메서드 구현
	* 열거타입에 추상 메서드를 선언하고 각 상수별 클래스 몸체, 즉 각 상수에서 자신에 맞게 재정의 하는 방법
```java
public enum Operation {
	PLUS {public double apply(double x, double y) { return x + y; }};
	MINUS {public double apply(double x, double y) { return x - y; }};
	TIMES {public double apply(double x, double y) { return x * y; }};
	DIVIDE {public double apply(double x, double y) { return x / y; }};
	public abstract double apply(double x, double y);
}
```
* 전략 열거 타입 패턴
	* 열거 타입 상수 일부가 같은 동작을 공유할 때 사용
```java
enum PayrollDay {
	MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY,
	SATURDAY(PayType.WEEKEND), SUNDAY(PayType.WEEKEND);
	private final PayType payType;
	PayrollDay(PayType payType) { this.payType = payType; }
	int pay(int minutesWorked, int payRate) {
		return payType.pay(minutesWorked, payRate);
	}
	
	// 전략 열거 타입
	enum PayType {
		WEEKDAY {
			int overtimePay(int minsWorked, int payRate) {
				return minsWorked <= MINS_PER_SHIFT ? 0 : (minsWorked - MINS_PER_SHIFT) * payRate / 2;
			}
		},
		WEEKEND {
			int overtimePay(int minsWorked, int payRate) {
				return minsWorked * payRate / 2;
			}
		};
		abstract int overtimePay(int minsWorked, int payRate);
		private static final int MINS_PER_SHIFT = 8 * 60;
		int pay(int minutesWorked, int payRate) {
			int basePay = minsWorked * payRate;
			return basePay + overtimePay(minsWorked, payRate);
		}
	}
}
```
* 언제 열거타입을 쓸까 ?
	* **필요한 원소를 컴파일타임에 다 알 수 있는 상수 집합이라면...***
	* ex, 태양계 행성, 한 주의 요일, 체스 말
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkxOTg1ODcyMSwxODgzODU1MDhdfQ==
-->