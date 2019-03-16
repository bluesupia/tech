## 아이템27. 비검사 경고를 제거하라
* **할 수 있는 한 모든 비검사 경고를 제거하라**
	* 모두 제거 한다면 그 코드는 타입 안전성이 보장
	* 런타임에 ClassCastException이 발생할 일이 없고 의도대로 잘 동작하리라 확신
* **@SuppressWar**
	* **경고를 제거할 수는 없지만 타입 안전하다고 확인할 수 있다면**
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA4Njg4MTMzMF19
-->