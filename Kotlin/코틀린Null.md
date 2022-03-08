# Null safeTy

```
Kotlin's type system is aimed at eliminating the danger of null references
```

- 널참조의 위험을 제거하기 위한 시스템


## 명시적 null xkdlq

- 코틀린에서는 기본적으로 Null이 불가능하도록 강제하고 있지만, 자바와의 상호 운용성 등의 이유로 코틀린에서도 null이 사용되므로 null이 가능한 타입을 제공하고 있다.
일반 타입 뒤에 '?'를 붙이면 Null을 할당할 수 있는 타입을 의미한다.

```kotlin
var name: String? = null // null을 할당할 수 있다.
```


## doulble- bang 연산자(non-null 단언 연산자)

- double-bang 연산자라고 불리는 '!!' non-null 단언 연산자를 이용하여 null을 처리할 수 있다. 

- null이 절대 할당되지 않는다는 의미로 쓰여진다.


## 안전 호출 연산자(safe call operator)

- 안전 호출 연산자 '?.'이며 null값을 검사하여 null이면 건너뛰고 null이 아닐 때는 호출된 연산을 실행한다.

```
val name: String? = null
println(name?.toString) //null 반환  NPE가 호출되지 않는다.
```

## Elvis 연산자(null 복합 연산자)
- null복합 연산자는 '?:'이다. Elvis연산자는 Java에서 삼항 연산자로 null을 체크하는 로직을 단축한 것과 유사하다.

- Elvis 연산자의 왼쪽 피연산자가 null이면 오른쪽 피연산자를 실행하고 아니면 왼쪽 피연산자의 결과를 반환한다.

```kotlin
    @Test
    fun Elvis() {
        //given
        val name: String? = null
        val testName = name ?: "name이 null이면 이값을 반환"
        println(testName)
    }
```