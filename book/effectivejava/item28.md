## 아이템28. 배열보다는 리스트를 사용하라
* 배열과 제너릭 타입의 차이
	* 배열은 공변 (함께 변한다)
		* Sub가 Super의 하위 타입이라면 배열 Sub[]는 배열 Super[]의 하위 타입
	* 제너릭은 불공변
		* 서로 다른 타입 Type1, Type2가 있을 때 `List<Type1>`과 `List<Type2>`는 상 하위 타입이 아님
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMzMjUxMjg1OF19
-->