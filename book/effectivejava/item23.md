## 아이템23. 태그 달린 클래스보다는 클래스 계층구조를 활용하라
* 태그 달린 클래스의 단점
	* 열거 타입 선언, 태그 필드, Switch 문 등 쓸데없는 코드가 많고 가독성이 나쁘다
	* 다른 의미를 위한 코드도 항상 함께하여 메모리도 많이 사용
	* 필드들을 final로 선언하려면, 해당 의미에서 쓰이지 않는 필드들까지 생성자에서 초기화해야한다
	* 또 다른 의미를 추가하려면 코드를 수정
	* 인스턴스의 타입 만으로는 현재 타나내는 의미를 알 수 없다
	* 요약 : **장황하고 오류를 내기 쉽고 비효율적**
* 태그 달린 클래스 -> 클래스 계층 구조로 바꾸는 방법
	* 계층 구조의 루트가 될 추상 클래스를 정의하고 태그 값에 따라 동작이 달라지는 메서드드들을 추상 메서드로 선언
	* 태그 값에 관계없이 동작이 일정한 메서드들을 일반 메서드로 추가
	* 모든 하위 클래스에서 공통으로 사용하는 데이터 필드들도 전부 루트 클래스로 올림
	* 루트 클래스를 *확장*한 구체 클래스를 의미별로 정의
	* 각 하위 클래스에는 각자의 의미에 해당하는 데이터 필드 추가
	* 루트 클래스가 정의한 추상 메서드를 각자의 의미에 맞게 구현
```java
abstract class Figure {
	abstract double area();
}

class Circle extends Figure {
	final double radius;

	Circle(double radius) { this.radius = radius; }

	@Override
	double area() {
		return Math.PI * (radius * radius);
	}
}

class Rectangle extends Figure {
	final double length;
	final double width;
	
	Rectangle(double length, double width) {
		this.length = length;
		this.width = width;
	}

	@Override
	double area() {
		return length * width;
	}
}
```
* 계층구조 활용의 이점
	* 간결하고 명확
	* 쓸데없는 코드가 사라짐
	* 관련없는 데이터 필드 제거
	* 데이터 필드가 모두 final
	* 각 클래스의 생성자가 모든 필드를 초기화하고 추상 메서드를 모두 구현했는지 컴파일러가 확인
	* 타입 사이의 계층 관계반영으로 유연성은 물론 컴파일타임 타입 검사 능력을 높여준다
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NDIxNjU5MjAsLTExODQyNTAzNjJdfQ
==
-->