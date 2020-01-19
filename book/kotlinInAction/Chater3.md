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
``` kotlin
// 초기구현

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMjkwMzQ5OTIsLTE4NjE2MTc2MDksMT
I5NTM0NTI0MCwyMTM2MzQyNDgyXX0=
-->