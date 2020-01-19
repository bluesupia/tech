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
- 더많은 기능을 쓸 수 있음 
	- 확장함수!!
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM2ODk0NzAzNiwxMjk1MzQ1MjQwLDIxMz
YzNDI0ODJdfQ==
-->