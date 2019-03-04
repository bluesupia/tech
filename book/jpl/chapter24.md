
# 24. 국제화와 지역화
**Write once, run anywhere**
* 국제화(Internationalization) : 프로그램을 유연하게 작성하는 것
* 지역화(Localization) : 국제화 도구를 사용하여 프로그램을 특정 로케일(Locale)에 적합하게 만드는 것 (ex. 메시지를 로케일 언어로 번역)
## Locale
* java.util.Locale : 장소, 즉 문화적, 정치적, 지리적인 것을 기술
## 리소스 번들
* 추상 클래스 ResourceBundle은 번들 내의 자원을 문자열 키로 찾을 수 있는 메소드와 키가 없을 경우 검색 대상으로 부모 번들을 제공하는 메소드를 제공
* **in my proj.**
* Spring > ResourceBundleMessageSource

### ListResourceBundle
### PropertyResourceBundle
### ResourceBundle의 서브 클래스 만들기
## Currency

## Time, Date, Calendar
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
eyJoaXN0b3J5IjpbNTM2MTQ1NzcxLDE3MTEwNDkwNzZdfQ==
-->