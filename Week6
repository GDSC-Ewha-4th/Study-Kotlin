# Week6 Nullable Deep dive _Sohyun

## 널이 될 수 있는 타입의 값으로 뭘 할 수 있을까?

- 연산이 제한적이긴하지만 결과가 널인지 아닌 지 확인할 수 있다.
  
- **언니의 팁!** : 자바 기준으로 래퍼 객체가 null을 받을 수 있는데, DB에서 select문으로 뭔가를 찾는데 값이 없으면 오류가 안나니까. 오류인지 널 값인지 알 수 있다. 널만 받으면 오류가 안남. 그런데 만약에 널을 받아서 무언가 활용하려고 하면 에러가 남. NullPointerError가 남! Nullable이 아니면 판단할 수 없는 에러가 나서 뭐가 문제인지 모르는데, Nullable을 받으면 Null인 것을 확인 할 수 있다.
  
  - Nullable을 쓰는게 오류가 적게 발생하고 편하다.

## !!활용: 널 아님 단언문의 사용

- 기본적으로 널 아님 단언은 자바 프로그램의 널 관련 동작, 즉 널 값을 역참조하려 할 때 예외를 던지는 동작을 부활시킨다.
- ```kotlin
  val n = readLine()!!.toInt()
  ```
  
- Error가 발생시키지 않을 때만 사용한다.(신중하게 사용해야 한다.)
- 어떤 함수에서 이미 이 값이 null이 아닌 것을 체크했는데 그 값을 다른 함수에서 참조한다고 하면, 그 함수는 앞에서 검사했으니까 널 아닌 것을 확실히 검사를 했음을 보증한다.

let 함수

> <mark>**스코프 함수**</mark>
> 
> 람다식으로 호출할 때, 이는 임시로 범위(scope)를 형성합니다. 이 범위 내에서는 객체의 이름이 없어도 객체에 접근할 수 있습니다. 객체의 컨텍스트 내에서 코드 블럭을 실행하기 위한 목적만을 가진 여러가지 함수

여러 가지 스코프 함수의 정리이다.

| Function | Object reference | Return value | Is extension function |
| --- | --- | --- | --- |
| let | it  | Lambda result | Yes |
| run | this | Lambda result | Yes |
| run | -   | Lambda result | No: called without the context object |
| with | this | Lambda result | No: takes the context object as an argument |
| apply | this | Context object | Yes |
| also | it  | Context object | Yes |

스코프 함수를 람다 함수로 사용하면 임시로 스코프(범위)를 형성한다. 이 범위는 그 괄호 안이다.

스코프 내에서는 객체의 이름을 통해 일일이 참조할 필요가 없다.

> **T**를 **타입 변수(type variable)** 라고 하며, **임의의 참조형 타입**을 의미합니다.

let은 수신 객체를 이용해 작업을 한 후 마지막 줄을 return 할 때 사용한다.

- null check 후 코드를 실행해야 하는 경우
  
- nullable한 수신객체를 다른 타입의 변수로 변환해야 하는경우
  

let은 위와 같은 경우에 사용한다.

- 참조 링크

[[Kotlin] apply, run, with, let, also 차이 한 번에 정리하기 — Kotlin World](https://kotlinworld.com/255)

<mark>널이 될 수 있는 타입 확장</mark>

- 어떤 메서드를 호출하기 전에 수신 객체 역할을 하는(인수로 넘겨지는) 변수가 널이 될 수 없다고 보장하는 대신 직접 변수에 대해 메서드를 호출해도 확장 함수인 메서드가 알아서 널을 처리해준다.
  
- 일단 받을 때부터 널일 수 있다고 받고, 함수 내에서 처리해주면 된다.
  

-> 이런 처리가 확장 함수 내에서만 가능한가? 객체 인스턴스를 통해 디스패치 되기 때문에

> 디스패치: 객체지향 언어에서 객체의 동적 타입에 따라 적절한 메서드를 호출해주는 방식

- 자바의 경우 메서드 안의 this는 그 메서드가 호출된 수신 객체를 가리키므로 항상 널이 아니다. 그러나 코틀린에서는 널이 될 수 있는 타입의 확장 함수 안에서 this가 널이 될 수 있다. 자바는 확장함수 정의가 불가능하다.

읽기 전용 컬렉션이 항상 스레드 안전하지는 않은 이유

> 스레드 안전(Thread-Satety)란 **멀티 스레드 프로그래밍에서 일반적으로 어떤 함수나 변수, 혹은 객체가 여러 스레드로부터 동시에 접근이 이루어져도 프로그램의 실행에 문제가 없는 것을 말한다**.

- 스레드 : 프로세스의 실행 단위
  
- 어떤 데이터가 스레드 안전이라는 건 공유 데이터가 1) 변형이 없거나 2) 변형한 값을 이상한 값으로 다음 스레드에 넘기지 않는다는 것인데, 그래서 읽기 전용 컬렉션이면 안전할 것이라고 생각한다.
  
- 하지만 읽기 전용 컬렉션이어도 그것을 통해서 데이터에 접근할 때는 변경 불가능 하지만, 그 데이터 자체는 변경 가능한 다른 컬렉션으로 접근할 때는 변경 가능하다.
  

시그니처란?

> 자바 프로그래밍 언어에서 메서드 시그니처는 메서드 명과 파라미터의 순서, 타입, 개수를 의미한다.

오버로딩이 다른 시그니쳐를 같는 여러 함수를 정의한다.

- 자세한 링크 : https://ildann.tistory.com/7

컬렉션과 배열: 배열 생성?

- 배열은 'arrayOf', 'arrayOfNulls', 'emptyArray'로 배열 객체가 생성된다.
  
- 이 외에도 Array 생성자를 사용할 수 있다.
  
  ```kotlin
  fun main(args: Array<String>) {
      initWithConstructor()
  }
  
  private fun initWithConstructor() {
      val array: Array<Int> = Array(3) { i -> i } [0,1,2]
      // 좌측 i : index
      // 우측 i : index에 들어갈 값
      // i는 0부터 시작.
  
      array.forEach { print("$it ") }
      println()
  }
  ```
  

다음 링크는 Array 생성의 설명을 링크를 첨부했다.

- [[Kotlin] Array 생성하고 변경하는 방법 한 번에 정리하기 — Kotlin World](https://kotlinworld.com/2)
  
- [[Kotlin]코틀린 배열 arrayOf(), Array](https://warmdeveloper.tistory.com/15)
