# Kotlin in Action 2장 - 코틀린 기초


## 1. Property란?


프로퍼티를 위키백과에서는 이렇게 정의하고 있다.
>프로퍼티(property)는 일부 객체 지향 프로그래밍 언어에서 필드(데이터 멤버)와 메소드 간 기능의 중간인 클래스 멤버의 특수한 유형이다. 프로퍼티의 읽기와 쓰기는 일반적으로 게터(getter)와 세터(setter) 메소드 호출로 변환된다.

Kotlin in Action 2장에서도 프로퍼티에 대해 다루었지만([바로가기](https://velog.io/@akimcse/Kotlin-in-Action-2%EC%9E%A5-%EC%BD%94%ED%8B%80%EB%A6%B0-%EA%B8%B0%EC%B4%88#1-%ED%94%84%EB%A1%9C%ED%8D%BC%ED%8B%B0)), 위 정의문과 유사하게 너무 이론적인 내용이라 어떤 개념인지 크게 와닿지 않는다는 의견이 다수였다.

프로퍼티를 어떻게하면 좀 더 **와닿게** 설명할 수 있을까?

</br>

쉽게 접근해보자. property는 우리말로 `재산`, `소유물`, `속성`이라는 의미를 갖는다.
그렇다면 Kotlin 클래스의 property는 해당 클래스가 가지고 있는 것들 혹은 속성정도로 생각해볼 수 있겠다.

그렇다면 클래스가 가질 수 있는 것에는 무엇이 있는가?
Java로 따지면 `필드(field)`와 `접근자`가 있겠다. 
Java에서는 데이터를  필드에 저장한다. 그리고 각각의 필드들에는 접근자인 `getter`와 `setter`를 통해 접근하게 된다.

그렇기에 Java의 클래스는 Kotlin in Action 2장에서 다루었듯이 이렇게 길어지게 된다.

```java
// Java 클래스 Person
public class Person {
	private final String name; // name 필드

	public Person(String name) {
			this.name = name;
	}

	public String getName() { //name getter
			return name;
	}
}
```
`name`이라는 필드의 경우 final로 선언되어 변경이 불가능하고, 그렇기에 읽기 위한 getter만이 존재하는 것을 볼 수 있다.

그런데 이 클래스에 `name` 이외의 다른 필드가 추가된다면 어떻게 될까?

```java
// Java 클래스 Person2
public class Person2 {
    private final String name; //name 필드
    private boolean isMarried; //isMarried 필드

    public Person(String name, boolean isMarried) {
        this.name = name;
        this.isMarried = isMarried;
    }

    public String getName() { //name getter
        return this.name;
    }

    public void setIsMarried(boolean isMarried) { //isMarried setter
        this.isMarried = isMarried;
    }

    public boolean getIsMarried() { //isMarried getter
        return this.isMarried;
    }
}
```
이번에는 변경 가능한 `isMarried`라는 필드가 추가되어 getter와 setter 두 개를 더 달고 왔다.

이렇게 계속 필드를 추가하다보면 이 클래스에는 getter와 setter 같은 보일러 플레이트 코드가 점점 늘어날 것이다. ~~생각만해도 끔찍하다.~~

</br>

하지만 위의 21줄이나 되는 코드를 Kotlin으로 쓰면 아래와 같다.

```kotlin
class Person(val name: String, var isMarried: Boolean)
```
혁신적이다. 이러한 혁신이 가능한 이유가 바로 `프로퍼티` 라는 개념 때문이다.

앞서 필드, 접근자 등에 대해 다뤄본 것을 생각하며 다시 위키백과의 정의문을 읽어보자.
>프로퍼티(property)는 일부 객체 지향 프로그래밍 언어에서 필드(데이터 멤버)와 메소드 간 기능의 중간인 클래스 멤버의 특수한 유형이다. 프로퍼티의 읽기와 쓰기는 일반적으로 게터(getter)와 세터(setter) 메소드 호출로 변환된다.

요약해보면 프로퍼티는 필드도 메소드도 아니지만 **클래스 멤버**이고, getter와 setter **메소드를 호출하여 접근**한다는 내용이다.

>즉, Kotlin이 제공하는 `property` 라는 개념은 필드와 접근자를 따로 써줄 필요 없는 클래스의 한 멤버다.

그리고 이 property는 변경 가능한 경우 `var` (getter, setter 포함), `var` (getter만 포함) 의 두 타입으로 나눠진다.

</br>


## 2. enum이란?
`enum` 은 말 그대로 열거형이다. 또한, 2장에서 Kotlin 에서는 `enum class` 형태로 쓰인다고 했었다.
그런데 무언가를 열거하는데 흔히 쓰이는 리스트도 아니고, 배열도 아니고, 별 볼일 없어보이는 *열거형*이 굳이 필요한 이유가 뭘까?

안드로이드 개발을 하는 상황이라고 가정해보자.
서버에서 0, 1, 2, 3 등으로 구분된 특정 값을 받으면 해당 값에 따라 다른 캐릭터 이미지를 보여줘야 한다. 이런 경우 어떻게 코드를 작성하면 좋을까?

```kotlin
private const val ALICE = 0
private const val BOB = 1
private const val CATHERINE = 2
private const val DANA = 3
```
우선 이런 식으로 각 캐릭터에 대한 상수값을 지정해줄 수 있다.
그리고 아래쪽 어딘가에 내려가서 이 상수값을 이용해 이미지를 붙여주는 함수를 작성하게 될 것 이다.

물론 이것도 어떻게든 구현은 되긴 되겠다만, 만약 이렇게 선언할 상수들이 늘어난다면 뭐가 어디에 쓰이는 상수인지 구별하기 힘들어질 것이다.
나중에 다시 이 코드를 열어보았을 때 맨 위에 가득한 상수들을 보고 ~~도대체 이게 무슨..~~ 혼란에 빠질 수도 있다.

</br>

그렇다면 아래와 같이 Character라는 같은 속성을 가진 상수의 집합을 하나로 묶어서 써주면 어떨까.

```kotlin
enum class Character(
	ALICE, BOB, CATHERINE, DANA; //ALICE(0), BOB(1), CATHERINE(2), DANA(3) 과 같다.
    
    fun getCharacterImage(character: Character) = 
        when(character) {
        	Character.ALICE -> R.drawable.img_alice
            Character.BOB -> R.drawable.img_bob
            Character.CATHERINE -> R.drawable.img_catherine
            Character.DANA -> R.drawable.img_dana
        }
    }
}
```
위 `enum class`는 `ALICE`, `BOB`, `CATHERINE`, `DANA`라는 각각의 캐릭터 이름을 묶어놓았을 뿐만 아니라, 아래 `getCharacterImage` 메서드를 추가해 각 캐릭터에 따라 적절한 이미지를 가져오는 과정까지 하나의 클래스에 담았다.

1. 쓰임이 같은 상수들이 묶여있고
2. 상수와 관련된 함수가 함께 묶여있으니

코드 가독성이 매우 좋아졌다!

이것이 바로 `enum class` 의 존재 이유다. 이름은 소박하게도 열거형이라는 뜻이지만, 배열, 리스트와 같은 단순한 나열과는 차원이 다른 개념인 것이다.

</br>

## 3. Java와 Kotlin의 예외처리는 어떻게 다른가?

둘의 차이점을 다뤄보기에 앞서, **예외**에 대해 알아보도록 하자.
예외(Exception)란 실행 중 오동작이나 결과에 악영향을 미치는 예상치 못한 상황이 발생한 경우를 말한다. Java에서는 실행 중 발생하는 에러를 예외로 처리한다.

![image](https://blog.kakaocdn.net/dn/cbmycN/btq4NGXDgYX/ZW2HMUiz3kkPy9kfIt5PCK/img.png)

Java에서 예외를 상속받는 클래스는 크게 두 종류로 나뉜다. (위 그림 참고)
- checked Exception : `RuntimeExeception` 클래스를 상속하지 않은 예외 클래스들
이름에서 알 수 있듯이 꼭 체크해주고 가야하는, 즉 예외 처리 과정이 꼭 필요한 예외들이다.
`catch` 문으로 잡거나, `throws` 를 통해 함수 밖으로 던져야 한다.

- unchecked Exception : `RuntimeException` 클래스를 상속한 Exception 클래스들
반면 unchecked Exception의 경우 꼭 처리해주지 않아도 된다. 
`RuntimeException`은 보통 프로그램에 문제가 있는 경우로, 예상치 못한 곳에서 발생하는 것은 아니다. 따라서 굳이 예외 처리가 강제는 아닌 것이다.


그러나 Kotlin은 이 두 예외를 구분하지 않는다.

</br>

*왜 그럴까?*

Java의 경우 앞서 말한 것 처럼 체크 예외의 처리를 강제하고 있으나 실제 프로그래머들의 처리는

1. 의미없이 다시 예외를 던져버림
2. 예외를 잡긴 하는데 처리는 하지 않고 놔둠

위 두 경우가 가장 흔하다. 
그렇기에 예외 처리 규칙을 만들어두었으나 실제 오류는 방지하지 못하고 허상뿐인 상황이 생기게 된다.

맨 처음에 Kotlin에 대해 알아볼 때 Jetbrains 개발자들이 Java가 ~~너무 구려서~~ 불편한 점들을 개선하고자 직접 개발한 언어라고 했는데, 바로 이 부분도 놓치지 않은 것이다.

>허상뿐인 규칙을 없애기 위해 Kotlin에서는 두 예외를 구분하지 않으며 **throws 절도 쓰지 않아도 된다.**

이것이 바로 Java와 Kotlin의 예외처리에 있어 가장 큰 차이점이다.
이를 제외하면 try-catch-(finally)를 이용하는 등 예외처리의 전반적인 내용은 Java와 유사하다.
