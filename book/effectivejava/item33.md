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
```java
// 타입 안전 이종 컨테이너 패턴 - Client
public static void main(String[] args) {
	Favorites f = new Favorites();

	f.putFavorite(String.class, "Java");
	f.putFavorite(Integer.class, 0xcafebabe);
	f.putFavorite(Class.class, Favorites.class);

	String favoriteString = f.getFavorites(String.class);
	int favoriteInteger = f.getFavorites(Integer.class);
	Class<?> favoriteClass = f.getClass(Class.class);

	System.out.printf("%s %x %s%n", favoriteString, favoriteInteger, favoriteClass.getName());
}
```
* 타입 안전 이종 컨테이너 패턴 - 구현
```java
public class Favorites {
	private Map<Class<?>, Object> favorites = new HashMap<>();
	public <T> void putFavorite(Class<T> type, T instance) {
		favorites.put(Objects.requireNonNull(type), instance);
	}
	
	public <T> 
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAyNDIzMTU0Ml19
-->