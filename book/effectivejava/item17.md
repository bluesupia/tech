

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
* 불변 복소스 클래스
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
	
	}
}
```

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3ODE5ODEzMF19
-->