# Chater3. 함수 정의와 호출

- 개선된 함수 정의와 호출 기능
- 확장 함수와 프로퍼티를 사용해 자바 라이브러리를 코틀린 스타일로 적용하는 방법
- 코틀린-자바 함께 쓰는 프로젝트에서 최대 장점
- 컬렉션, 문자열, 정규식으로 문제 한정


## 3.1 코틀린에서 컬렉션 만들기
```kotlin
val set = hashSetOf(1, 7, 53)  
val list = arrayListOf(1, 7, 53)  
val map = hashMapOf(1 to "one", 7 to "seven", 53 to "fifty-three")

println(set.javaClass)  
println(list.javaClass)  
println(map.javaClass)
```
- 코틀린은 자체 컬렉션을 제공하지 않음
	- 표준 자바 컬렉션을 활용
	- 자바코드와 상호작용하기가 쉬움
- 더많은 기능을 쓸 수 있음 
	- 확장함수!!
```kotlin
val strings = listOf("first", "second", "fourteenth")  
println(strings.last())  
  
val numbers = setOf(1, 14, 2)  
println(numbers.max())
```
- 어떻게 만드는지! 어떻게 가능하게 하는가

## 3.2 함수를 호출하기 쉽게 만들기
```kotlin
val list = listOf(1, 2, 3)
println(list)
```
- 디폴트 구현과 달리 원소사이를 세미콜록으로 구분하고 괄호로 기스트를 둘러싸도록 해보자
```kotlin
// 초기구현
fun <T> joinToString(  
    collection: Collection<T>,  
    serator: String,  
    prefix: String,  
    postfix: String  
) : String {  
    val result = StringBuilder(prefix)  
    for ((index, element) in collection.withIndex()) {  
        if (index > 0) result.append(serator)  
        result.append(element)  
    }  
  
    result.append(postfix)  
    return result.toString()  
}

// 초기 호출
val list = listOf(1, 2, 3)  
println(joinToString(list, "; ", "(", ")"))
```
- 위 함수는 제너릭
- 선언부분의 고민
	- 함수를 호출하는 문장을 덜 번잡 스럽게 만들까!
	- 호출할 때마다 네개의 인자를 모두 전달 하지 않으려면 ?

### 3.2.1 이름 붙인 인자
- 함수 호출 부분의 가독성
	- 인자로 전달한 각 문자열이 어떤 역할을 하지는지의 구분? 
```kotlin
println(joinToString(list, seperator = "; ", prefix = "(", postfix = ")"))  
println(joinToString(list, prefix = "(", postfix = ")", seperator = "; "))
```

### 3.2.2 디폴트 파라미터 값
- 자바에서는 일부 클래스에서 오버로딩한 메소드가 너무 많아지는 문제가 존재
	- java.lang.Thread의 8가지 생성자 ([https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html#constructor.summary](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html#constructor.summary))
	- 함수선언에서 파라미터의 디폴트 값을 지정해 오버로드 중 상당수를 피할 수 있다
```kotlin
fun <T> joinToString(  
    collection: Collection<T>,  
  seperator: String = ", ",  
  prefix: String = "",  
  postfix: String = ""  
): String {  
    val result = StringBuilder(prefix)  
    for ((index, element) in collection.withIndex()) {  
        if (index > 0) result.append(seperator)  
        result.append(element)  
    }  
  
    result.append(postfix)  
    return result.toString()  
}  
  
println(joinToString(list, ", ", "", ""))  
println(joinToString(list))  
println(joinToString(list, "; "))  
println(joinToString(list, postfix = ";", prefix = "# ")) 
```
- 이름붙은 인자를 사용하는 경우
	- 인자 목록의 중간인자를 생략하고, 
	- 저장하고 싶은 인자를 이름을 붙여서 
	- 순서와 관계없이 지정가능
- 디폴트 파라미터 값은 선언쪽에서 지정
	- 어떤 클래스 안에 정의된 함수의 디폴트 값 변경 후 재컴파일 시, 해당 코드 중 값을 지정하지 않는 모든 인자는 자동으로 바뀐 디폴트값을 적용

### 3.2.3 정적인 유틸리티 클래스 없애기 : 최상위 함수와 프로퍼티
- 객체지향 언어인 자바는, 모든 코드를 클래스의 메소드로 작성해야한다!
	- 다양한 정적 메소드를 모아두는 역할만 담당하며, 특별한 상태나 인스턴스 메소드가 없는 클래스가 존재
			- ex. JDK의 Collections([https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html](https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html)), ***Util
- 코틀린에서는, 무의미한 클래스 대신 함수를 직접 소스파일의 최상위 수준, 모든 다른 클래스의 밖에 위치시키면 된다
- join.kt
```kotlin 
package strings  
  
//fun String.lastChar() : Char = this.get(this.length - 1)  
  
val String.lastChar : Char  
get() = get(length - 1)  
  
  
@JvmOverloads  
fun <T> joinToString(  
    collection: Collection<T>,  
    seperator: String = ", ",  
    prefix: String = "",  
    postfix: String = ""  
): String {  
    val result = StringBuilder(prefix)  
    for ((index, element) in collection.withIndex()) {  
        if (index > 0) result.append(seperator)  
        result.append(element)  
    }  
  
    result.append(postfix)  
    return result.toString()  
}
```
```java
import strings.JoinKt;
...
// @JvmOverloads 으로 가능!
JoinKt.joinToString(list) 
```

#### 최상위 프로퍼티
- 다른 프로퍼티처럼 접근자 메소드를 통해 자바 코드에 노출
	- val는 게터, var는 게터와 세터
- 겉으로 보기엔 상수이나 게터를 사용해야한다면 자연스럽지 못하다
- 그러러면 상수를 public static final 필드로 컴파일
- ```const``` 변경자를 추가하면 public static final 필드로 컴파일
	- 원시타입과 String 만!

## 3.3 메소드를 다른 클래스에 추가: 확장 함수와 확장 프로퍼티
### 3.3.1 임포트와 확장 함수
- 기존 코드와 코틀린 코드를 자연스럽게 통합하는 핵심목표를 위해 그 역할을 해주는 것이 **확장함수**
- 확장 함수의 개념
	- 어떤 클래스의 멤버 메소드인 것처럼 호출할 수 있지만 그 클래스 밖에 선언된 함수
	- 
### 3.3.2 자바에서 확장 함수 호출
### 3.3.3 확장 함수로 유틸리티 함수 정의
### 3.3.4 확장 함수는 오버라이드할 수 없다
### 3.3.5 	확장 프로퍼티

## 3.4 컬렉션 처리: 가변 길이 인자, 중위 함수 호출, 라이브러리 지원
### 3.4.1 자바 컬렉션 API 확장
### 3.4.2 가변 인자 함수: 인자의 개수가 달라질 수 있는 함수 정의
### 3.4.3  값의 쌍 다루기: 중위 호출과 구조 분해 선언

## 3.5 문자열과 정규식 다루기
### 3.5.1 문자열 나누기
### 3.5.2 정규식과 3중 따옴표로 묶은 문자열
### 3.5.3  여러 줄 3중 따옴표 문자열

## 3.6 코드 다듬기: 로컬 함수와 확장

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgxNDg2MDY2MywtMTIzNjQyMTcxMywxNT
I2NzI3Nzk3LC0xMjEyMjQ0MDA0LC0xODYxNjE3NjA5LDEyOTUz
NDUyNDAsMjEzNjM0MjQ4Ml19
-->