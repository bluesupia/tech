
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
* 시간(Time)은 그라니치 표준시(GMT) 1970년 1월 1일 밤 12시 이후부터 밀리세컨드로 측정한 long형 정수로 표현
* 시간 측정의 시작 시점은 신기원(the epoch)
	* 이값은 부호가 있으며 음수값은 신기원 전
### Calendar
* 시간의 추이를 표시
* 대부분의 나라에서는 교황 그레고리 13세 이후부터 그레고리안 달력을 사용
* 달력 추상 개념
	* Calendar
	* TimeZone
	* java.text.DateFormat
* 달력의 구체적인 구현 클래스
	* GregorianCalendar
	* SimpleTimeZone - GregorianCalendar 와 함께 사용
	* java.text.SimpleDateFormat 클래스 - 그레고리안 날짜와 시간을 원하는 형식으로 표현하고 파싱함

### 시간대
* TimeZone은 추상클래스로서 GMT 오프셋과 섬머타임같은 다른 오프셋도 포함
### GregorianCalendar와 SimpleTimeZone
## 날짜와 시간을 포맷팅하고 파싱하기
### Fomatter를 사용하여 날짜와 시간 출력하기
## 텍스트의 국제화와 지역화
### 컬레이션
### 포맷팅과 파싱
### 텍스트 경계

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4OTk5MTA1NTQsMTcxMTA0OTA3Nl19
-->