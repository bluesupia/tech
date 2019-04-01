## 아이템37. ordinal 인덱싱 대신 EnumMap을 사용하라
* EnumMap
	* 실질적으로 열거 타입 상수를 값으로 매핑하는 일
	* 배열보단 EnumMap!
	* 배열보다 짧고 명료하고 안전하고 성능도 원래 버전과 비등
	* 안전하지 않은 형변환은 쓰지 않고, 맵의 키인 열거타입 그 자체로 출력용 문자열을 제공
	* 배열 인덱스를 계산 하는 과정에서 오류가 날 가능성 원천봉쇄
```java
Map<Plant.LifeCyle, Set<Plant>> plantsByLifeCycle = new EnumMap<>(Plant.LifeCycle.class);
for (Plant.LifeCycle lc : Plant.LifeCycle.values())
	plantsByLifeCycle.put(lc, new HashSet<>());
for (Plant p : garden)
	plantsByLifeCycle.get(p.lifeCycle).add(p);
```

* 스트림 사용
	* 스트림으로 맵을 관리하면 코드를 더 줄임
	* 코드1은 EnumMap이 아닌 고유맵구현체 사용으로 Enumap을 써서 얻는 공간과 성능 이점이 사라짐
```java
//코드1
System.out.println(
Arrays.stream(garden)
	  .collect(groupingBy(p -> p.lifeCycle)
	  );
//코드2
System.out.println(
Arrays.stream(garden)
	  .collect(groupingBy(p -> p.lifeCycle, 
	  () -> new E
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0OTQ5MTQ5NjksLTExNTI5OTExNF19
-->