### 1. 동등성 연산자
Java와 Kotlin에서의 `==`, `===`, `equals` 비교 

| Expression | Java | Kotlin |
| --- | --- | --- |
| == | 원시 타입은 값을 비교/참조 타입은 주소값 | 참조 타입은 equals로 자동 컴파일 |
| equals | 참조 타입을 값을 비교 | ==이 사용되기 때문에 거의 사용되지 않음 |
| === | 존재하지 않음 | 주소값 비교 |
<br>

### 2. 구조 분해 선언
일반 클래스의 경우
```kotlin
class Point(val x: Int, val y: Int) {
    operator fun component1() = x
    operator fun component2() = y
}
```
위와 같이 operator를 이용해 컴포넌트를 직접 분해하여 선언하고
<br>

데이터 클래스의 경우
```kotlin
val(a, b) = data 
-> val a = data.component1()
   val b = data.component2()
```
위와 같이 구조 분해 선언을 써서 더 간결하게, 동등성을 보여주며 쓸 수 있다.

<br>

### 3. 복합 대입 연산자 +=
이론적으로는 코드에 있는 + 를 plus 와 plusAssign 양쪽으로 컴파일할 수 있다. 즉, `+=` 를 사용하면 `plus`와 `plusAssign` 함수가 모두 호출될 수 있다는 말이다.

> 이를 방지하기 위해 앞서 Kotlin in action에서 본 것 처럼 `var` 를 `val` 로 바꿔서 `plusAssign` 적용이 불가능하게 할 수 있다.

코틀린 표준 라이브러리는 아래와 같이 _변경 가능한 컬렉션_에 대해 `plusAssign` 을 정의하기 때문이다.

```kotlin
operator fun <T> MutableCollection<T>.plusAssign(element: T) {
		this.add(element)
}
```

따라서 애초에 변경 불가능한 `var` 로 정의하면 `plusAssign` 은 적용되지 않도록 막아둘 수 있게 되는 것이다.
