#### **What is**

#### **Extension** 

-   상속이나 디자인 패턴 없이 클래스를 간단하게 확장할 수 있는 방법

(자바에는 확장 기능이 없어서 AOP 같은 관점 지향 프레임워크를 동원해서 사용함.)

\+ 자바와 달리 코틀린은 클래스가 기본적으로 기본 상속 변경자가 final 이라서 open 키워드를 달아주지 않으면 상속 불가능하다. 그래서 확장(Extension)을 지원한다.

-   정적 바인딩된다는 특징

정적 바인딩이란: 컴파일 시간에 구성요소의 성격이 결정되는 것.

#### **코틀린의 확장 함수(Extension function)**

클래스로 대변되는 타입에 함수를 상속관계 없이 추가하는 것.

함수를 마치 이 타입의 메소드(멤버 함수)인 것처럼 호출 가능.

호출 가능한 함수들은 패키지의 멤버 함수이다.

새로운 클래스를 만들지 않아도 되어서 간편하게 사용 가능하다.

```
fun MutableList<Int>.swap(index1: Int, index2: Int) {
    val tmp = this[index1] // 'this' corresponds to the list
    this[index1] = this[index2]
    this[index2] = tmp
}
```

위 예시는 MutableList<Int> 에 swap 함수를 추가한다.

MutableList<>는 수신자 타입(receiver type)이다: 확장하려는 대상이 되는 타입

swap 은 확장 함수

> 확장 함수를 만드는 방법:  
> 클래스 이름 .(온점) 함수 이름

확장함수는 클래스 안에 들어가지 않는다.

실제로 메서드를 클래스 안에 만드는 것이 아니라 클래스 밖에서 선언되는 것이다. 마치 이 클래스의 멤버 함수인 것처럼.

\* 단 확장 함수는 수신되는 클래스가 private이나 protected 일 때 사용하지 못함. public 멤버만 사용 가능하다.

(확장 함수는 클래스 밖에서 접근하는 형태이기 때문에)

#### **코틀린의 확장 프로퍼티(Extension Property)**

함수 확장 뿐만 아니라 프로퍼티 확장도 할 수 있다.

프로퍼티, 즉 멤버 변수도 확장 가능.

```
val <T> List<T>.lastIndex: Int
    get() = size - 1
```

> 확장할 때 클래스 안에 멤버를 넣는 것이 아니라서 확장 프로퍼티를 초기화할 수 없다.  
> getter/setter 를 명시적으로 제공함으로써 초기화와 같은 기능을 사용할 수 있다.

아래와 같은 코드는 에러가 발생한다.

```
val House.number = 1 // error: initializers are not allowed for extension properties
```

#### **확장 함수 import**

확장 함수를 정의해도 프로젝트의 모든 소스코드에서 함수를 사용할 수는 없어서 import 를 해야 한다.

코틀린에서는 개별 함수를 임포트 할 수 있다.

만약 한 클래스 안에 같은 이름의 함수가 둘 이상 있다면 서로 충돌하여 오류가 생긴다.

이 경우, as 키워드를 사용하여 다른 이름으로 바꿔주면 된다.

```
import ~.common.convertToMileage
import ~.utils.convertToMileage as toMileage

val mileage = 52000
mileage.convertToMileage() //common 패키지의 확장함수
mileage.toMileage() //utils 패키지의 확장함수
//https://0391kjy.tistory.com/18 참고
```

#### **확장 함수의 오버라이드과 오버로드**

확장 함수는 오버라이딩 불가능하다. 확장 함수는 클래스 밖에서 선언되기 때문이다.

오버라이딩이란? 상위 클래스의 메서드와 이름과 용례가 같은 함수를 하위 클래스에 재정의하는 것

확장 함수는 오버로드 가능하다. 클래스의 메소드와 확장 함수의 이름이 같더라도 매개 변수 타입이 다르면 가능하다.

오버로드란? 같은 클래스나 상속 관계에서 동일한 이름의 메소드 중복 작성.

```
class Me {
    fun getHeight(height: Int) {
        println("제 키는 ${height}cm 입니다.")
    }
}
 
fun Me.getHeigth(height: Float) {
    println("제 키는 ${height}cm 입니다.")
}
 
fun main() {
    val me = Me()
    me.getHeight(180) // 제 키는 180cm 입니다.
    me.getHeigth(180.3F) // 제 키는 180.3cm 입니다.
}
```

\*humancoding 강의, kotlinlang.org 참고
