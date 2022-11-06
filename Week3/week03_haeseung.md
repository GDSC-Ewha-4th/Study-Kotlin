# Kotlin in Action Chap.3 QnA🔍

## #Extension function

확장 함수는 어떤 클래스의 맴버 메소드인 것처럼 호출할 수 있지만 그 클래스의 밖에 선언된 함수를 뜻한다. 확장 함수를 선언한 다음에는 해당 클래스에 속한 것처럼 사용할 수 있다. 즉, ( . )을 통해 접근 가능하다. 이는 j마치 java에서 static으로 만들어진 메소드를 사용하는 것과 비슷하다. 단, 확장 함수의 개념이 `캡슐화`를 깨는 것은 아니다.

```kotlin
fun String.lastChar(): Char = this.get(this.length-1)
println("Kotlin".lastChar())
```

## #Extension property

확장 함수는 프로퍼티처럼 사용이 가능하다(프로퍼티란, 클래스에 val/var로 정의되는 변수). 이는 곧 기존 클래스 객체에 대한 프로퍼티 형식의 구문으로 사용할 수 있는 API를 추가하는 것이다.

프로퍼티 문법을 사용할 경우 코드를 더 짧게 작성할 수 있어 편리한 상황이 있기 때문에 이와 같은 방식을 사용한다.

```kotlin
val String.lastChar: Char
		get() = get(length -1)
```

- 뒷받침하는 필드가 없어 기본 getter를 제공할 수 없으므로 최소한 getter는 꼭 정의를 해야 한다.
- 초기화 코드에서 계산한 값을 담을 장소가 없으므로 초기화 코드를 쓸 수 없다.

## #infix

```kotlin
val map = mapOf(1 to "one", 7 to "seven", 53 to "fifty-three")

//중위 함수로 선언
infix fun Any.to(other: Any) = Pair(this, other)
```

위의 코드에서 `to`는 construct가 아니라 infix call, 즉 `중위 호출`이라는 것을 알 수 있다. 중위 호출은 extenstion 함수나 일반적인 메소드 모두에 활용 가능한데, 이를 위해서는 `infix` 키워드를 함수 앞에 붙이면 된다. to 함수는 Pair를 리턴한다. 예를 들어, `1 to “one”`이라고 사용하면 결과로 (1, “one”)이 반환된다. 이러한 특성은 **구조 분해 선언**이라고 부른다(**객체가 가지고 있는 여러 값을 분해해서 여러 변수에 한꺼번에 초기화하는 것**). 구조 분해 선언은 pair 뿐만 아니라 map, loop 등등 여러 요소에 사용 가능하다.

중위 함수를 사용하기 위한 조건은 다음과 같다.

1. 클래스의 멤버 함수 혹은 확장 함수여야 한다(즉, 반드시 확장 함수일 필요가 없다).
2. 매개변수는 하나만 가져야 한다.
3. infix 키워드를 사용하여 정의한다.

## #API

확장 함수나 확장 프로퍼티를 만들면, 기존의 자바 collection interface를 불러오는 두 애플리케이션 간의 서비스 계약을 새롭게 만드는 것이다. 새로운 객체를 프로퍼티 형식으로 만든다면 그 객체가 새로운 API가 되는 것이다. 실제의 Collection의 class를 수정한 것이 아니니까, 이를 Interface라고 즉 API라고 할 수 있다.

## #Receiver Object

`수신 (객체) 타입`은 클래스의 이름을 말한다. 확장 함수를 사용하는 객체, 즉 함수를 실행할 객체를 `수신 객체`라고 부른다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2Fbru16N%2Fbtq0dnbOvfu%2FGK3ZQy1rE6MQL8BjO2jr90%2Fimg.png)