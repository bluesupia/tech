### 20190320
&#35; markdown 특수문자
|  |  |
|--|--|
| `#` | `&#35;` |
| `#` | `&#35;` |
https://www.ascii.cl/htmlcodes.htm
### 20190319-2
* ::
	* method reference
	* double colon operator

### 20190319
&#35;java &#35;annotation
* @SuppressWarnings
	* **컴파일러가** 일반적으로 **경고하는 내용** 중  
"이건 하지마"하고 **제외**시킬 때  

-   all  모든 경고를 억제합니다.
-   boxing  boxing/unboxing 오퍼레이션과 관련된 경고를 억제합니다.
-   cast  캐스트 오퍼레이션과 관련된 경고를 억제합니다.
-   dep-ann  권장되지 않는 어노테이션과 관련된 경고를 억제합니다.
-   deprecation  권장되지 않는 기능과 관련된 경고를 억제합니다.
-   fallthrough  switch 문에서 누락된 break 문과 관련된 경고를 억제합니다.
-   finally  리턴되지 않는 마지막 블록과 관련된 경고를 억제합니다.
-   hiding  변수를 숨기는 로컬과 관련된 경고를 억제합니다.
-   incomplete-switch  switch 문에서 누락된 항목과 관련된 경고를 억제합니다(enum case).
-   javadoc  javadoc 경고와 관련된 경고를 억제합니다.
-   nls  비nls 문자열 리터럴과 관련된 경고를 억제합니다.
-   null  널(null) 분석과 관련된 경고를 억제합니다.
-   rawtypes  원시 유형 사용법과 관련된 경고를 억제합니다.
-   resource  닫기 가능 유형의 자원 사용에 관련된 경고 억제
-   restriction  올바르지 않거나 금지된 참조 사용법과 관련된 경고를 억제합니다.
-   serial  직렬화 가능 클래스에 대한 누락된 serialVersionUID 필드와 관련된 경고를 억제합니다.
-   static-access  잘못된 정적 액세스와 관련된 경고를 억제합니다.
-   static-method  static으로 선언될 수 있는 메소드와 관련된 경고를 억제합니다.
-   super  수퍼 호출을 사용하지 않는 메소드 겹쳐쓰기와 관련된 경고를 억제합니다.
-   synthetic-access  내부 클래스로부터의 최적화되지 않은 액세스와 관련된 경고를 억제합니다.
-   sync-override  동기화된 메소드를 오버라이드하는 경우 누락된 동기화로 인한 경고 억제
-   unchecked  미확인 오퍼레이션과 관련된 경고를 억제합니다.
-   unqualified-field-access  규정되지 않은 필드 액세스와 관련된 경고를 억제합니다.
-   unused  사용하지 않은 코드 및 불필요한 코드와 관련된 경고를 억제합니다.
  
### in my proj.
* Duplictes
* rawtypes
* unchecked
* unused
* UnstableApiUsage

https://www.ibm.com/support/knowledgecenter/ko/SSRTLW_9.6.1/org.eclipse.jdt.doc.user/tasks/task-suppress_warnings.htm
출처: [https://jinwoonote.tistory.com/entry/SuppressWarnings-이건-뭐지](https://jinwoonote.tistory.com/entry/SuppressWarnings-%EC%9D%B4%EA%B1%B4-%EB%AD%90%EC%A7%80) [노트]

### 20190318
&#35;API
* REST API
	* 어느날 클라에서 요청이 온다. 기존 PATCH /content/1로 사용하던 수정 API를 명시적으로 /content/1/update 로 바꿀 수 없을까요 ? REST API 규칙을 따르고 있어서 어렵다고 말하고 싶은데 ... 제대로 알고 있나 확인해보자
	* 가장 중요한 규칙
		* **첫 번째,**  URI는 정보의 자원을 표현해야 한다.  
		* **두 번째,**  자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.
https://meetup.toast.com/posts/92

### 20190313
&#35;ava &#35;code
* `return null`을 지양하자
	* from / 코드리뷰를 하다보면 `null`을 특정 케이스로 사용하는 경우가 있는데요, 이런건 지양해 주시면 좋습니다. null이란건 NPE의 원인이 되기도 하고 알아보기 힘들기 때문에 특정 값을 정의하시거나 `Optional`을 쓰시는게 좋습니다.
	

### 20190312
&#35;gradle
shadowJar
* jar 파일에 모든 라이브러리를 넣어주는 플러그인
* fat jar
https://blog.leocat.kr/notes/2017/10/11/gradle-shadowjar-make-fat-jar

### 20190305
&#35;java &#35;code
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
&#35;java &#35;error
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
eyJoaXN0b3J5IjpbLTE1NDE3MDYzNDksLTEyNTY4NDAxNDgsMT
Q1MzAyMjA0MywxODU4OTIzMTIsMTQzODYwMTA4NiwtMjA2ODI2
NzczNCw2NzYzMTEwNjcsMTM1MDA3ODgzMSwtMTc2OTk5MDY2MC
wtMTQ5MTU4MTE2MSwtODU4MDk1NjE1XX0=
-->