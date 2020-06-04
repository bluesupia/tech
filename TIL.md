#### 20200604
Intellij Test 실패 발생시 No tests found for given includes  [https://adunhansa.tistory.com/236](https://adunhansa.tistory.com/236)

#### 20200530
[https://www.oracle.com/technetwork/java/javase/tech/index-jsp-140228.html](https://www.oracle.com/technetwork/java/javase/tech/index-jsp-140228.html)
#### 20200525
&#35;exception
[https://cheese10yun.github.io/checked-exception/](https://cheese10yun.github.io/checked-exception/)

#### 20200514
&#35;SocketException
- java.net.SocketException: Connection reset by peer: socket write error  
원인: write 시 상대방 socket close 된 경우  
  
- java.net.SocketException: Connection reset  
원인: read 시 상대방 socket close 된 경우  
  
- java.io.IOException: Broken pipe  
원인: receiver에서 송신받은 데이터를 제때 처리하지 못하는 상황(네트워크가 느리거나 서버의 CPU가 max인 경우 등)에서 sender가 계속 보내는 경우  
  
결 론적으로 위의 두 종류의 Exception은 데이터를 요청한 측(브라우저 등)에서 서버에서 데이터를 다 보내기 전에 강제로 닫아 버리거나 종료해버릴때 발생하는 것이기 때문에 큰 문제는 되지 않는다. 하지만 Broken Pipe의 경우 정상적으로 데이터를 보내지만 받지 못하는 상황이 발생하기 때문에 문제가 된다.  
  
  
출처: [https://multifrontgarden.tistory.com/46](https://multifrontgarden.tistory.com/46) [우리집앞마당]  
 
#### 20200409
&#35;naming convention &#35;pr
* static factory method convention
[https://docs.oracle.com/javase/tutorial/datetime/overview/naming.html](https://docs.oracle.com/javase/tutorial/datetime/overview/naming.html)

#### 20200406
&#35;pattern &#35;pr
* Observer pattern
[https://ko.wikipedia.org/wiki/%EC%98%B5%EC%84%9C%EB%B2%84_%ED%8C%A8%ED%84%B4](https://ko.wikipedia.org/wiki/%EC%98%B5%EC%84%9C%EB%B2%84_%ED%8C%A8%ED%84%B4)

#### 20200129
&#35; dic &#35;pr
* cut
[http://wiki.c2.com/?ClassUnderTest](http://wiki.c2.com/?ClassUnderTest)
테스트 대상 클래스는 `cut`라고 명명해 주시면 코드 읽는데 도움이 됩니다

#### 20191028
&#35;나는멍청이

* No content to map due to end-of-input
 ``
com.fasterxml.jackson.databind.JsonMappingException: No content to map due to end-of-input
 at [Source: java.io.StringReader@421ea4c0; line: 1, column: 1]
``
- 상단에 response.body().string()으로 로깅 해두고, 그다음 response.body().bytes() 로 시도하려니 .... ;ㅁ;

- 아무거나 로깅하지 말자 ?

#### 20191015
&#35;mongodb
- query 조건에 shard key 가 포함되어야, 특정 샤드로 target query 가 수행(라우터는 데이터 위치를 shard key 기준으로만 가지고있음)
- 만약 _id 만 조건에 포함된다면, 모든 샤드들을 대상으로 브로드캐스트로 쿼리가 실행

#### 20191003
&#35;kafka
* offset 이동!
kafka-consumer-groups.sh --bootstrap-server AAA -group BBB --topic CCC --reset-offsets --to-offset DDD --execute

#### 20191001
&#35;log4j
* 로그레벨을 알아보자
* TRACE > DEBUG > INFO > WARN > ERROR > FATAL 순 입니다.

  
#### 20190930
&#35;mongo
* 예상사이즈를 예상해보자
Object.bsonsize({})

* [https://blog.outsider.ne.kr/941](https://blog.outsider.ne.kr/941)

&#35;codereview
* 구글 코드리뷰 가이드
* [https://soojin.ro/review/](https://soojin.ro/review/)
* 

#### 20190923
&#35;linux &#35;shell
df : 디스크의 남은 용량을 확인
- **df -k** : 킬로바이트 단위로 현재 남은 용량을 확인

- **df -m** : 메가바이트 단위로 남은 용량을 왁인

- **df -h** : 보기 좋게 보여줌

- **df .** : 현재 디렉토리가 포함된 파티션의 남은 용량을 확인

  
du : 현재 디렉토리에서 서브디렉토리까지의 사용량을 확인

- **du -a** : 현재 디렉토리의 사용량을 파일단위 출력

- **du -s** : 총 사용량을 확인

- **du -h** : 보기 좋게 바꿔줌

- **du -sh *** : 한단계 서브디렉토리 기준으로 보여준다.

  
  
출처: [https://ra2kstar.tistory.com/135](https://ra2kstar.tistory.com/135) [초보개발자 이야기.
#### 20190917
&#35;spring
@Qualifier
* 같은 타입의 Bean 존재할때 지정
* [https://n1tjrgns.tistory.com/163](https://n1tjrgns.tistory.com/163)

### 20190905ˇ
&#35;kafa

brew install kafka

### 20190821
&#35;mongo
* FCV 변경 ?
```
db.adminCommand({getParameter:1, featureCompatibilityVersion:1})
```

### 20190816
java.lang.UnsupportedOperationException 을만나다
```
List<String> strings = List.of("first", "second");  
strings.addAll(CollectionUtils.EMPTY_COLLECTION);
```
* in my proj.
	* 빈 컬렉션을 넣는게 잘못된줄..
	* 알고보니 of... 는 immutableCollection 반환

### 20190727
&#35;java &#35;spring
**Bean 메서드 생명주기**
* in my proj.
* 나는 스태틱 메소드를 만들고 싶었다.
* 스태틱 메소드에서는 생성자에 있는 로직이 실행되지 않는다. 생성자는 non-static이니깐
* bean 생성 후 초기화하는 작업을 한 메소드를 호출하고 싶다
* 1차. static {}
* 2차. @PostContruct
* 3차. InitializingBean
* 4차. @Bean(method = "init")
* 
[http://wonwoo.ml/index.php/post/1820](http://wonwoo.ml/index.php/post/1820)

### 20190717
&#35;codereview 
**single character concatenation**
* '+' vs "+"
[https://stackoverflow.com/questions/24859500/concatenate-char-literal-x-vs-single-char-string-literal-x](https://stackoverflow.com/questions/24859500/concatenate-char-literal-x-vs-single-char-string-literal-x)

&#35;q&a

**curl**
- 요청 후 응답이 간헐적으로 NOT FOUND가 나고 있다
- 시스템쪽 확인을 위해 관리자가 아래처럼 호출테스트 결과를 보냄
```
$ curl -I -H 'host: CDN' URL
```
- curl option중에 -I 는 헤더만 가져가기 때문에 CDN에 캐싱되지 않음
- cdn Test시에 -I 보다는 -o /dev/null -v 옵션을 사용하셔야 정확한 결과를 얻을 수 있음!
### 20190613
&#35;elasticsearch
### bool 쿼리
bool 쿼리는 bool 필터와는 다르게 동작한다.
-   must: bool 필터와 동일
-   must_not: bool 필터와 동일
-   should: minimum_should_match(기본값=1) 값보다 크면 결과에 포함
-   minimum_should_match: 기본값=1, (**must가 함께 사용되면 기본값=0**). 퍼센트, 실수 사용가능
[https://bakyeono.net/post/2016-08-20-elasticsearch-querydsl-basic.html](https://bakyeono.net/post/2016-08-20-elasticsearch-querydsl-basic.html)
- 
### 20190503
&#35;gradle &#35;complie &#35;codereview

```
<!--  Add the lines below to get more details about that warning --> 
allprojects {
    gradle.projectsEvaluated {
        tasks.withType(JavaCompile) {
            options.compilerArgs << "-Xlint:unchecked" << "-Xlint:deprecation"
        }
    }
}
```


### 20190401
&#35;java &#35;spring &#35;annotation
* @Configuration
[http://tech.javacafe.io/spring/2018/11/04/%EC%8A%A4%ED%94%84%EB%A7%81-Configuration-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98-%EC%98%88%EC%A0%9C/](http://tech.javacafe.io/spring/2018/11/04/%EC%8A%A4%ED%94%84%EB%A7%81-Configuration-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98-%EC%98%88%EC%A0%9C/)

### 20190324
&#35; effectivejava
* 슈퍼타입토큰
	* List&lt;String&gt;.class
* 참고!
https://homoefficio.github.io/2016/11/30/%ED%81%B4%EB%9E%98%EC%8A%A4-%EB%A6%AC%ED%84%B0%EB%9F%B4-%ED%83%80%EC%9E%85-%ED%86%A0%ED%81%B0-%EC%88%98%ED%8D%BC-%ED%83%80%EC%9E%85-%ED%86%A0%ED%81%B0/

### 20190323
&#35; effectivejava
* 힙 오염(Heap pollution)
	* 매개변수화 타입의 변수가 타입이 다른 객체를 참조하는 경우 발생.

### 20190320
&#35; markdown 특수문자
| Symbol |  HTML Number|  HTML Name|  HTML Description|
|--|--|--|--|
| `#` | `&#35;` ||number sign|
| `<` | `&#60;` |`&#lt;`|less than sign|
| `>` | `&#62;` |`&#gt;`|greater than sign|

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
eyJoaXN0b3J5IjpbLTEzODI1NTcyMjYsMTMzNjkzNzg0OCwtMj
EwMzc3MjM2MSwxNDYzMDcxNzcxLC0xNzM3NDQ0NDg3LC0xNjMy
Mjg0NDQ0LDE0MzcxMjIyNzEsNjgzMTI4NDQzLC0yMDM5ODY1Mz
Q3LC02NTYzMTcyNTksMjA4MDI2MDQzMSwtMTIwOTM2ODczMSw3
OTA4MTY5NTksLTEyMTUxMDEyNDcsLTk3NTkxODQ2LC03Njc1OT
QyNzAsLTExMjc0NTE1NTMsMTcwOTgxNDQyOCwyMDM1NDYxNTYx
LDEyMTYxNzEzNDBdfQ==
-->