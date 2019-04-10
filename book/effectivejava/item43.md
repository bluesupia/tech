## 아이템43. 람다보다는 메서드 참조를 사용하라
* 메서드 참조 (::)
* 주의
	* 람다 매개변수의 이름 자체가 프로그래머에게 좋은 가이드가 되기조 함
	* 람다가 더 짧고 더 명확한 경우도 있다
		* 주로 메서드와 람다가 같은 클래스에 있을 때
* 메서드 참조 유형
* 
| 메서드 참조 유형 |예  |같은 기능을 하는 람다|
|--|--|--|
|정적 |Integer::parseInt |str -> Integer.parseInt(str)|
|한정적(인스턴스)  |Instant.now()::isAfter  |Instant then = Instant.now();<br/>t -> then.isAter()|
|비한정적(인스턴스)  |String::toLowerCase  |str -> str.toLowerCase()|
|클래스 생성자  |TreeMap<K,V>::new|() -> new TreeMap<K,V>()|
|배열 생성자  |int[]::new|len -> new int[len]|

#### 핵심정리
* **메서드 참조 쪽이 짧고 명확하다면 메서드 참조를! 그렇지 않을 때만 람다!
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5NTMxNDU2NDksLTU3MzEyOTE3Nl19
-->