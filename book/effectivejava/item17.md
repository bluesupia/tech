

# 아이템17. 변경 가능성을 최소화하라
* 불변 클래스
	* 그 인스턴스의 내부 값을 수정할 수 없는 클래스
	* 불변 인스턴스에 간직된 정보는 고정되어 객체가 파괴되는 순간까지 절대 달라지지 않는다
	* 자바 플랫폼 라이브러리 불변클래스 예
		* String, BigInteger, BigDecimal
	* 장점
		* 가변 클래스보다 설계/구현/사용하기 쉬움
		* 오류가 생길 여지가 적고 훨씬 안전함
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDU2NDMzMzRdfQ==
-->