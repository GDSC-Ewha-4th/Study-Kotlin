# Kotlin in Action Chap.9 QnA🔍

## #****Start Projection : 타입 인자 대신 * 사용****

제네릭 타입 인자 정보가 없음을 표현하기 위해 **Star Projection**을 사용할 수 있고, 이는 `List<*>`와 같이 사용 가능하다.

첫째, MutableList<>는 `MutableList<Any?>`*와 **같지 않다**.* `MutableList<Any?>`는 모든 타입의 원소를 담을 수 있다는 사실을 알 수 있는 리스트다. 반면 `MutableList<*>`는 어떤 정해진 구체적인 타입의 원소만을 담는 리스트지만 그 원소의 타입을 정확히 모른다는 뜻이다.

```kotlin
>>> val list: MutableList<Any?> = mutableListOf('a', 1, "qwe")
>>> val chars = mutableListOf('a', 'b', 'c')
>>> val unknownElements: MutableList<*> =                
...         if (Random().nextBoolean()) list else chars
>>> unknownElements.add(42) // 컴파일러는 이 메소드 호출을 금지한다.                              
Error: Out-projected type 'MutableList<*>' prohibits
the use of 'fun add(element: E): Boolean'
>>> println(unknownElements.first()) // 원소를 가져와도 안전하다. first()는 Any? 타입의 원소를 반환한다. 
a
```

위의 예시에서 컴파일러는 `MutableList<*>`를 아웃 프로젝션 타입으로 인식하는데, 여기서의 `MutableList<*>`는 `MutableList<out Any?>`와 동일하게 동작한다. 어떤 리스트의 원소 타입을 모르더라도 그 리스트에서 안전하게 `Any?` 타입을 꺼내오는 것은 가능하지만, 타입을 모르는 리스트에 원소를 마음대로 넣는 것은 불가능하다.

## #****실체화한 타입 파라미터(Reified type parameters)****

****실체화한 타입 파라미터****를 사용하면 런타임에 인라인 함수 호출에서 타입 인자로 사용되는 특정 형식을 참조할 수 있다. (일반 클래스나 함수의 경우, 형식 인수는 런타임에 지워지기 때문에 이 작업은 불가능하다.)

즉, Reified 키워드를 사용하면 Generics function에서 Runtime에 타입 정보를 알 수 있다. 하지만 inline function과 함께 사용할 때만 가능하다.

## #****reified 타입 파라미터의 제약****

reified 타입 파라미터는 매우 유용한 도구이지만, 이에도 제약이 존재한다. 

**reified 타입 파라미터는 다음과 같은 경우 사용할 수 있다.**

- 타입 검사와 캐스팅 (`is,!is,as,as?`)
- 10장에서 설명할 코틀린 리플렉션 API(`::class`)
- 코틀린 타입에 대응하는 `java.lang.Class` 얻기 (`::class.java`)
- 다른 함수를 호출할 때 타입 인자로 사용

**하지만 다음과 같은 작업은 불가능**

- 타입 파라미터 클래스의 인스턴스 생성하기
- 타입 파라미터 클래스의 동반 객체 메소드 호출하기
- reified 타입 파라미터를 요구하는 함수를 호출하면서 reified 하지 않은 타입 파라미터로 받은 타입을 타입 인자로 넘기기
- 클래스, 프로퍼티, 인라인 함수가 아닌 함수의 타입 파라미터를 reified로 지정하기

reified가 인라인 함수에만 사용할 수 있기 때문에, reified를 사용하면 무조건 람다가 인라이닝이 된다. 따라서 람다를 인라이닝하고 싶지 않은 경우에는 `noinline` 키워드를 쓸 수 있다.

## #무공변(invariance) : 타입 관계 없음

무공변(invariance)이란, **타입 S가 T의 하위 타입일 때, Box[S]와 Box[T] 사이에 상속 관계가 없는 것**을 나타낸다.

## #****covariant(공변성) : 하위 타입 관계 유지****

공변(covariance)는 **타입 S가 T의 하위 타입일 때, Box[S]가 Box[T]의 하위 타입** 임을 나타내는 개념이다. 이를 공변 관계가 유지된다고 말한다. 코틀린에서는 공변을 <out T>로 선언하면 됩니다.

## #****contravariance(반공변) : 뒤집힌 하위 타입 관계****

반공변(contravariance)은 **타입 S가 T의 하위 타입일 때, Box[S]가 Box[T]의 상위 타입**임을 나타내는 개념이다.

## #in과 out 위치

그런데 아무 클래스나 공변적으로 만든다면 굉장히 불안정해질 것이다. 타입 안정성을 보장하기 위해서 클래스의 맴버를 `in`과 `out` 포지션으로 나누어야 한다.

즉, 클래스 멤버를 선언할 때 타입 파라미터를 사용할 수 있는 지점은 모두 `인(in)`과 `아웃(out)`위치로 나뉜다. 만약 T가 함수의 반환 타입에 쓰인다면 T는 `아웃` 위치에 있다. 그 함수는 T 타입의 값을 생산한다. T가 함수의 파라미터 타입에 쓰인다면 T는 `인` 위치에 있다.

| covariant, 공변성 | contravariance, 반공변성 | invariant, 무공변 |
| --- | --- | --- |
| Producer<out T> | Consumer<in T> | MutableList |
| 타입 인자의 subtype 관계가 제네릭 타입에서도 유지 | 타입 인자의 subtype 관계가 제네릭 타입에서 역전 | subtype 관계 성립 안함 |
| Producer<Cat>은 Producer<Animal>의 subtype | Consumer<Animal>은 Consumer<Cat>의 subtype |  |
| T를 out위치에만 사용 가능
(반환 타입에서만 사용) | T를 in위치에만 사용 가능
(파라미터 위치에만 사용) | T를 아무데나 사용 가능
(즉, 반환과 파라미터에서 사용) |