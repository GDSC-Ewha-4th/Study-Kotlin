# Kotlin in Action Chap.2 QnA🔍

## #Property의 정의

코틀린에서는 클래스에 val/var로 정의되는 변수를 프로퍼티(property)라고 한다. 코틀린의 프로퍼티는 자바의 멤버변수(field)와 다르다. 코틀린의 프로퍼티는 자바의 `field + getter/setter 메소드`
라고 할 수 있다. 그 이유는 프로퍼티를 생성함과 동시에 getter/setter가 자동으로 같이 생성되기 때문이다. 이때 `val`은 읽기 전용이기 때문에 `field + getter 메소드`를, `var`은 읽기와 쓰기 모두 가능하기 때문에 `field + getter/setter 메소드`가 된다.

```kotlin
class Person(
    val name: String // 읽기 전용 프로퍼티로 private field와 public getter 함수를 생성
    var age: Int // 쓰기도 가능 프로퍼티로 private field와 public getter, public setter 함수를 생성
    var isMarried: Boolean
)
```

물론, 필요할 경우 개발자가 직접 getter/setter를 생성할 수 있다. 예시는 다음과 같다.

```kotlin
class Rectangle(val height: Int, val width: Int){
    val isSquare: Boolean
        get(){ //개발자가 새롭게 만든 getter 메소드
            return height == width
        }
}
```

이때 프로퍼티 `isSquare`는 값을 저장할 필드가 필요하지 않으며, 개발자에 의해 구현된 getter 함수 만을 가지고 있다는 점을 알 수 있다.

그러나 private val/var은 자바의 field와 동일하다고 말할 수 있다. private은 getter와 setter를 만들지 않기 때문이다.

## #Enum의 정의

`Enum`은 무엇이고, `Enum class`는 왜 필요한 것일까? 우선 Enum의 뜻을 알아보자. Enum은 Enumeration에서 따온 것으로, 열거형이라는 뜻을 가지고 있다. 즉, 서로 연관되거나 또는 관련이 있는 상수들을 열거해 놓은 집합을 말한다.

여기까지는 간단하다. 그렇다면 Enum class는 왜 필요한 것인지 알아보자. 사실 이번 스터디를 하기 전에는 Enum class라는 것을 한번도 사용해본 적이 없기 때문에 사용 이유가 생소하게 다가왔다. 이전까지 나는 다음과 같이 상수를 클래스 내부에서 사용했었다(집합으로 관리해야 할 만큼 많은 상수를 쓰는 코드를 작성해 본 적이 없었다).

```java
public class Person {
    final static String name= "홍길동";
    final static Integer age= 18;
}
```

그러나 이와 같은 방식으로 상수를 끊임없이 정의한다면 추후 상수의 개수가 많아지면서 네임 충돌이 발생할 수 있고 가독성이 낮아지게 된다. 또한 새로운 상수를 추가하면 그 상수에 대한 처리 로직을 계속 만들어줘야 하며, 로직을 작성하지 않을 경우 에러가 생기게 된다.

이런 상황에서 Enum class를 사용하게 되면 특정한 상태가 추가되었을 때 처리해야 하는 코드들을 놓치는 경우가 없어진다.

정리하자면, Enum class는 다음과 같은 특징 덕분에 활용된다.

**1. 코드 가독성 증대**

**2. 특정 상수값에 대한 로직이 없는 것을 방지**

**3. 네임 충돌 방지**

또한 Enum class는 다른 패키지 또는 클래스에서 변경하지 못하게 하고, 인스턴스 생성이나 상속을 방지함으로서 `타입 안정성`을 보장해준다고 알려져 있다. 타입 안정성이 무엇인지는 다음 포스트에서 굉장히 잘 설명해주고 있으므로 링크를 첨부하겠다.

**[Type-safety 란 무엇인가](https://tlonist-sang.github.io/Today-I-learned/jekyll/update/2020/09/29/typed-language.html)**

## #Smart cast

코틀린에서는 개발자가 타입을 캐스팅하지 않아도 컴파일러가 알아서 타입 캐스팅을 하는 기능이 있다. 먼저 다음과 같은 코드가 있다고 가정해보자.

```kotlin
interface Expr
class Num(val value: Int) : Expr
class Sum(val left: Expr, val right: Expr) : Expr
```

위 코드는 왼쪽(left)과 오른쪽(right)을 받아서 Sum에 저장하기 위한 우선 변수를 선언해 놓은 것이다. Sum 값을 반환하기 위해서 새로운 메소드를 작성한다면, 다음과 같이 표현할 수 있다.

```kotlin
fun eval(e: Expr): Int =
        when(e) { //when 활용하여 리턴값 지정
            is Num -> e.value //is 활용하여 타입을 검사한 후 타입 변환 자동으로 거치기
            is Sum -> eval(e.left) + eval(e.right)
            else -> throw IllegalArgumentException("Unknown expression")
        }
fun main(arg: Array<String>){
    println(eval3(Sum(Sum(Num(10), Num(7)), Num(3))))
}
```

`is`라는 키워드는 타입을 검사한 후 자동으로 캐스팅하는 기능을 가지고 있기 때문에(이를 smart cast라고 한다) 타입 변환을 거칠 필요가 없다. 예를 들어, `e as Num`과 같이 개발자가 명시적으로 타입 변환을 할 필요가 전혀 없어지는 것이다.

참고: 여기서 말하는 명시적인 타입 변환이란, 자바의 `Downcasting`을 생각하면 된다. 자바에서는 downcasting을 하기 위해서는 객체 앞에 `(타입)`을 명시해야 한다. 코틀린에서는 그럴 필요가 없다고 말하고 있는 것이다.

## #코틀린의 예외 처리 vs 자바의 예외 처리

우선 코틀린과 자바의 예외 처리를 알기 위해서, *****Checked Exception*****과 *****Unchecked Exception*****의 차이를 알아야 한다. 이 둘은 무엇일까? 간단하게 말하자면, Checked Exception는 명시적인 예외 처리를 강제하며 반드시 `try-catch`로 예외를 잡거나 `throw`로 호출한 메소드에게 예외를 던져야 하는 에러를 말한다. 반면 Unchecked Exception은 위와 같은 명시적인 예외 처리를 강제하지 않는다.

자바에서 상속관계를 따지자면 다음과 같다. RuntimeException을 상속하지 않은 클래스는 Checked Exception, 상속한 클래스는 Unchecked Exception이라고 생각할 수 있다. 쉽게 말해서 Unchecked Exception은 Runtime때 확인되기 때문에 명시적인 예외 처리가 필요 없으며, Checked Exception는 Compile때 확인되기 때문에 명시적인 예외 처리가 꼭 필요하다.

![](https://madplay.github.io/img/post/2019-03-02-java-checked-unchecked-exceptions-1.png)

자바가 Checked와 Unchecked를 구분하는 것과 달리, 코틀린에서는 이 둘을 구분하지 않는다. 그 이유를 Kotlin in Action에서는 다음과 같이 설명~~돌려까기~~하고 있다.

> For example, in listing 2.27, `NumberFormatException` isn’t a checked exception.
Therefore, the Java compiler doesn’t force you to catch it, and you can easily see the exception happen at runtime. **This is unfortunate, because invalid input data is a common situation and should be handled gracefully**. At the same time, the BufferedReader.close method can throw an `IOException`, which is a checked exception and needs to be handled. Most programs can’t take any meaningful action if closing a stream fails, so the code required to catch the exception from the close method is boilerplat.
> 

위의 설명에서는 자바가 `NumberFormatException`이 흔하게 일어나고 주의 깊게 다루어야 하는 예외임에도 불구하고  Unchecked로 다루고 있는 것을 비판하고 있다. 이에 반면, `IOException`의 경우에는 해당 에러가 일어났을 때 프로그램의 의미가 사라지기 때문에 checked로 다루어야 한다고 설명하고 있다. **즉, 코틀린에서는 자바의 이러한 문제점을 극복하기 위해서 checked와 unchecked를 구분하지 않고 모두 checked로 다루기로 한 것이다.**