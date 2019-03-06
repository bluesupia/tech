

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
* 함수형 프로그래밍의 장점
	* 불변이 되는 영역의 비율이 높아짐
* 불변 객체
	* **불변 객체는 단순**
	* **불변 객체는 근본적으로 스레드 안전하여 따로 동기화 불필요**
		* **안심하고 공유할 수 있음**
	* 불변 클래스라면 한번 만든 인스턴스를 최대한 재활용
		* 자주 쓰이는 값들을 상수 (public static final)Fh 

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

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA0NzYwNDEyMV19
-->