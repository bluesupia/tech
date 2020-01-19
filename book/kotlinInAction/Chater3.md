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
- 기존 코드와 코틀린 코드를 자연스럽게 통합하는 핵심목표를 위해 그 역할을 해주는 것이 **확장함수**
- 확장 함수의 개념
	- 어떤 클래스의 멤버 메소드인 것처럼 호출할 수 있지만 그 클래스 밖에 선언된 함수
- 문자열의 마지막 문자를 돌려주는 메소드 추가
```kotlin
package strings  
  
fun String.lastChar() : Char = this.get(this.length - 1)
```
* 확장함수를 만드는 방법
	* 추가하려는 함수 이름 앞에 그 함수가 확장할 클래스의 이름을 덧붙인다
	* 클래스의 이름 : **수신 객체 타입**(receiver type)
		* 확장이 정의될 클래스의 타입
	* 확장함수가 호출되는 대상이 되는 값 : **수신 객체**(receiver object)
		* 그 클래스가 속한 인스턴스 객체
* 호출하는 구문
	* 수신 객체 타입 : String, 수신객체: "Kotlin"
```kotlin
println("Kotlin".lastChar())
```
* 자바 클래스로 컴파일한 클래스 파일이 있는 한  그 클래스에 원하는대로 확을 추가할 수 있다
* 확장함수 본문에도 this는 생략 가능
```kotlin
fun String.lastChar() : Char = get(length - 1)
```
* 확장함수가 캡슐화를 깨지는 않는다
	* 클래스 안에서 정의한 메소드와 달리 확장 함수 안에서는 클래스 내부에서만 사용할 수 있는 private / protected 멤버를 사용할 수 없다
* 클래스의 멤버 메소드나 확장함수 모두 **메소드**
	* 확장함수 내부에서는 수신 객체의 모든 메소드를 호출 할 수 있다
	* = 확장함수 내부에서 수신 객체의 멤버 메소드와 확장 함수를 모두 호출할 수있다
	* 호출하는 쪽에서 확장 함수와 멤버 메소드를 구분할 수 없다
	
### 3.3.1 임포트와 확장 함수
* 확장 함수를 사용하기 위해서는 **임포트** 해야만 한다
* 코틀린에서는 개별 함수도 동일한 구문으로 임포트 할 수 있다
```kotlin
import strings.lastChar  
val c = "Kotlin".lastChar()  
}
```
* *를 사용한 임포트도 가능
```kotlin
import strings.*  
val c = "Kotlin".lastChar()  
}
```
* ``as`` 키워드를 사용해 임포트한 클래스나 함수를 다른 이름으로 부를 수 있다
	* 일반적인 클래스나 함수라면 그 전체이름 (FQN, Fully Qualified Name)을 써도 되나, 코틀린 문법상 확장 함수는 반드시 짧은 이름으로 써야한다
	* 임포트시 이름을 바꾸는 것이 확장함 수 이름 충돌을 해결할 유일한 방법
```kotlin
import strings.lastChar as last
val c = "Kotlin".last()  
}
```

### 3.3.2 자바에서 확장 함수 호출
* 내부적으로 확장 함수는 ***수신 객체를 첫 번째 인자로 받는 정적 메소드***
	* 호출시 다른 어댑터 객체나 실행 시점 부가 비용이 들지 않는다
* 이런 설계로 인해 자바에서 확장함수를 사용하기 편함
	* 정적 메소드를 호출하면서 첫 번째 인자로 수신 객체를 넘기기만 하면 된다
```java
char c = StringUtilKt.lastChar("java");
```

### 3.3.3 확장 함수로 유틸리티 함수 정의
* joinToString의 최종 버전
```kotlin
fun <T> Collection<T>.joinToString(  
    seperator: String = ", ",  
  prefix: String = "",  
  postfix: String = ""  
): String {  
    val result = StringBuilder(prefix)  
    for ((index, element) in this.withIndex()) {  
        if (index > 0) result.append(seperator)  
        result.append(element)  
    }  
  
    result.append(postfix)  
    return result.toString()  
}  
  
val list = listOf(1, 2, 3)  
    list.joinToString("; ", "(", ")") 
```
* 확장함수는 단지 정적 메소드 호출에 대한 문법적인 편의
	* 클래스가 아닌 더 구체적인 타입을 수신객체 타입으로 지정할 수 있다
* 문자열의 컬렉션에 대한 정의
```kotlin
fun Collection<String>.join(  
    seperator: String = ", ",  
  prefix: String = "",  
  postfix: String = ""  
): String = joinToString(seperator, prefix, postfix)
```	
	
### 3.3.4 확장 함수는 오버라이드할 수 없다
* 코틀린의 메소드 오버라이드도 일반적인 객체지향의 메소드 오버라이드와 마찬가지
	* 확장함수는 오버라이드 할 수 없다
* view를 확장한 button
	* "view"에 저장된 값의 실제 타입에 따라 호출할 메소드가 결정!
		* 동적 디스패치
```kotlin
open class View {  
    open fun click() = println("View clicked")  
}  
  
class Button : View() {  
    override fun click() = println("Button clicked")  
}
```
```kotlin
val view:View = Button()  
view.click()
>>> Button clicked
```
* 확장함수는 클래스의 일부가 아닌, 클래스 밖에서 선언
* 이름과 파라미터가 완전히 같은 확장 함수를 기반 클래스와 하위 클래스에 대해 정의해도 실제로는 확장 함수를 호출 할때 수신객체로 지정한 변수의 **정적** 타입에 의해 어떤 확장 함수가 호출될지 결정된다
```
fun View.showOff() = println("I'm a view!")  
fun Button.showOff() = println("I'm a button!")
```
```kotlin
val view:View = Button()  
view.showOff()
>>> I'm a view!
```
* 확장 함수를 첫번째 인자가 수신 객체인 정적 자바 메소드로 컴파일 !
* 자바도 호출할 정적 함수를 같은 방식으로 정적으로 결정
```
!주의
어떤 클래스를 확장한 함수와 그 클래스의 멤버함수의 이름과 시그니처가 같다면 멤버함수가 호출된다
멤버 함수의 우선순위가 더 높다
```

### 3.3.5 	확장 프로퍼티
* 실제로 확장 프로퍼티는 아무 상태도 가질 수 없다
	* 프로퍼티라는 이름으로 불리나 상태를 저장할 적절한 방법이 없기 때문
	* 기존 클래스의 인스턴스 객체에 필드를 추가할 방법은 없다
* 프로퍼티 문법으로 더 짧게 코드를 작성할 수 있어 편리
* 확장 프로퍼티 선언
	* 일반적인 프로퍼티와 같으나, 단지 수신 객체 클래스가 추가됐을 뿐
	* 뒷받침하는 필드가 없어서 기본 게터구현을 제공할 수 없으므로 최소한 게터는 꼭 정의
```kotlin
val String.lashChar : Char  
    get() = get(length - 1)
```
* 변경가능한 확장 프로퍼티 선언
```kotlin
val String.lastChar : Char  
    get() = get(length - 1)  
  
var StringBuilder.lastChar : Char  
    get() = get(length - 1)  
    set(value: Char) {  
    this.setCharAt(length - 1, value)  
}
```
```kotlin
println("Kotlin".lastChar)  
val sb = StringBuilder("Kotlin?")  
sb.lastChar = '!'  
println(sb)
```
* 자바에서 사용하고 싶다면 항상 게터나 세터를 명시적으로 호출해야한다!
```java
char cc = StringUtilKt.getLastChar2("Java");
```

## 3.4 컬렉션 처리: 가변 길이 인자, 중위 함수 호출, 라이브러리 지원
* vararg  키워드를 사용하면,  호출 시 인자 개수가 달라질 수 있는 함수를 정의할 수 있다
* 중위 함수 호출 구문을 사용하면,  인자가 하나뿐인 메소드를 간편하게 호출 할 수 있다
* 구조 분해 선언을 하면, 복합적인 값을 분해해서 여러 변수를 나눠 담을 수 있다

### 3.4.1 자바 컬렉션 API 확장
* 코틀린에서 사용하는 컬렉션의 확장 함수
```kotlin
val strings = listOf("first", "second", "fourteenth")  
println(strings.last())  
  
val numbers = setOf(1, 14, 2)  
println(numbers.max())
```

### 3.4.2 가변 인자 함수: 인자의 개수가 달라질 수 있는 함수 정의
* 가변 길이 인자
	* 메소드를 호출할 때 원하는 개수만큼 값을 인자로 넘기면 자바 컴파일러가 배열에 그 값들을 넣어주는 기능
	* 자바 : 타입뒤에 ...을 붙인다
	* 코틀린 : 파라미터 앞에 ```vararg``` 변경자를 붙인다
```kotlin
fun listOf<T>(vararg values: T) : List<T> { ... }
```
* 이미 배열에 들어있는 원소를 가변 길이 인자로 넘길 때
	* 자바 : 배열을 그대로 넘긴다
	* 코틀린 : 배열을 명시적으로 풀어서 배열의 각 원소가 인자로 전달되게 해야한다
		* 기술적으로는 스프레드* 연산자가 그런 작업을 해준다
```kotlin
fun main(args:Array<String>) {
	val list = listOf("args:", *args)
	println(list)
}
```
### 3.4.3  값의 쌍 다루기: 중위 호출과 구조 분해 선언
```kotlin
val map = hashMapOf(1 to "one", 7 to "seven", 53 to "fifty-three")
```
* **중위 호출 (infix call)**
	* 수신 객체와 유일한 메소드 인자 사이에 메소드 이름을 넣는다
		* 객체, 메소드 이름, 유일한 인자 사이에는 공백이 들어가야 한다)
```kotlin
1.to("one")
1 to one  // 중위 호출 방식
```
* 인자가 하나뿐인 일반 메소드나, 인자가 하나뿐인 확장 함수에 중위호출을 사용할 수 있다
* 함수를 중위 호출에 사용하게 허용하고 싶으면 ``ìnfix`` 변경자를 함수 선언 앞에 추가해야한다
```kotlin
infix fun Any.to(other: Any) = Pair(this, other)
```
```kotlin
val (number, name) = 1 to "one"
```
* **구조 분해 선언**
	* Pair의 내용으로 두 변수를 즉시 초기화
	* jointToString에서 본 withIndex

## 3.5 문자열과 정규식 다루기
* 코틀린 문자열 = 자바 문자열
* 코틀린은 다양한 확장 함수를 제공

### 3.5.1 문자열 나누기
* split의 구분 문자열은 실제로 정규식
* 코틀린에서는 정규식을 명시적으로 만듦
```kotlin
println("12.345-6.A".split("\\.|-".toRegex()))  
```
* 구분 문자열을 하나 이상 인자로 받는 확장함수 존재
```kotlin
println("12.345-6.A".split(".", "-"))
```

### 3.5.2 정규식과 3중 따옴표로 묶은 문자열
#### 파일의 전체 경로명을 디렉터리, 파일이름, 확장자로 구분하는 예제
* String 확장 함수를 사용해 경로 파싱
```kotlin
fun parsePath(path : String) {  
    val directory = path.substringBeforeLast("/")  
    val fullName = path.substringAfterLast("/")  
    val fileName = fullName.substringBeforeLast(".")  
    val extension = fullName.substringAfterLast(".")  
  
    println("Dir : $directory, name : $fileName, ext : $extension")  
}
```
* 정규식을 사용해 파싱
```kotlin
fun parsePath2(path : String) {  
    val regex = """(.+)/(.+)\.(.+)""".toRegex()  
    val matchResult = regex.matchEntire(path)  
    if (matchResult != null) {  
        val (directory, filename, extention) = matchResult.destructured  
		println("Dir : $directory, name : $filename, ext : $extention")  
    }  
}
```
* 3중 따옴표 문자열에서는 역슬래시(\)를 포함한 어떤 문자도 이스케이프 할 필요가 없다

### 3.5.3  여러 줄 3중 따옴표 문자열

## 3.6 코드 다듬기: 로컬 함수와 확장
* DRY 원칙
* 코드 중복 예제
```kotlin
class User(val id:Int, val name: String, val address:String)  
  
fun savedUser(user: User) {  
    if(user.name.isEmpty()) {  
        throw IllegalArgumentException("Can't save user ${user.id} : empty Name")  
    }  
  
    if(user.address.isEmpty()) {  
        throw IllegalArgumentException("Can't save user ${user.id} : empty Address")  
    }  
  
    // user를 DB에 저장  
}
```
* 로컬함수를 사용해 코드 중복 제거
	* 중복이 사라지고, 다른 필드의 검증도 쉽게 추가 가능
	* User 객체를 로컬 함수에게 하나하나 전달해야하는 문제
		* 로컬함수는 자신이 속한 바깥함수의 모든 파라미터 변수를 사용할 수 있다
```kotlin
fun savedUser(user: User) {  
    fun validate(user: User, value: String, fieldName: String) {  
        if (value.isEmpty()) {  
            throw IllegalArgumentException("Can't save user ${user.id} : empty $fieldName")  
        }  
    }  
    validate(user, user.name, "Name")  
    validate(user, user.address, "Address")  
      
    // user를 DB에 저장  
}
```
* 로컬 함수에서 바깥 함수의 파라미터 접근하기
```kotlin
fun savedUser(user: User) {  
    fun validate(value: String, fieldName: String) {  
        if (value.isEmpty()) {  
            throw IllegalArgumentException("Can't save user ${user.id} : empty $fieldName")  
        }  
    }  
    validate(user.name, "Name")  
    validate(user.address, "Address")  
  
    // user를 DB에 저장  
}
```
* 한단계만 함수를 중첩하기를 권장
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5ODQ2OTc2NCwtNDI0NDM0NTI2LDcyOT
k2MjA1NywzMDUzNTMyOTYsLTExODcwMzE0OTUsLTE5NDkxNDg5
NDcsMTM3NjE1ODUyOCwtMTI2NDQxNjczMiwtMTA4Mzc2NjcyMC
w0MjMwNjMyMTUsLTE0ODMxNzE2NDEsLTkxMDgwODg5OCwtOTEw
NzYxNzgzLC04MTQ4NjA2NjMsLTEyMzY0MjE3MTMsMTUyNjcyNz
c5NywtMTIxMjI0NDAwNCwtMTg2MTYxNzYwOSwxMjk1MzQ1MjQw
LDIxMzYzNDI0ODJdfQ==
-->