
# 24. 국제화와 지역화
**Write once, run anywhere**
* 국제화(Internationalization) : 프로그램을 유연하게 작성하는 것
* 지역화(Localization) : 국제화 도구를 사용하여 프로그램을 특정 로케일(Locale)에 적합하게 만드는 것 (ex. 메시지를 로케일 언어로 번역)
## Locale
* java.util.Locale : 장소, 즉 문화적, 정치적, 지리적인 것을 기술
## 리소스 번들
* 추상 클래스 ResourceBundle은 번들 내의 자원을 문자열 키로 찾을 수 있는 메소드와 키가 없을 경우 검색 대상으로 부모 번들을 제공하는 메소드를 제공
* **in my proj.**
	* Spring 의 ResourceBundleMessageSource 사용

### ListResourceBundle
### PropertyResourceBundle
### ResourceBundle의 서브 클래스 만들기
## Currency
* 통화(Currency) 인코딩은 로케일과 매우 많은 관련이 있고, 통화값을 적적하게 표현하기 위해서는 java.util.Currency 클래스 사용
## Time, Date, Calendar
* 시간은
### Calendar
### 시간대
### GregorianCalendar와 SimpleTimeZone
## 날짜와 시간을 포맷팅하고 파싱하기
### Fomatter를 사용하여 날짜와 시간 출력하기
## 텍스트의 국제화와 지역화
### 컬레이션
### 포맷팅과 파싱
### 텍스트 경계

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbODA5Njc2ODE5LDE3MTEwNDkwNzZdfQ==
-->