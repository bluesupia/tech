

# 아이템17. 변경 가능성을 최소화하라
* 불변 클래스
	* 그 인스턴스의 내부 값을 수정할 수 없는 클래스
	* 불변 인스턴스에 간직된 정보는 고정되어 객체가 파괴되는 순간까지 절대 달라지지 않는다
	* 자바 플랫폼 라이브러리 불변클래스 예
		* String, BigInteger, BigDecimal
	* 장점
		* 가변 클래스보다 설계/구현/사용하기 쉬움
		* 오류가 생길 여지가 적고 훨씬 안전함
* 불변클래스로 만드는 규칙
	* 객체의 상태를 변겨아는 메서드를 제공하지 않는다
	* 클래스를 확장할 수 없도록 한다
		* 상속을 막는 대표적인 방법은 클래스를 final로 선언
	* 모든 필드를 final로 선언
	* 모든 필드를 private로 선언
	* 자신 외에는 내부의 가변 컴포넌트에 접근할 수 없도록 한다
* 불변 복소수 클래스
	* 복소수(실수부와 허수부로 구성된 수)를 표현
	* 메서드
		* 실수부와 허수부값을 반환하는 접근자 메서드 ( realPart, imaginaryPart)
		* 사칙연산 메서드(plus, minus, times, dividedBy)
			* 인스턴스 자신을 수정하지 않고 새로운 Complex 인스턴스를 만들어 반환
			* 함수형프로그래밍
				* 피연산자 자체는 그대로인 프로그래밍 패턴
			* 메서드 이름으로 동사(ex. add) 대신 전치사 (ex. plus) 사용

```java
public final class Complex {
	private final double re;
	private final double im;

	public Complex(double re, double im) {
		this.re = re;
		this.im = im;
	}
	
	public double realPart() { return re; }
	public double imaginaryPart() { return im; }
	
	public Complex plus(Complex c) {
		return new Complex(re + c.re, im + c.im);
	}
	
	public Complex minus(Complex c) {
		return new Complex(re - c.re. im - c.im);
	}
	
	public Complex times(Complex c) {
		return new Complex(re * c.re - im * c.im, re * c.im + im * c.re);
	}

	public Complex dividedBy(Complex c) {
		double tmp = c.re * c.re + c.im * c.im;
		return new Complex(re * c.re + im * c.im) / tmp, (im * c.re - re * c.im) / tmp);
	}
}
```
* 함수형 프로그래밍의 장점
	* 불변이 되는 영역의 비율이 높아짐
* 불변 객체
	* **불변 객체는 단순**
	* **불변 객체는 근본적으로 스레드 안전하여 따로 동기화 불필요**
		* **안심하고 공유할 수 있음**
	* 불변 클래스라면 한번 만든 인스턴스를 최대한 재활용
		* 자주 쓰이는 값들을 상수 (public static final)로 제공
	* 정적 팩터리 사용
		* 여러 클라이언트가 인스턴스를 공유하여 메모리 사용량과 가비지 컬렉션 비용이 줄어즘
		* 클라이언트를 수정하지 않고도 필요에 따라 캐시기능을 나중에 추가가능
	* 자유롭게 공유 가능 = 방어적 복사도 필요 없다
	* 불변 객체끼리는 내부 데이터를 공유할 수 있음
	* 그 자체로 실패 원자성(failure atomicity)을 제공
	* 단점
		* **값이 다르면 반드시 독립된 객체로 만들어야 함**

* 불변클래스 설계방법
	* 생성자 대신 정적 팩터리를 사용한 불변클래스
		*  이방법이 대개 최선
		* 정적 팩터리 방식은 다수의 구현 클래스를 활용한 유연성을 제공하고 다음 릴리스에서 객체 캐싱 기능을 추가해 성능을 끌어올릴 수 있다
```java
public final class Complex {
	private final double re;
	private final double im;

	private Complex(double re, double im) {
		this.re = re;
		this.im = im;
	}

	public static Complex valueOf(double re, double im) {
		return new Complex(re, im);
	}
```

* 게터가 있다고해서 무조건 세터를 만들지 말자
	* **클래스는 꼭 필요한 경우가 아니라면 불변!**
* **불변으로 만들 수 없는 클래스라도 변경할 수 있는 부분은 최소한으로 줄이자**
* **생성자는 불변식 설정이 모두 완료된, 초기화가 완벽히 끝난 상태의 객체를 생성해야함**
### 아이템15+17 = 다른 합당한 이유가 없다면 모든 필드는 private final이어야 한다
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM5ODI3OTI5Ml19
-->