### 20190318
#API
* REST API
	* 어느날 클라에서 요청이 온다. 기존 PATCH /content/1로 사용하던 수정 API를 명시적으로 /content/1/dup
https://meetup.toast.com/posts/92

### 20190313
#java #code
* `return null`을 지양하자
	* from / 코드리뷰를 하다보면 `null`을 특정 케이스로 사용하는 경우가 있는데요, 이런건 지양해 주시면 좋습니다. null이란건 NPE의 원인이 되기도 하고 알아보기 힘들기 때문에 특정 값을 정의하시거나 `Optional`을 쓰시는게 좋습니다.
	

### 20190312
#gradle
shadowJar
* jar 파일에 모든 라이브러리를 넣어주는 플러그인
* fat jar
https://blog.leocat.kr/notes/2017/10/11/gradle-shadowjar-make-fat-jar

### 20190305
#java
필드 주입을 피하자
* 필드 주입의 장점은 코드량 뿐 ?
	* **코드량이 줄어드는 장점( boilerplate 제거)**
* 단점
	* #### **첫째, DI 컨테이너와 결합이 매우 강하게 되어 외부에서 사용하기 어려워 진다.**
	* #### **둘째, 의존 관계가 보이지 않는다.**
* 필드 주입대신 ?
	* 생성자 주입 : 필수적이며 불변 프로퍼티의 경우
	* 수정자 주입 : 선택적이며 가변 프로퍼티의 경우

* https://ohjongsung.io/2017/06/02/%ED%95%84%EB%93%9C-%EC%A3%BC%EC%9E%85-field-injection-%EC%9D%84-%ED%94%BC%ED%95%98%EC%9E%90

### 20190304
#java
**java.lang.InstantiationError**
* 어플리케이션이 Java `new` 구문을 사용해 abstract 클래스나 인터페이스의 인스턴스를 생성하려고 했을 때 발생하는 Error
http://cris.joongbu.ac.kr/course/java/api/java/lang/InstantiationError.html
### 20190303
Locale 찾다가... 
SIMPLIFIED_CHINESE / TRADITIONAL_CHINESE
```java
private Locale toLocale(String language) {  
    if (StringUtils.isBlank(language)) {  
        return Locale.ENGLISH;  
  }  
  
    if (StringUtils.startsWith(language, "zh")) {  
        return StringUtils.startsWith(language, "zh_cn") ? Locale.SIMPLIFIED_CHINESE : Locale.TRADITIONAL_CHINESE;  
  }  
  
    return Locale.forLanguageTag(language);  
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzk1Njc2Nzg4LC0yMDY4MjY3NzM0LDY3Nj
MxMTA2NywxMzUwMDc4ODMxLC0xNzY5OTkwNjYwLC0xNDkxNTgx
MTYxLC04NTgwOTU2MTVdfQ==
-->