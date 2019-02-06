# 객체 생성과 파괴

 ## 아이템1. 생성자 대신 정적 팩터리 메서드를 고려하라

### 장점
 1. 이름을 가질 수 있다.
 2. 호출될 때마다 인스턴스를 새로 생성하지 않아도 된다
 3. 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
 4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
5. 정적 팩터리 메서드를 작성하는 시점에는 반환활 객체의 클래스가 존재 하지 않아도 된다.

### 단점
1. 상속을 하려면 public이나 protected 생성자가 필요하니, 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.
2. 정적 팩터리 메서드는 프로그래머가 찾기 어렵다


### 핵심정리.
정적 팩터리 메서드와 public 생성자는 각자의 쓰임새가 있으니 상대적인 장단점을 이해하고 사용하는 것이 좋다.
그렇다고 하더라도 정적 팩터리 메서드를 사용하는게 유리한 경우가 더 많으므로 무작정 public 생성자를 제공하던 습관이 있다면 고치자.


#### In my case.
처음 해보는 코드리뷰
무작정 public 생성자를 이용하고 있던 나.
정적 팩터리 메서드라뇨 ?
그렇게 알게되었다.
이책의 첫 아이템만 봤더라도 부끄럽진 않았을 텐데
언젠가 내 코드를 보고 
한점 부끄럼 없게되길


## 아이템2. 생성자에 매개변수가 많다면 빌더를 고려하라
선택적 매개변수가 많을 경우의 대응방법
1. 점층적 생성자 패턴
2. 자바빈스 패턴
3. 빌더 패턴 : 1+2의 장점만 취함

### 핵심정리.
생성자나 정적 팩터리가 처리해야 할 매개변수가 많다면 (4개이상?) 빌더 패턴을 선택하는게 낫다.
매개변수 중 다수가 필수가 아니거나 같은 타입이면 더욱더!

## 아이템3. private 생성자나 열거 타입으로 싱글턴임을 보증하라
* 싱글턴(singleton)
인스턴스를 오직 하나만 생성할 수 있는 클래스
**사용하는 클라이언트를 테스트하기가 어려워 질 수 있다**
인터페이스를 구현해서 만든 경우가 아니라면 mock 구현으로 대체할 수 없기 때문

* 싱글턴을 만드는 방법
1. public static filan 필드 방식의 싱글턴
 ```java
public class Elvis {
    public static final Elvis INSTANCE = new Elvis();
	private Elvis() { ... }
	public void leaveTheBuilding() { ... }
}
```
- 해당클래스가 싱글턴임이 API에 명백히 드러난다
- 간결함

2. 정적 팩터리 방식의 싱글턴
 ```java
public class Elvis {
	private static final Elvis INSTANCE = new Elvis();
	private Elvis() { ... }
	public static Elvis getInstance() { return INSTANCE; }
	
	public void leaveTheBuilding() { ... }	
}
```
- (마음이 바뀌면) API를 바꾸지 않고도 싱글턴이 아니게 변경할 수 있다
- 원한다면 정적 팩터리를 제너릭 싱글턴 팩터리로 만들 수 있다
- 정적 팩터리의 메서드 참조를 공급자로 사용할 수 있다

3. 열거타입 방식의 싱글턴 (가장 좋은 방법)
```java
public enum Elvis {
	INSTANCE;
	public void leaveTheBuilding() { ... }
}
```

## 아이템4. 인스턴스화를 막으려거든 private 생성자를 사용하라

```java
public class UtilityClass {
	private UtilityClass() {
		throw new AssertionError();
	}
}
```

## 아이템5. 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라
많은 클래스가 하나 이상의 자원을 의존한다

- 정적 유틸리티를 잘못 사용한 예 - 유연하지 않고 테스트가 어렵다
```java
public class SpellChecker {
	private static final Lexicon dictionary = ...;
	
	private SpellChecker() {}
	
	public static static boolean isValid(String word) { ... }
	public static List<String> suggestions(String typo) { ... }
}
```

- 싱글턴을 잘못 사용한 예 - 유연하지 않고 테스트가 어렵다
```java
public class SpellChecker {
	private final Lexicon dictionary = ... ;
	
	private SpellChecker( ... ) {}
	public static SpellChecker INSTANCE = new SpellChecker( ... );

	public static static boolean isValid(String word) { ... }
	public static List<String> suggestions(String typo) { ... }
}
```

* 두 예제의 문제점
	* 사전을 단 하나만 사용한다고 가정한다는 점에서 유연성이 떨어진다

### 인스턴스를 생성할 때 생성자에게 필요한 자원을 넘겨주자!
```java
public class SpellChecker {
	private final Lexicon dictionary = ... ;
	
	public SpellChecker(Lexicon dictionary) {
		this.dictionary = Objects.requireNonNull(dictionary);
	}

	public static static boolean isValid(String word) { ... }
	public static List<String> suggestions(String typo) { ... }
}
```

* 변형
	* 생성자에게 자원 팩터리를 넘겨주는 방식
	* *팩터리란* ? 호출할 때마다 특정 타입의 인스턴스를 반복해서 만들어주는 객체
	* 즉, 팩터리 메스드 패턴을 구현한 것
	* 자바8에서 소개한 **Supplier`<T>`** 인터페이스가 팩터리를 완벽하게 표현한 예

### 핵심정리
- 클래스가 내부적으로 하나 이상의 자원에 의존하고, 그 자원이 클래스 동작에 영향을 준다면 싱글턴과 정적 유틸리티 클래스는 사용하지 않는 것이 좋다. 이 자원들을 클래스가 직접 만들게 해서도 안된다
- 필요한 자원을 (혹은 그 자원을 만들어주는 팩터리를) 생성자에 (혹은 정적 팩터리나 빌더에) 넘겨주자


## 아이템6. 불필요한 객체 생성을 피하라

* 똑같은 기능의 객체를 매번 생성하기보다 객체 하나를 재사용하는 편이 더 좋다.
```java
String s = new String("supersupa")
String ss = "supersupa"
```

* 생성자 대신 정적 팩터리 메스드를 제공하는 불변 클래스에서는 정적 팩터리 메서드를 사용해 불필요한 객체 생성을 피할 수 있다
```java
Boolean b = new Boolean("TRUE");
Boolean bb = Boolean.valueOf("TRUE");
```

* 생성비용이 아주 비싼 객체도 있다. 이럴 땐 캐싱하여 재사용하길 권장
	* **String.matches** 
```java
static boolean isRomanNumeral(String s) {
	return s.matches("^(?=,)M*(C[MD]|D?C{0,3})" + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");
}
```
```java
public class RomanNumerals {
	private static final Pattern ROMAN = Pattern.comile("^(?=,)M*(C[MD]|D?C{0,3})" + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");

	static boolean isRomanNumeral(String s) {
		return ROMAN.matcher(s).matches();
	}
}
```

* 객체가 불변이라면 재사용해도 안전함이 명백하나 훨씬 덜 명확하거나 반대되는 상황도 있따
	* *어댑터* ? 실제 작업은 뒷단 객체에 위임하고 자신은 제 2의 인터페이스 역활을 해주는 객체
	* 어댑터는 뒷단 객체만 관리하면 된다
		* Map 인터페이스의 keySet 메서드

* *오토박싱* ? 프로그래머가 기본 타입과 박싱된 기본 타입을 섞어 쓸 때 자동으로 상호 변환해주는 기술
	* 박싱된 기본 타입보다는 기본 타입을 사용하고, 의도치 않은 오토박싱이 숨어들지 않도록 주의
```java
private static long sum() {
	Long sum = 0L;
	for (long i=0; i < Integer.MAX_VALUE; i++) {
		sum += i;
	}
	return sum;
}
```

* vs 아이템50 : 새로운 객체를 만들어야 한다면 기존 객체를 재사용하지 마라
* 기존 객체를 재사용해야 한다면 새로운 객체를 만들지 마라

## 아이템7. 다 쓴 객체 참조를 해제하라

* 자바에서 메모리 관리를 더이상 신경 쓰지 않아도 되는 것은 아니다
* 쓰자마자 모두 null 처리 할 필요는 없다. **예외적인 경우에만**

## 아이템8. finalizer와 cleaner 사용을 피하라
자바가 제공하는 두가지 객체 소멸자
- finalizer / cleaner
- finalizer :예측할 수 없고, 상황에 따라 위험할 수 있어 일반적으로 불필요
- cleaner :  finalizer보다 덜 위험하나 예측할 수 없고, 느리고, 일반적으로 불필요
- 즉시 수행된다는 보장이 없으므로 **제때 실행되어야 하는 작업은 절대 할 수 없음**
- 수행 시점 뿐만 아니라 수행 여부조차 보장하지 않으므로 **상태를 영구적으로 수정하는 작업에서는 의존해선 안됨** ex. 데이터베이스 같은 공유 자원의 영구락 해제
- **심각한 성능문제 동반**
- **finalizer 공격에 노출되어 심각한 보안문제 야기** 방어위해서는 아무일도 하지 않는 finalize 메소드를 만들고  fianl로 선언

자바에서 접근할 수 없게 된 객체를 회수하는 역할은 가비지 컬렉터가 담당
프로그래머는 아무런 작업도 요구하지 않음

비메모리 자원 회수 하는 용도로 try-with-resources / try-finally 사용해 해결


파일이나 스레드 등 종료해야 할 자원을 담고 있는 객체의 클래스에서 대안은 ?
- AutoCloseable 을 구현해주고 사용이 끝나면 close 메서드를 호출

### in my case.
나는 사용해 본 적이 없다 ... finalizer와 cleaner를..
자원회수에 대해 고려해서 프로그래밍 하지 않았다..
처음 만들어본 Consumer Thread
스레드 종료 후 에 대해 생각해보는 계기가 되었다
주옥같은 PR

## 아이템9.  try-finally 보다는 try-with-resources를 사용하라.

* close 메서드를 호출해 직접 닫아줘야 하는 자원들이 많다
* ex) InputStream, OutputStream, java.sql.Connection
* 전통적으로 try-finally가 쓰임
```java
static void copy(String src, String dst) throws IOException {
	InputStream in = new FileInputStream(src);
	try {
		OutputStream out = new FileOutputStream(dst);
		try {
			byte[] buf = new byte[BUFFER_SIZE];
			int n;
			while((n = in.read(buf)) >= 0) 
				out.write(buf, 0, n);
		} finally {
			out.close();
		}
	} finally {
		in.close();
	}
}
```
* try-with-resoucese 사용하면 코드가 더 짧고 분명해지고, 만들어지는 예외 정보도 훨씬 유용
	* 이 구조를 사용하려면 해당 자원이 AutoCloseable 인터페이스를 구현해야함
		* void를 반환하는 close 메서드 하나만 정의된 인터페이스 
```java
static void copy(String src, String dst) throws IOException {
	try (InputStream in = new FileInputStream(src);
		 OutputStream out = new FileOutputStream(dst)) {
		 byte[] buf = new byte[BUFFER_SIZE];
		 int n; 
		 while((n = in.read(buf)) >= 0)
			 out.write(buf, 0, n)
	}
}
```

### in my case.
* 위 주옥같은 코드리뷰 이후 소스코드가 바뀌었다
comsumer의 close 메소드를
상위클래스에서 close 메소드에서 호출하고 있었다
왜 위처럼 하였는가!!
이후 상위 클래스가 AutoClosable을 implement 하도록 수정되고, close 메소드를 override 하도록 함!

#모든 객체의 공통 메서드

## 아이템10. equals는 일반 규약을 지켜 재정의하라


<!--stackedit_data:
eyJoaXN0b3J5IjpbNDg3Mjc5OTIyLC0xNjcyMzA2NDM4LDE3OD
kyODU1ODksLTkwMjc3MzUxOSwtNzQxNDg0MTAxLDE2NDAyMzI1
ODcsMTkzODIzNjk5OCwtMTcyOTg0ODQ0Niw3NDIxNjc2NjEsLT
MwNzY0Mjg5MF19
-->