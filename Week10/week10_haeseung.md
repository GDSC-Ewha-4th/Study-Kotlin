# Kotlin in Action Chap.10 QnA🔍

## #애노테이션 클래스 선언과 사용 방법 ****

**애노테이션  메타데이터 ( 부가기능 )을 코드에 비침투적으로 추가하는 것이다. 애노테이션 클래스는 오직 선언이나 식과 관련있는 메타데이터의 구조를 정의하기 때문에 내부에 아무 코드도 들어있을 수 없다.**

```kotlin
annotation class Karol

@Karol
class Sabarada(
    @Karol val first: Int
) {
    @Karol
    fun baz(@Karol second: Int): Int {
        return first + second
    }
}
```

## #커스텀 **애노테이션 파라미터로 제네릭 클래스 활용**

**다음 블로그 참고: [https://bboglebbogle.tistory.com/47](https://bboglebbogle.tistory.com/47)**

기본적으로 JKid는 nonprimitive 타입의 프로퍼티를 중첩 객체로 직렬화한다. 그렇지만 직접 개인적인 `직렬화 로직`을 생성하는 것으로 바꿀 수도 있다.

@CustomSerializer 애노테이션은 커스텀 직렬화 클래스를 인자로 가진다. 직렬화 클래스는 ValueSerializer 인터페이스를 구현해야 한다.

```kotlin
interface ValueSerializer<T> {
	fun toJsonValue(value: T): Any?
	fun fromJsonValue(jsonValue: Any?): T
}
```

@CustomSerializer 애노테이션을 선언하는 방식은 다음과 같다. 이때 애노테이션에 사용될 프로퍼티 타입을 알 수 없기 때문에 스타 프로젝션(*)을 사용할 수 있다.

```kotlin
data class Person(
	val name: String,
	@CustomSerializer(DateSerializer::class) val birthDate: Date
) //DateSerializer라는 클래스가 ValueSerializer 인터페이스를 구현할 것임

annotation class CustomSerializer( //ValueSerializer 인터페이스를 구현한 클래스만 인자로 받음
	val serializerClass: KClass<out ValueSerializer<*>>
)
```

이때 KClass 타입은 자바에서 java.lang.Class와 상응한다. 여기서 out 키워드는 Any를 확장한 클래스를 모두 인자로 허용할 수 있게 만들어준다.

## #리플렉션

리플렉션이란 런타임에 동적으로 프로그램의 클래스를 조사하기 위해서 사용되는 방법이다. 가끔씩 아무 타입의 객체로든 적용 가능한 코드나, 런타임에만 매소드/프로퍼티의 이름을 파악 가능한 경우가 생긴다. 이때 리플렉션을 통해 프로그램이 실행중일 때 인스턴스 등을 통해 객체의 내부 구조 등을 파악하게 만들 수 있다.

즉 리플렉션을 사용하면 실행 시점에 (동적으로) 객체의 프로퍼티와 메소드에 접근할 수 있다.

## #**더블 콜론(::)**

더블콜론(**`::`**)은 코틀린에서 메서드 참조, 프로퍼티 참조와 같은 메타 프로그래밍에 관련된 것으로, **`리플렉션`**을 위해 사용한다.

코틀린에서 변수나 클래스명 앞에 더블콜론(**`::`**)을 명시하면, **변수나 클래스에 대한 참조를** 할 수 있다. 더블콜론을 명시하면 변수가 아닌 객체(Object)로 접근할 수 있기 때문이다.

자바와 코틀린에서의 리플렉션 차이는 다음과 같다.

```kotlin
/* 자바 */
SomeClass.class   //클래스 그 자체를 리플렉션
someInstance.getClass()   //인스턴스에서 클래스를 리플렉션

/* 코틀린 */
SomeClass::class
someInstance::class
```

JSON 직렬화 라이브러리를 사용하면 실행 시점에만 변수가 클래스에 대한 정보를 알 수 있다. 따라서 실행 시점에 동적으로 참조하도록 리플렉션을 사용한다.

## #메타데이터

메타데이터란 애플리케이션이 처리해야 할 데이터가 아니라, 컴파일 과정과 실행 과정에서 **코드를 어떻게 컴파일**하고 **처리**할 것인지 알려주는 정보다. 즉, 어노테이션은 메타데이터다.

데이터에 관한 구조화된 데이터를 메타데이터라고 한다. 도서관의 책 내용이 데이터라고 한다면, 해당 책이 서재에 어떻게 꽂혀 있어야 하는지, 이름표는 무엇인지에 대한 데이터를 메타데이터라고 한다.