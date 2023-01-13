# Kotlin in Action Chap.8 QnA🔍

## #****디폴트 값을 지정한 함수 타입 파라미터나 널이 될 수 있는 함수 타입 파라미터****

파라미터를 함수 타입으로 선언할 때도 디폴트 값을 정할 수 있다. joinToString을 예시로 들어보겠다.

```kotlin
fun<T> Collection<T>.joinToString(
    separator: String=",",
    prefix: String="",
    postfix: String=""
): String{
    val result = StringBuilder(prefix)
    for((index, element) in this.withIndex()){
        if(index > 0) result.append(separator)
        result.append(element)
    }
    result.append(postfix)
    return result.toString()
}
```

이 구현은 유연하지만 컬렉션의 각 원소를 문자열로 변환하는 방법을 제어할 수 없다. 위 코드는 StringBuilder.append(o: Any?)를 사용하는데, 이 함수는 항상 객체를 toString 메소드를 통해 문자열로 바꾼다. `toString`으로 충분한 경우도 많지만 그렇지 않을 때도 있다. 이때 원소를 문자열로 바꾸는 방법을 람다로 전달하면 된다. 하지만  `joinToString` 을 호출할 때마다 매번 람다를 넘기게 만들면 함수 호출을 오히려 더 불편하게 만든다는 문제가 있다.

이 때 함수 타입의 파라미터에 대한 디폴트 값을 람다로 지정하면 이런 문제를 해결할 수 있다. **즉, 고차함수를 만들 때 디폴트 값으로 선언하는 방식도 있다.**

```kotlin
fun<T> Collection<T>.joinToString(
    separator: String=",",
    prefix: String="",
    postfix: String="",
    transform: (T) -> String={it.toString()}
): String{
    val result = StringBuilder(prefix)
    for((index, element) in this.withIndex()){
        if(index > 0) result.append(separator)
        result.append(transform(element))
    }
    result.append(postfix)
    return result.toString()
}
```

이 함수는 제네릭 함수다. 따라서 컬렉션의 원소 타입을 표현하는 `T`를 타입 파라미터로 받는다. `transform`람다는 그 `T`타입의 값을 인자로 받는다.

## #invoke 메서드를 구현하는 인터페이스란?

자바에서 코틀린 함수 타입은 인터페이스로 바뀐다. 함수 타입의 변수는 FunctionN 인터페이스를 구현하는 객체를 저장한다. **코틀린 표준 라이브러리는 함수 인자의 개수에 따라 Function0<R> (인자가 없는 함수), Function1<P1, R> (인자가 하나인 함수) 등의 인터페이스를 제공**한다. 각 인터페이스에는 invoke 메소드가 있는데, 이를 호출하면 함수를 실행할 수 있다. 함수 타입인 변수는 인자 개수에 따라 적당한 FunctionN 인터페이스를 구현하는 클래스의 인스턴스를 저장하고 있으며, 그 클래스의 invoke 메소드 본문에는 람다의 본문이 들어간다. **즉, 함수 타입은 invoke 메서드를 구현하는 인터페이스다. 따라서 자바에서보다 코틀린에서 더 코드를 짧게 구현 가능하다.**
**참고: [https://wooooooak.github.io/kotlin/2019/03/21/kotlin_invoke/](https://wooooooak.github.io/kotlin/2019/03/21/kotlin_invoke/)**

```kotlin
/* Kotlin declaration */
fun processTheAnswer(f: (Int) -> Int) {
	println(f(42))
}

/* Java */
>>> processTheAnswer(number -> number + 1);
43

/* Java */
>>> processTheAnswer(
		  new Function1<Integer, Integer>() {
				@Override
				public Integer invoke(Integer number) {
					System.out.println(number);
					return number + 1;
		 }
	 });
43
```

<aside>
📌 **Invoke 메소드란?**

Class Object를 통해 Method 정보를 가져온 주 목적은 해당 Method를 실행하는 것이 주 목적이다. 이때 Method를 동적으로 실행시켜 주는 메소드가 바로 Invoke 메소드다.

</aside>

## #전략 패턴이란?

전략패턴이란 **특정 컨텍스트에서 알고리즘을 별도로 분리하는 설계 방법을 의미한다.** 다른 말로 정의하자면 특정한 기능을 수행하는 데에 있어 다양한 알고리즘이 적용될 수 있는 경우, 이 다양한 알고리즘을 별도로 분리하는 설계 방법을 의미한다.

**참고: [https://velog.io/@kyle/디자인-패턴-전략패턴이란](https://velog.io/@kyle/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EC%A0%84%EB%9E%B5%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80)**

## #인라인이란?

코틀린에서 람다가 변수를 포획하면 람다가 생성되는 시점마다 새로운 무명 클래스 객체(****프로그램 내에서 일시적으로(단발성으로) 한번만 사용되어야 하는 객체)****가 생성된다. 이런 경우는 실행 시점에 무명 클래스의 생성에 부가 비용이 들기 때문에, 일반 함수를 사용한 구현보다 덜 효율적이다.

이와 같은 상황에서 사용하는 것이 **인라인(inline) 키워드인데,** inline 변경자를 **어떤 함수에 붙이면 컴파일러는 그 함수를 호출하는 모든 문장을 함수 본문에 해당하는 바이트코드로 바꿔치기 해준다.**

**참고-메크로 함수와 인라인 함수: [https://kyun2.tistory.com/18](https://kyun2.tistory.com/18)**

## #Function types

`var funOrNull: ((Int, Int) -> Int)? = null`는 함수의 타입 자체가 널이 될 수도 있고, 함수의 반환 타입이 널이 될 수도 있다.