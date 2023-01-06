# Kotlin in Action Chap.7 QnA🔍

## #****동등성 연산자 : eqauls****

| Expression | java | kotlin |
| --- | --- | --- |
| == | 원시 타입은 값을 비교/참조 타입은 주소값 | 참조 타입은 equals로 자동 컴파일 |
| equals | 참조 타입을 값을 비교 | ==이 사용되기 때문에 거의 사용되지 않음 |
| === | 존재하지 않음 | 주소값 비교 |

`==`

**자바**: 원시 타입에서는 값 비교, 객체의 비교에서는 참조값(주소)를 비교

**코틀린**: 원시 타입과 객체에 상관없이 equals 메소드를 수행하는 관례(s1 == s2 는 내부적으로는 s1.equals(s2)가 수행)

`equals`

**자바**: 참조 타입의 값을 비교하는 것과 같음

`===`

**코틀린**: 자바에서의 참조값(주소) 비교와 같게 사용

## #구조 분해 선언

구조 분해를 사용하면 **객체가 가지고 있는 여러 값을 분해해서 여러 변수에 한꺼번에 초기화할 수 있다.** 

```kotlin
/* 구조 분해 간단한 예제 */
>>> val car = Car("현대", "그랜저 IG")
>>> val (manufacturer, model) = car  //변수를 선언함과 동시에 car의 여러 컴포넌트로 초기화 합니다.
>>> println(manufacturer)
현대
>>> println(model)
그랜저 IG
```

구조 분해 선언은 일반 변수 선언과 비슷해 보이지만 **"=" 의 좌변에 여러 변수를 괄호로 묶었다는 점**에서 차이가 있다. 구조 분해 선언의 내부에서는 각 변수를 초기화하기 위해 **componentN**이라는 함수를 호출하게 되며, 여기서 N은 구조 분해 선언에 있는 변수 위치에 따라붙는 번호이다.

데이터 클래스의 주 생성자에 들어있는 프로퍼티에 대해서는 컴파일러가 자동으로 componentN 함수를 만들어준다. 구조 분해 선언은 데이터 클래스의 특징이기 때문에, 일반 클래스에서 사용하고 싶다면 다음과 같이 operator 키워드를 명시적으로 사용해야 한다.

```kotlin
/* 일반 클래스에서 구조 분해 선언하는 예제 */
class Point(val x: Int, val y: Int) {
	operator fun component1() = x
	operator fun component2() = y
}
```

## #복합 대입 연산자 오버로딩

경우에 따라 `+=` 연산이 객체에 대한 참조를 다른 참조로 바꾸기보다 원래 객체의 내부 상태를 변경하게 만들고 싶을 때가 있다. **변경 가능한 컬렉션**에 원소를 추가하는 경우가 대표적인 예다.

코틀린 표준 라이브러리는 변경 가능한 컬렉션에 대해 `plusAssign` 을 정의한다. 이를 보면 MutableCollection이라는 것을 알 수 있다.

```kotlin
operator fun <T> MutableCollection<T>.plusAssign(element: T) {
		this.add(element)
}
```

이론적으로, 코드에서 `+=`를 사용할 때, plus와 plusAssign 함수가 모두 호출될 수 있다. 이런 경우에 두 함수가 모두 정의되어 있고 실행 가능한 상황이라면, 컴파일러는 오류를 호출할 것이다. 이럴 경우 해결할 수 있는 방법 중 하나는 `var`(가변)을 `val`(불변)로 바꾸어 plusAssign을 실행 불가능하게 만드는 것이다. 왜냐하면 `val`은 변경 불가능한 프로퍼티이기 때문에 `plusAssign` 이 적용되지 않는다. 

경우에 따라 += 연산을 정의하여 변수가 참조하는 개체를 수정하되 **참조를 재할당하지 않는 것**이 좋다. 이러한 경우 중 하나는 가변 컬렉션에 요소를 추가하는 것이다. 즉, 참조를 바꾸지 않으면서 내부 값을 바꾸는 것이다.

```kotlin
>>> val numbers = ArrayList<Int>()
>>> numbers += 42
>>> println(numbers[0])
42
```

## #도우미 프로퍼티

위임 프로퍼티의 일반적인 문법은 다음과 같다. 

```kotlin
class Foo {
var p: Type by Delegate()
}
```

프로퍼티 p는 다른 객체에게 자신의 로직을 위임한다. 이 예시에서는 `Delegate` 클래스가 된다. 이는 `by` 키워드를 통해 뒤에 있는 식을 계산해서 위임에 쓰일 객체를 얻을 수 있다.

```kotlin
class Delegate {
	operator fun getValue(...) { ... } //getter에 대한 로직을 가지고 있음
	operator fun setValue(..., value: Type) { ... } //setter에 대한 로직을 가지고 있음
}
class Foo {
	var p: Type by Delegate() //by 키워드는 프로퍼티를 위임 객체와 관계맺음
}
>>> val foo = Foo()
>>> val oldValue = foo.p //foo.p는 delegate.getValue(…)를 호출함
>>> foo.p = newValue //프로퍼티 값을 delegate.setValue(…, newValue)를 호출해 바꿈
```

foo.p처럼 접근하였지만, 이는 사실 도우미 프로퍼티인 Delegate 타입이 호출된 것이다. 이렇게 사용하면 Foo 클래스 내부에서 getter, setter를 사용할 필요가 없이 다른 클래스가 만든 getter와 setter를 위임으로 간편하게 가져다 쓸 수 있다.

**참고: 위임을 사용하는 이유**

```kotlin
class Kia(private val premiumCar: Car) : Car{
	override val tire: String = premiumCar.tire
	override val body: String = premiumCar.body
	override fun manufacture() = premiumCar.manufacture()
} //클래스의 모든 내용이 다 상속받는 내용밖에 없기 때문에, 굳이 보일러플레이트 코드로 만들 필요가 없다.

class DelegatedKia(premiumCar: Car) : Car by premiumCar
********************************************************************//한 줄로 간단하게 모두 위임할 수 있다.
```