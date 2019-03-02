
## 아이템13. clone 재정의는 주의해서 진행하라
* Cloneable ?
	* 복제해도 되는 클래스임을 명시하는 용도의 믹스인 인터페이스
	* 의도한 목적을 제대로 이루지 못함
		* clone 메서드가 선언된 곳이 Cloneable이 아닌 Object 이고 proteced 라는 게 문제
		* 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAyNzg0NzI4Nl19
-->