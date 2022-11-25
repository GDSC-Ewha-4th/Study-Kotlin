# Kotlin in Action Chap.6 QnA🔍

## #널로 할 수 있는 일

자바 기준으로는 Wrapper 객체에 넣으면 null을 사용할 수 있다. 따라서 Integer로 받으면 null 값을 사용할 수 있다. 코틀린에서 null을 처리해주기 때문에 자바에서 사용한 null과 비교해줄 수 있다. 코틀린에서는 널러블이 아닌 값에서 오류가 나도 그냥 오류를 발생하기 때문에 어디서 오류가 난 건지 알 수 없는데, null일 경우에는 NullPointerException을 발생하기 때문에 null에 의해 오류가 발생했다는 것을 알 수 있다.

## #널 아님 단언(”!!”) 사용하는 경우

```kotlin
fun ignoreNulls(s: String?) {
	val sNotNull: String = s!!
	println(sNotNull.length)
}
```

위의 예시에서 s가 null일 경우에는 어떤 일이 생길까? **NullPointerException**와 같은 에러가 런타임에 발생한다. 따라서 null 아님 단언은 NPE와 같은 에러를 감수할 때만 사용할 수 있다.

그러나 null 아님 단언을 유용하게 활용할 수 있는 상황도 있는데, **예를 들어 한 함수에서 이미 null이 아님을 체크했고 그 값을 다른 함수에서 활용할 때**, 컴파일러는 해당 값이 null이 아니라는 것을 모른다. 따라서 이를 명시적으로 표현해주어야 된다.

```kotlin
class CopyRowAction(val list: JList<String>) : AbstractAction() {
	override fun isEnabled(): Boolean =
		list.selectedValue != null
	override fun actionPerformed(e: ActionEvent) {
		val value = list.selectedValue!!
		// copy value to clipboard
	}
}
```

만약 위의 상황에서 `!!`를 사용하지 않는다면, `val value = list.selectedValue ?: return`를 통해 non-null 타입을 확보할 수 있다.

## #let 함수

let 함수는 nullable을 다루기 더 쉽게 만들어준다. 주로 null이 불가능한 함수의 파라미터로 nullable한 타입의 값을 넘기려고 할 때 let을 활용한다.

**사용 경우:**

- **null check 후 코드를 실행**해야 하는 경우
- **nullable한 수신 객체를 다른 타입의 변수로 변환**해야 하는 경우

```kotlin
fun sendEmailTo(email: String) { /*...*/ }
>>> val email: String? = ...
>>> sendEmailTo(email)
ERROR: Type mismatch: inferred type is String? but String was expected

//솔루션 중 하나: null이 아님을 체크한다.
if (email != null) sendEmailTo(email)
```

let 함수를 활용하면 위의 문제를 해결할 수 있다. null이 아닐 때는 자동으로 생성된 `it`으로 활용 가능하다.

```java
email?.let { email -> sendEmailTo(email) }
email?.let { sendEmailTo(it) }

fun sendEmailTo(email: String) {
	println("Sending email to $email")
}
>>> var email: String? = "yole@example.com"
>>> email?.let { sendEmailTo(it) }
Sending email to yole@example.com
>>> email = null
>>> email?.let { sendEmailTo(it) }
```

여러 값을 null인지 아닌지 체크해야 할 때는 중첩 let을 활용하여 할 수 있다. 그러나 이럴 경우 굉장히 코드가 복잡해질 수 있기 때문에, 평범한 if 조건문을 활용하는 것이 더욱 권장된다.

<aside>
💡 **참고: scope 함수**

코틀린 표준 라이브러리는 객체의 컨텍스트 내에서 코드 블럭을 실행하기 위한 목적만을 가진 여러가지 함수를 제공한다. 이런 함수들을 람다식으로 호출할 때, 이는 임시로 범위(scope)를 형성한다. 이 범위 내에서는 객체의 이름이 없어도 객체에 접근할 수 있다. 이러한 함수를 *scope functions*라고 한다. 이러한 scope 함수는 `let`, `run`, `with`, `apply`, `also`의 5가지가 있다.

기본적으로, 이 함수들은 동일한 역할을 한다. 이는 바로 객체에 코드 블럭을 실행하는 것이다. 다른 점은, 이 객체를 어떤 방식으로 블럭 안에서 사용할 수 있는지, 그리고 전체 표현식(expression)의 결과가 어떻게 되는 지이다.

[scope 함수](https://shinjekim.github.io/kotlin/2019/12/05/Kotlin-%EC%BD%94%ED%8B%80%EB%A6%B0%EC%9D%98-Scope-%ED%95%A8%EC%88%98/)

</aside>

## #읽기 전용 컬렉션이 항상 스레드 안전하지는 않은 이유

기본적으로 코틀린이 가진 컬렉션은 전부 변경할 수 없는 컬렉션이다. 그런데 `MutableCollection`을 사용하면 컬렉션을 변경할 수 있는 컬렉션을 사용할 수 있다. val과 var을 분리한 것처럼, 읽기 전용 컬렉션과 변경 가능 컬렉션을 분리하면 프로그램 안의 데이터에 무슨 일이 일어나고 있는지 더 쉽게 이해할 수 있다. 만약 Collection을 사용한다면 데이터의 변경은 일어나고 있지 않음을, MutableCollection에 데이터를 넘겨준다면 변경을 의도하고 있는 것임을 알 수 있다.

```kotlin
fun <T> copyElements(source: Collection<T>, target: MutableCollection<T>) {
	for (item in source) {
		target.add(item) **//변경 가능한 컬렉션에 넘겨줌**
	}
}
>>> val source: Collectionthread-safe<Int> = arrayListOf(3, 5, 7)
>>> val target: MutableCollection<Int> = arrayListOf(1)
>>> copyElements(source, target)
>>> println(target)
[1, 3, 5, 7]
```

그렇다고 해서 collection이 가리키는 데이터가 **항상 변경 불가능하다는 것은 아니다.** 다른 레퍼런스는 변경 가능한 `MutableCollection`일 수도 있다! 그런데 다양한 레퍼런스가 하나의 데이터를 가리키고 있을 때 `ConcurrentModificationException`이 발생할 수 있으므로, 읽기 전용과 변경 가능을 구분해주는 것이 필수적이다.

<aside>
💡 **참고: 스레드 안전**

스레드 안전(thread 安全, 영어: thread safety)은 멀티 스레드 프로그래밍에서 일반적으로 어떤 함수나 변수, 혹은 객체가 여러 스레드로부터 동시에 접근이 이루어져도 프로그램의 실행에 문제가 없음을 뜻한다. 보다 엄밀하게는 하나의 함수가 한 스레드로부터 호출되어 실행 중일 때, 다른 스레드가 그 함수를 호출하여 동시에 함께 실행되더라도 각 스레드에서의 함수의 수행 결과가 올바로 나오는 것으로 정의한다.

[스레드 안전이란?](https://gompangs.tistory.com/entry/OS-Thread-Safe%EB%9E%80)

</aside>

## #nullable 타입의 확장

```kotlin
//확장 함수를 호출하는 경우
fun verifyUserInput(input: String?) {
	if (input.isNullOrBlank()) {
		println("Please fill in the required fields")
	}nullable 타입의 확장
}
```

nullable 타입으로 확장 함수를 호출하는 경우, nullable 타입으로 해당 함수를 부를 수 있게 된다. 또한 null 타입을 가질 수 있기 때문에 이를 명시적으로 체크해줘야 한다.

> 어떤 메서드를 호출하기 전에 수신 객체 역할을 하는 변수가 널이 될 수 없다고 보장하는 대신 직접 변수에 대해 메서드를 호출해도 확장 함수인 메서드가 알아서 널을 처리해준다.
> 

위의 말을 해석하면, 파라미터로 변수를 넘겨주기 위해 null이 아님을 체크하고 넘겨주기 보다는, 그냥 일단 값을 받은 후 함수 내에서 null 아님을 체크해주면 된다.

자바에서는 null이 될 수 있는 변수에 대해 확장 함수 정의를 하는 것이 불가능하기 때문에, this 또한 null이 될 수 없다. 그러나 코틀린에서는 널이 될 수 있는 타입의 확장 함수 안에서 this가 널이 될 수 있다. 따라서 코틀린에서는 널이 될 수 있는 타입에 대한 확장을 정의하면 널이 될 수 있는 값에 대해 그 확장 함수를 호출할 수 있다.

> When you declare an extension function for a nullable type (ending with ?), that means you can call this function on nullable values; and `this` in a function body can be `null`, so you have to check for that explicitly. In Java, this is always `not-null`, because it references the instance of a class you’re in. In Kotlin, that’s no longer the case: in an extension function for a nullable type, `this` can be null.
> 

## #시그니처란?

함수의 파라미터 리스트를 함수의 시그니처라고 한다. 즉, 함수 시그니처란 함수의 원형에 명시되는 매개변수 리스트를 가리킨다. 만약 두 함수가 매개변수의 개수와 그 타입이 모두 같다면, 이 두 함수의 시그니처는 같다고 할 수 있다.

반환 타입과 Exception은 시그니처에 Optional로 포함될 수 있다.

## #배열

코틀린에서 배열을 생성하기 위해서는 다음과 같은 경우를 거칠 수 있다.

1. arrayOf 함수로 배열을 생성한다.
2. arrayOfNulls 함수로 null을 포함한 배열을 만든다.
3. Array 생성자로 배열과 람다를 만든다. 각 요소들은 람다로 초기화된다. 배열은 non-null 요소로 초기화한다.