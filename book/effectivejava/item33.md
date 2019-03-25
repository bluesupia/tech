## 아이템33. 타입 안전 이종 컨테이너를 고려하라
* 타입 안전 이종 컨테이너 패턴
	* 키를 매개변수화
	* 컨테이너에 값을 넣거나 뺄 때 매개변수화한 키를 함께 제공
```java
// 타입 안전 이종 컨테이너 패턴 - API
public class Favorites {
	public <T> void putFavorite(Class<T> type, T instance);
	public <T> T getFavofite(Class<T> type);
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTY4OTgzNTMxXX0=
-->