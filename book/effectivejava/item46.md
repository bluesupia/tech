## 아이템46. 스트림에서는 부작용 없는 함수를 사용하라
* 스트림 패러다임의 핵심
	* 계산을 일련의 변환으로 재구성하는 부분
	* 각 변환 단계는 가능한 한 이전 단계의 결과를 받아 처리하는 순수 함수
	* 스트림 연산에 건네는 함수 객체는 모두 부작용이 없어야 함
* forEach 연산
	* **스트림 계산 결과 보고할 때만 사용하고 계산할 때는 쓰지말자**
	* 종단 연산 중 기능이 가장 적고 가장 '덜' 스트림답다
* 수집기(Collector)
	* java.util.stream.Collectors 
	* 스트림 사용시 꼭 배워야 하는 개념
	* 자바 10에서 43개 메소드 존재
	* API 문서 참조
	* [https://docs.oracle.com/javase/10/docs/api/java/util/stream/Collectors.html](https://docs.oracle.com/javase/10/docs/api/java/util/stream/Collectors.html)
* 중요 수집기 팩터리
	* toList
	* toMap
	* groupingBy
	* joining
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTcxMDM1Nzc1Ml19
-->