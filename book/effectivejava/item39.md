## 아이템39.  명명 패턴보다 애너테이션을 사용하라
* 명명패턴의 단점
	* 오타가 나면 안됨
	* 올바른 프로그램 요소에서만 사용되리라 보증할 방법이 없음
	* 프로그램 요소를 매개변수로 전달할 마땅한 방법이 없다
* 애너테이션
	* 위 명명패턴의 문제를 해결해주는 개념
	* Junit4에도 전면 도입
* 마커 애너테이션 타입선언
	* 메타애너테이션(meta-annatation)
		* 애너테이션 선언에 다는 애너테이션
			* @Retention(RetentionPolicy.RUNTIME)
				* @Test가 런타임에도 유지되어야 한다는 표시
				* 생략시 테스트 도구는 @Test를 인식할 수 없다
			* @Target(ElementType.METHOD)
				* @Test가 반드시 메서드 선언에서만 사용돼야 한다
				* 클래스 선언, 필드 선언 등 다름 프로그램 요소에는 달 수 없다
	* 아무 매개변수 없이 단순히 대상에 마킹(marking)
	* 대상 코드의 의미는 그대로 둔 채 그 애너테이션에 관심 있는 도구에서 특별한 처리를 할 기회를 준다
```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Test{
}
```
* 매개변수 하나를 받는 애너테이션 타입
```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface ExceptionTest {
	Class<? extends Throwable> value();
}
```
* 배열 매개변수를 받는 애너테이션 타입
```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface ExceptionTest {
	Class<? extends Throwable>[] value();
}
```
* @Repeatable
	* 반복 가능 애너테이션
	* 여러개의 값을 받는 애너테이션을 다른 방식 !
	* 주의!
		* @Repeatable 을 단 애너테이션을 반환하는 '컨테이너 애너테이션'을 하나더 정의하고, @Repeatable에 이 컨테이너 애너테이션의 class 객체를 매개변수로 전달
		* 컨테이너 애너테이션은 내부 애너테이션 타입의 배열을 반환하는 value 매서드를 정의
		* 컨테이너 애너테이션 타입에는 적절한 보존 정책(@Retention)과 적용 대상(@Target)을 명시해야 함
```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@Repeatable(ExceptionTestContainer.class)
public @interface ExceptionTest {
	Class<? extends Throwable> value();
}

@Retention(RetentionPolicy.RUNTIME) 
@Target(ElementType.METHOD)
public @interface ExceptionTestContainer {
	ExceptionTest[] value();
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI0OTk1NDIzMSwtNDk0MDMyMTU3LC0xNT
k5MTMzMDBdfQ==
-->