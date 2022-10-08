### Week02_Sohyun



### Kotlin in action: 코틀린 모르는 것 도장깨기 1편



Java를 분명히 잘 안다고 생각했는데~~  Kotlin을 처음부터 배우다 보니 처음 보는 개념들이 너무 많았다. ~~(스터디원들 중에 내가 가장 질문이 많았던 것 같기도..)~~

그래서 서로 주고받았던 질문들에서 새로 배워가는 개념들을 정리하며 다른 개발 블로그에서 나에겐 2% 부족했던 내용들을 각각 채워서 나만의 코틀린 FAQ 사전을 만들어보겠다!

### 1. Property에 대해서

Java에서도 나오는 개념인 Property를 왜인지 나는 처음 들어보는 것 같은 느낌이 들었다. Kotlin에서 Property를 기본으로 제공하는데, 그렇다면 정확히 무엇이 제공되는 것인지, Java에서 Property는 어떤 형태를 띄는지에 대해 알고 넘어가고 싶었다.

#### 그렇다면, Property는 무엇이 Java에서 Property는 어떤 형태를 띌까?

Property를 이해하기 위해서는 자바빈(JavaBean)을 이해해야 한다. **JavaBean은 데이터를 표현하는 것을 목적으로 하는 자바 클래스이다.** 자바의 재활용 가능한 컴포넌트 모델이며, 정보의 덩어리, 데이터 저장소이다. 

~~Java Bean에는 여러가지 사용 규약이 있는데 이것까지 정확하게 짚고 넘어가면 내용이 방대해질 것 같다..~~ 

사용규약은 아래의 링크에서 확인 가능하다.

https://imasoftwareengineer.tistory.com/101

 [JavaBean이란?

JavaBean이라는 말을 많이 들어 보았을 것이다. 예를들어 스프링을 사용하는 경우 @Bean이라는 어노테이션을 사용해 봤다던가, XML에 아래와 같은 선언을 해본적이 있을 것이다. 그렇다면 JavaBean이란

imasoftwareengineer.tistory.com](https://imasoftwareengineer.tistory.com/101)

정리해보자면 자바빈은 **데이터를 저장하기 위한 필드와 데이터를 컨트롤하는 getter/setter 메소드(보통 접근자라고 부른다)를 하나의 쌍으로 가지고 있는 클래스**이다.

Read Only의 자바빈의 경우 Setter는 없을 수 있다. 

**자바빈에 있는 멤버 변수를 property(프로퍼티)라고 부른다.** 보통 getter와 setter을 같이 묶어서 프로퍼티라고 부른다.

자바빈의 프로퍼티에 접근하려면 **getter와 setter로만 접근**할 수 있다. 프로퍼티가 Boolean 값일 경우 get 대신 is를 사용할 수 있다.

자바빈(코드에서 Person class) 과 프로퍼티(name과 isMarried, 보통은 get set메서드도 함께 프로퍼티로 본다)의 코드 예시는 다음과 같다

```
public class Person {
    private final String name;
    private boolean isMarried;

    public Person(String name, boolean isMarried) {
        this.name = name;
        this.isMarried = isMarried;
    }

    public String getName() {
        return this.name;
    }

    public void setIsMarried(boolean isMarried) {
        this.isMarried = isMarried;
    }

    public boolean getIsMarried() {
        return this.isMarried;
    }
}
```

#### Kotlin에서 Property를 제공해준다는 의미가 무엇일까?

만일 자바빈 클래스 필드에 들어가는 데이터들이 점점 많아진다면, getter와 setter 같은 코드가 지저분하게 많아진다.

그래서 Kotlin은 이 getter와 setter 메서드를 쓰지 않아도 된다.(**프로퍼티를 제공해준다는 것은 이것을 의미한다.)**

위에서 본 긴 Java 코드를 Kotlin에서는 한줄로 나타낼 수 있다.

```
class Person(val name: String, var isMarried: Boolean)
```

위에서 name은 Read Only 였기 때문에 Setter가 제공되지 않았었는데, 그래서 val(불변변수)로 정의하였고, isMarried는 var(가변 변수)로 정의 되었다. 보통 get과 set 메서드는 숨겨져있으나, 코드를 명시적으로 적어줄 수 있다. 이 말은 getter와 setter를 커스텀 수도 있다는 뜻이다.

### 2. Kotlin에서 배열은 어떻게 선언될까?

Kotlin에는 배열을 생성하는 문법이 따로 존재하지 않는다. 그래서 배열을 생성할 때 Java와 다르게 기본적으로 제공되는 배열과 관련된 제네릭(데이터 타입을 일반화한다는 것을 의미)이나 클래스를 사용한다.

Java에서 배열을 정의하는 방법은 자료형[] 변수명 = {데이터....}이다.(다른 언어와 비슷)

그에 반해 **Kotlin은 제네릭을 사용하거나, 제공되는 클래스**를 이용할 수 있다.

- 제네릭 사용

```
var array = Array<Int>(4,{0}) //사이즈는 4이고 각자리에는 빈 값 0이 들어가 있는 배열
```

- 제공 클래스 사용

```
var array = CharArray(10){" "}
```

배열에 값을 넣거나 빼기 위해서는 getter와 setter를 사용하거나 다른 언어에서처럼 배열이름[]을 사용하면된다.

Getter, Setter 사용하기 : 배열명.set(인덱스, 넣을 값), 배열명.get(인덱스)

### 3. Enum에 대해서

작년에 Java를 공부할 때 enum에 대해서 배운 적이 있었던 것 같은데  너무 얼레벌레 배워서 기억이 잘 나지 않는다. 

enum은 **열거형(enumerated type)**이라 부른다. 열거형은 **서로 연관된 상수들의 집합**이다.

그래서 조건문 "when"을 enum 클래스를 다룰 때 사용하는데, 프론트나 서버에서 상수를 사용하지 않고 가독성이 좋게 enum을 사용하면서 이해도를 높일 수 있다는 장점이 있다.

```
//아마 위에 Color enum class가 정의되어 있겠지..?
fun getMnemonic(color: Color) =
when (color) {
    Color.RED -> "Richard"
    Color.ORANGE -> "Of"
    Color.YELLOW -> "York"
    Color.GREEN -> "Gave"
    Color.BLUE -> "Battle"
    Color.INDIGO -> "In"
    Color.VIOLET -> "Vain"
}
```

### 4. Kotlin의 예외처리와 Java의 예외처리의 차이점

Kotlin은 **체크 예외와 언체크 예외를 구분하지 않는다.** 언체크 예외의 경우 명시적인 예외 처리를 강제하지 않는데, 그래서 언체크 예외라고 불리며 보통 RuntimeException이라고 부른다.

![](https://blog.kakaocdn.net/dn/XBZaW/btrN4kwFCYO/KBVjEOWhggWDz0uqgGBLjk/img.png)

Java에서는 언체크 예외와 체크 예외를 구분하고 그에 대한  throws절이 함수 뒤에 코드에 있어야한다. 

그런데 Java에서 이렇게 구분하면서 생기는 자잘한 문제들이 있고, 그것 때문인지는 모르겠지만(?) **Kotlin에서는 언체크와 체크 예외를 따로 구분하지 않는다고 한다.**

그래서 명시적으로 **throws절이 필요없다**. (이게 코틀린의 예외처리에서 자바와 다른 점이다.)

이 throw 절이 필요없다는 얘기가 정확히 이해가 가지 않았는데 보통 예외를 던질 때는 throw를 사용하지만, 함수를 작성할 때 체크 예외가 발생할 수 있는 상황이여도 throws 절을 쓰지 않아도 된다는 것이다.

어제 스터디때 까지만 해도 정확히 이해가 가지 않았는데, **throw절과 throws절이 다르다**는 것을 몰랐기 때문이다!

throw는 **예외를 강제로 발생**시킨 후,(프로그래머의 판단에 따른 처리) 상위 블럭이나 catch문으로 예외를 던진다. 그러니까 메소드 내에서 상위 블럭으로 예외를 던지는 것이다.

throws는 예외가 발생하면 상위 메서드로 예외를 던진다. throws문을 통해 메소드를 사용하기 위해서 어떠한 예외들이 처리되어야하는지 쉽게 알 수 있다. 이렇게 **예외를 명시하는 것은 예외를 처리하는 것이 아니라 자신을 호출한 메소드에게 예외를 전달하여 예외처리를 떠맡**기는 것이다.(전가)

throw가 메소드 내에서 상위 블럭으로 예외를 던지는 것이라면, **throws는 현재 메소드에서 상위 메소드로 예외를 던지**는 것이다.

### 5. 스마트 캐스트(smart cast) 가 정확히 무엇인가요

스마트 캐스트는 프로그래머가 굳이 원하는 타입으로 캐스팅(타입 변환)하지 않아도, 컴파일러가 알아서 캐스팅해주는 것을 의미한다. 굳이 형변환 연산자를 써주지 않아도 알아서 형변환이 된다고 이해하면 된다.~~Java를 분명히 잘 안다고 생각했는데~~  Kotlin을 처음부터 배우다 보니 처음 보는 개념들이 너무 많았다. ~~(스터디원들 중에 내가 가장 질문이 많았던 것 같기도..)~~

그래서 서로 주고받았던 질문들에서 새로 배워가는 개념들을 정리하며 다른 개발 블로그에서 나에겐 2% 부족했던 내용들을 각각 채워서 나만의 코틀린 FAQ 사전을 만들어보겠다!

Java에서도 나오는 개념인 Property를 왜인지 나는 처음 들어보는 것 같은 느낌이 들었다. Kotlin에서 Property를 기본으로 제공하는데, 그렇다면 정확히 무엇이 제공되는 것인지, Java에서 Property는 어떤 형태를 띄는지에 대해 알고 넘어가고 싶었다.

#### 그렇다면, Property는 무엇이 Java에서 Property는 어떤 형태를 띌까?

Property를 이해하기 위해서는 자바빈(JavaBean)을 이해해야 한다. **JavaBean은 데이터를 표현하는 것을 목적으로 하는 자바 클래스이다.** 자바의 재활용 가능한 컴포넌트 모델이며, 정보의 덩어리, 데이터 저장소이다. 

~~Java Bean에는 여러가지 사용 규약이 있는데 이것까지 정확하게 짚고 넘어가면 내용이 방대해질 것 같다..~~ 

사용규약은 아래의 링크에서 확인 가능하다.

https://imasoftwareengineer.tistory.com/101

 [JavaBean이란?

JavaBean이라는 말을 많이 들어 보았을 것이다. 예를들어 스프링을 사용하는 경우 @Bean이라는 어노테이션을 사용해 봤다던가, XML에 아래와 같은 선언을 해본적이 있을 것이다. 그렇다면 JavaBean이란

imasoftwareengineer.tistory.com](https://imasoftwareengineer.tistory.com/101)

정리해보자면 자바빈은 **데이터를 저장하기 위한 필드와 데이터를 컨트롤하는 getter/setter 메소드(보통 접근자라고 부른다)를 하나의 쌍으로 가지고 있는 클래스**이다.

Read Only의 자바빈의 경우 Setter는 없을 수 있다. 

**자바빈에 있는 멤버 변수를 property(프로퍼티)라고 부른다.** 보통 getter와 setter을 같이 묶어서 프로퍼티라고 부른다.

자바빈의 프로퍼티에 접근하려면 **getter와 setter로만 접근**할 수 있다. 프로퍼티가 Boolean 값일 경우 get 대신 is를 사용할 수 있다.

자바빈(코드에서 Person class) 과 프로퍼티(name과 isMarried, 보통은 get set메서드도 함께 프로퍼티로 본다)의 코드 예시는 다음과 같다

```java
public class Person {
    private final String name;
    private boolean isMarried;

    public Person(String name, boolean isMarried) {
        this.name = name;
        this.isMarried = isMarried;
    }

    public String getName() {
        return this.name;
    }

    public void setIsMarried(boolean isMarried) {
        this.isMarried = isMarried;
    }

    public boolean getIsMarried() {
        return this.isMarried;
    }
}
```

#### Kotlin에서 Property를 제공해준다는 의미가 무엇일까?

만일 자바빈 클래스 필드에 들어가는 데이터들이 점점 많아진다면, getter와 setter 같은 코드가 지저분하게 많아진다.

그래서 Kotlin은 이 getter와 setter 메서드를 쓰지 않아도 된다.(**프로퍼티를 제공해준다는 것은 이것을 의미한다.)**

위에서 본 긴 Java 코드를 Kotlin에서는 한줄로 나타낼 수 있다.

```kotlin
class Person(val name: String, var isMarried: Boolean)
```

위에서 name은 Read Only 였기 때문에 Setter가 제공되지 않았었는데, 그래서 val(불변변수)로 정의하였고, isMarried는 var(가변 변수)로 정의 되었다. 보통 get과 set 메서드는 숨겨져있으나, 코드를 명시적으로 적어줄 수 있다. 이 말은 getter와 setter를 커스텀 수도 있다는 뜻이다.

### 2. Kotlin에서 배열은 어떻게 선언될까?

Kotlin에는 배열을 생성하는 문법이 따로 존재하지 않는다. 그래서 배열을 생성할 때 Java와 다르게 기본적으로 제공되는 배열과 관련된 제네릭(데이터 타입을 일반화한다는 것을 의미)이나 클래스를 사용한다.

Java에서 배열을 정의하는 방법은 자료형[] 변수명 = {데이터....}이다.(다른 언어와 비슷)

그에 반해 **Kotlin은 제네릭을 사용하거나, 제공되는 클래스**를 이용할 수 있다.

- 제네릭 사용

```kotlin
var array = Array<Int>(4,{0}) //사이즈는 4이고 각자리에는 빈 값 0이 들어가 있는 배열
```

- 제공 클래스 사용

```kotlin
var array = CharArray(10){" "}
```

배열에 값을 넣거나 빼기 위해서는 getter와 setter를 사용하거나 다른 언어에서처럼 배열이름[]을 사용하면된다.

Getter, Setter 사용하기 : 배열명.set(인덱스, 넣을 값), 배열명.get(인덱스)

### 3. Enum에 대해서

작년에 Java를 공부할 때 enum에 대해서 배운 적이 있었던 것 같은데  너무 얼레벌레 배워서 기억이 잘 나지 않는다. 

enum은 **열거형(enumerated type)**이라 부른다. 열거형은 **서로 연관된 상수들의 집합**이다.

그래서 조건문 "when"을 enum 클래스를 다룰 때 사용하는데, 프론트나 서버에서 상수를 사용하지 않고 가독성이 좋게 enum을 사용하면서 이해도를 높일 수 있다는 장점이 있다.

```kotlin
//아마 위에 Color enum class가 정의되어 있겠지..?
fun getMnemonic(color: Color) =
when (color) {
    Color.RED -> "Richard"
    Color.ORANGE -> "Of"
    Color.YELLOW -> "York"
    Color.GREEN -> "Gave"
    Color.BLUE -> "Battle"
    Color.INDIGO -> "In"
    Color.VIOLET -> "Vain"
}
```

### 4. Kotlin의 예외처리와 Java의 예외처리의 차이점

Kotlin은 **체크 예외와 언체크 예외를 구분하지 않는다.** 언체크 예외의 경우 명시적인 예외 처리를 강제하지 않는데, 그래서 언체크 예외라고 불리며 보통 RuntimeException이라고 부른다.

![](https://blog.kakaocdn.net/dn/XBZaW/btrN4kwFCYO/KBVjEOWhggWDz0uqgGBLjk/img.png)

Java에서는 언체크 예외와 체크 예외를 구분하고 그에 대한  throws절이 함수 뒤에 코드에 있어야한다. 

그런데 Java에서 이렇게 구분하면서 생기는 자잘한 문제들이 있고, 그것 때문인지는 모르겠지만(?) **Kotlin에서는 언체크와 체크 예외를 따로 구분하지 않는다고 한다.**

그래서 명시적으로 **throws절이 필요없다**. (이게 코틀린의 예외처리에서 자바와 다른 점이다.)

이 throw 절이 필요없다는 얘기가 정확히 이해가 가지 않았는데 보통 예외를 던질 때는 throw를 사용하지만, 함수를 작성할 때 체크 예외가 발생할 수 있는 상황이여도 throws 절을 쓰지 않아도 된다는 것이다.

어제 스터디때 까지만 해도 정확히 이해가 가지 않았는데, **throw절과 throws절이 다르다**는 것을 몰랐기 때문이다!

throw는 **예외를 강제로 발생**시킨 후,(프로그래머의 판단에 따른 처리) 상위 블럭이나 catch문으로 예외를 던진다. 그러니까 메소드 내에서 상위 블럭으로 예외를 던지는 것이다.

throws는 예외가 발생하면 상위 메서드로 예외를 던진다. throws문을 통해 메소드를 사용하기 위해서 어떠한 예외들이 처리되어야하는지 쉽게 알 수 있다. 이렇게 **예외를 명시하는 것은 예외를 처리하는 것이 아니라 자신을 호출한 메소드에게 예외를 전달하여 예외처리를 떠맡**기는 것이다.(전가)

throw가 메소드 내에서 상위 블럭으로 예외를 던지는 것이라면, **throws는 현재 메소드에서 상위 메소드로 예외를 던지**는 것이다.

### 5. 스마트 캐스트(smart cast) 가 정확히 무엇인가요

스마트 캐스트는 프로그래머가 굳이 원하는 타입으로 캐스팅(타입 변환)하지 않아도, 컴파일러가 알아서 캐스팅해주는 것을 의미한다. 굳이 형변환 연산자를 써주지 않아도 알아서 형변환이 된다고 이해하면 된다.
