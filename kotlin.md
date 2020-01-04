기간.
2020.01~

방법.
[https://kotlinlang.org/docs/tutorials/koans.html](https://kotlinlang.org/docs/tutorials/koans.html)


Introduction.
1. Simple Functions
```Kotlin
fun MethodName : String = TODO()
```

2. Named arguments
```Kotlin
fun reformat(str: String,
             normalizeCase: Boolean = true,
             upperCaseFirstLetter: Boolean = true,
             divideByCamelHumps: Boolean = false,
             wordSeparator: Char = ' ') {
/*...*/
}

reformat(str, wordSeparator = '_')
```

3. Default Arguments
```Kotlin
fun read(b: Array<Byte>, off: Int = 0, len: Int = b.size) { /*...*/ }
```

4. Lamdas
```
{ a, b -> a + b }
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIzNzU4NTQ3MSwxNjIzNDI2OTY3LDU4NT
c1NTc1OF19
-->