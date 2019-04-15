## 아이템44. 표준 함수형 인터페이스를 사용하라
* 템플릿 메서드 패턴의 현대적 해법
	* 같은효과의 함수객체를 받는 정적 팩터리나 생성자 제공
	* 이때 함수형 매개변수 타입을 올바르게 선택
```java
protected boolean removeEldestEntry(Map.Entry<K,V> eldest) { 
	return size() > 100;
}

// 불필요한 함수형 인터페이스 : 대신 표준 함수형 인터페이스를 사용하라!
@FuntionalInterface
interface EldestEntryRemoveFunction<
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ5MTEwOTA0OF19
-->