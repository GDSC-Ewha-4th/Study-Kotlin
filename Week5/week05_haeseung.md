# Kotlin in Action Chap.5 QnA🔍

## #현재 영역에 있는 변수 접근

코틀린의 람다 안에서는 파이널 변수가 아닌 변수에 접근이 가능하다. 반대로 자바에서는 파이널 변수만 접근할 수 있다. 즉, 자바는 변수의 복사본인 값에만 접근 가능하고, 코틀린에서는 값 자체에 접근이 가능하다. 이렇게 코틀린에서 람다 안에서 사용하는 외부 변수를 `람다가 포획(capture)한 변수`라고 부른다.

기본적으로 함수 안에 정의된 로컬 변수의 life cycle은 함수의 life cycle과 같다. 즉, 함수가 종료되면서 끝난다. 그러나 어떤 함수가 자신의 로컬 변수를 포획한 람다를 반환하거나, 다른 변수에 저장한다면 함수가 종료되어도 로컬 변수가 사라지지 않아 람다의 본문 코드가 포획한 변수에 접근/수정할 수 있다.

- `final`변수를 포획한 경우, 변수 값을 복사하여 람다 코드와 함께 저장한다.
- `final이 아닌 변수`를 포획한 경우, 변수를 특별한 래퍼(=Ref 인스턴스, 기본형을 객체로 변환하여 작업을 수행)와 감싸서 나중에 변경하거나 읽을 수 있도록 한 다음, 래퍼에 대한 참조를 람다 코드와 함께 저장한다.

코틀린에서의 다음과 같은 코드는,

```kotlin
var counter = 0
val inc = { counter++ }
```

자바에서 다음과 같이 바뀐다.

```java
final IntRef counter = new IntRef();
counter.element = 0;
Function0 inc = (Function0)(new Function0() {
    public Object invoke() {
        return this.invoke();
    }
 
    public final int invoke() {
        int var1 = counter.element++;
        return var1;
    }
});
```

<aside>
💡 객체 변수에 final로 선언하면 그 변수에 다른 참조 값을 지정할 수 없다.

원시 타입과 동일하게 한번 쓰여진 변수는 재변경 불가하다. 단, 객체 자체가 immutable(불변)하다는 의미는 아니다. 객체의 속성은 변경 가능하다.

```kotlin
public class FinalFieldClass {
  final Pet cat = new Pet();
    // cat = new Pet(); // 다른 객체로 변경할 수 없다
    cat.setWeight(15); //객체 필드는 변경할 수 있다
}
```

</aside>

## #핸들러

비동기 호출은 가장 많이 사용하는 핸들러를 빠르게 호출한다(우선순위를 가짐). 따라서 순서대로 호출되지 않기 때문에 아래 코드에서 clicks는 항상 0이 된다. 왜냐하면 `return`이 `onClick`보다 먼저 실행되기 때문이다.

```kotlin
fun tryToCountButtonClicks(button: Button): Int {
	var clicks = 0
	button.onClick { clicks++ }
	return clicks
}
```

## #멤버 참조

```kotlin
val getAge = Person::age
val getAge = {person:Person -> person.age} //위의 식의 의미
```

코틀린에서는 `::`를 클래스 이름과 참조하려는 멤버(프로퍼티나 메서드) 이름 사이에 위치하면, 함수를 값으로 바꾸어 직접 넘길 수 있다. 최상위 함수의 경우에는 클래스 이름을 생략하고 `::`로 바로 참조를 시작할 수 있다. 즉, 파라미터로 넘기려는 코드가 이미 함수로 선언되어 있을 때 사용한다.

## #바운드 멤버 참조

```kotlin
/*코틀린 1.0*/
>>> val p = Person("Dmitry", 34)
>>> val personsAgeFunction = Person::age //객체에 두 번 접근
>>> println(personsAgeFunction(p))
34

/*코틀린 1.1*/
>>> val dmitrysAgeFunction = p::age //객체를 한 번 접근
>>> println(dmitrysAgeFunction())
34
```

바운드 멤버 참조를 사용하여, 멤버 참조를 생성할 때 클래스 인스턴스를 함께 저장하여 나중에 그 인스턴스에 대해 멤버를 호출해준다. 따라서 객체의 접근 횟수를 줄여준다.

## #무명 클래스

코틀린에서는 람다 식을 통해 함수를 선언하는 대신 코드 블록을 직접 함수의 인자로 전달할 수 있다. 자바는 무명 내부 클래스를 선언하기 때문에 코드가 번잡스러워진다. 이는 무명 클래스가 한 번만 사용하고 버려지기 때문이다. 또한 확장을 쓰는 것이 유지 보수가 어려운 경우 무명 클래스를 사용한다.

```kotlin
/* Java */
button.setOnClickListener(new OnClickListener() {
@Override
public void onClick(View view) { //무명 내부 클래스의 선언
	/*클릭 시 수행할 동작 */
	}
});

/* Kotlin */
button.setOnClickListener{ /*클릭 시 수행할 동작 */}
```

## #SAM 생성자

SAM 생성자는 람다를 함수형 인터페이스로 명시적으로 변경하는 역할을 한다. 이는 컴파일러가 자동으로 생성한 함수로, 함수형 인터페이스의 인스턴스를 반환하는 메서드가 있다면 람다를 직접 반환할 수 없고, 반환하고픈 람다를 SAM 생성자로 감싸야 한다.

SAM 인터페이스는 단일 추상 메소드가 있는 인터페이스이다. 자바에서는 함수형이라는 타입이 없는데, 코틀린에서는 있기 때문에 둘 사이의 호환성을 맞출 수 없다. 따라서 자바에 람다를 전달해주고 싶을 경우, 컴파일러는 SAM 인터페이스로 자동 변환하여 호환성을 맞춘다.

## #지연 계산(lazy) 컬렉션 연산

앞에서 살펴본 컬렉션 함수는 결과 컬렉션을 **즉시** 생성하는데(=매 단계의 계산 중간 결과를 새로운 컬렉션에 담음) `시퀀스`를 사용하면 이러한 중간 컬렉션을 사용하지 않아도 컬렉션 연산을 인쇄할 수 있다. 따라서 지연 계산을 사용하면 호출될 때만 연산되어 효율이 좋다. `asSequence` 확장 함수를 호출하면 어떤 컬렉션이든 시퀀스로 바꿀 수 있다. (자바에서는 이 기능을 스트림으로 사용한다: [글 참고](https://velog.io/@cham/JAVA-%EC%8A%A4%ED%8A%B8%EB%A6%BCStream))

```kotlin
people.map(Person::name).filter { it.startsWith("A") } //원소가 많을 수록 리스트가 더 생기는 게 문제 생김

people.asSequence() //원본 컬렉션을 시퀀스로 변환하고, 결과 시퀀스를 다시 리스트로 변환
	.map(Person::name)
	.filter { it.startsWith("A") }
	.toList()
```

## #시퀀스 연산 실행: 중간 연산과 최종 연산

시퀀스에 대한 연산은 **중간** 연산과 **최종** 연산으로 나뉜다. 중간 연산은 다른 시퀀스를 반환하고, 최종 연산은 결과를 반환한다.

이 점을 고려하면 컬렉션의 종류나 연산 순서에 따라서 연산의 성능을 계산할 수 있다. 예를 들어, filter 후 map을 만들면 필요 없는 원소를 먼저 삭제하여 효율이 좋다.