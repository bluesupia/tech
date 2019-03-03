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
eyJoaXN0b3J5IjpbLTg1ODA5NTYxNV19
-->