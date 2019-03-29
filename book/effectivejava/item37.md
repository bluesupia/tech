## 아이템37. ordinal 인덱싱 대신 EnumMap을 사용하라
* EnumMap
	* 실질적으로 열거 타입 상수를 값으로 매핑하는 일
	* 배열보단 EnumMap!
```java
Map<Plant.LifeCyle, Set<Plant>> plantsByLifeCycle = new EnumMap<>(Plant.LifeCycle.class);
for (Plant.LifeCycle lc : Plant.LifeCycle.values())
	plantsByLifeCycle.put(lc, new HashSet<>());
for (Plant p : garden)
	plantsByLifeCycle.get(p.lifeCycle).add(p);
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTEzMzIzNDk1OF19
-->