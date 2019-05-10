## 아이템50. 적시에 방어적 복사본을 만들라!
* **클라이언트가 불변식을 깨뜨리려 한다는 가정하에 방어적으로 프로그래밍**
* **Date**
	* Instant, LocalDateTime, ZonedDateTime 사용
	* 생성자에게 받은 가변 매개 변수 각각을 방어적으로 복사해야 함
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIzNDgyMzMzMF19
-->