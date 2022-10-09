# 코틀린 FAQ_week2

Kotlin in Action 1장, 2장을 공부한 뒤, 새롭게 알게 된 부분과 그 전과 달리 궁금증이 해결된 부분에 대해 FAQ를 작성하겠다. ~~사실 모르는 게 너무 많았지만~~ 추가적인 부분은 차차 정리해나가겠다.

### **1. 코틀린의 배열 선언 방식/ 자바와의 차이점**

코틀린은 배열을 선언하기 위한 문법이 따로 존재하지 않는다. 그래서 코틀린을 사용하여 배열을 선언할 때는 Array클래스로 표현된다.

Array 에 대한 코틀린 공식 문서 ▼

[Array - Kotlin Programming Language](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-array/)

arrayOf(), arrayOfNulls() 등과 같은 방법으로 배열을 선언할 수 있다.

- arrayOf() 는 배열을 선언하는 동시에 배열에 데이터를 넣을 수 있다.

var 변수이름 = arrayOf(value1, value2, value3...)  ->  [value1, value2, value3...]을 생성한다.

- arrayOfNulls() 는 특정 자료형의 배열을 원하는 크기만큼 null로 초기화하면서 선언한다.

var 변수이름 = arrayOfNulls<변수타입>(배열크기)  ->  [null,null,null....]을 생성한다.

### **2. enum 이란?**

**enum**: 열거형(enumerated type)이다. 연관된 자료형으로만 작성된 집합을 나타낸다.

굳이 enum class를 사용하는 이유는

1. 코드가 단순하지면서 가독성이 좋아진다.

2. enum 키워드를 사용하면서 열거의 의도를 명확하게 드러낼 수 있다.

- 코틀린에서 세미콜론(;)을 사용하는 유일한 경우가 enum 과 관련하여 등장한다. enum 상수를 선언할 때 상수 목록과 메소드 정의 사이에 세미콜론(;)을 넣어줘야 한다.
-

### **3. 스마트 캐스트란?**

자바에서는 형변환을 할 때 변수의 타입 검사를 위해 명시적으로 표시하지 않아도 컴파일러가 자동으로 캐스팅해주는 것을 말한다.

물론 'is'를 사용하여 변수의 타입 검사를 하는데, 이후에 컴파일러가 자동으로 형변환을 해준다는 것이다.

~~이렇게 이해했다. 이게 맞겠지...~~

### **4. 코틀린의 예외처리/자바와 다른 점**

코틀린에서 예외처리 할 때, 자바와 비슷하게 오류가 발생하면 예외를 throw할 수 있고, 함수를 호출하는 쪽에서 예외를 catch 할 수 있다.

하지만 자바와 달리, 코틀린은 throws 절을 코드에 작성하지 않는다.

체크 예외, 언체크 예외에 대한 그림을 보자 ▼

![https://blog.kakaocdn.net/dn/yE6jp/btrN4Fnk6ch/MQnoxa8Li9karKPTYbBU5k/img.png](https://blog.kakaocdn.net/dn/yE6jp/btrN4Fnk6ch/MQnoxa8Li9karKPTYbBU5k/img.png)

체크 예외가 발생하는 메소드를 사용할 때 반드시 예외를 처리하는 코드를 작성해야 한다.(메소드가 체크 예외를 던진다면 이를 catch 하던지, throws를 정의해야한다. 그렇지 않으면 컴파일 에러가 발생한다.)

언체크 예외의 경우, 명시적으로 예외 처리를 요구하지 않다. 보통 RuntimeException을 가리킨다.

코틀린에서는 이 체크 예외와 언체크 예외를 구분하지 않기 때문에 throws절을 코드에 작성하지 않아도 되는 것이다.

**[notice]**

throws 절과 throw 절은 구별되는 것!

**throws**를 강제로 발생시킨 후, 상위 블록이나 catch 문으로 예외를 던진다.

**throw**는 예외가 발생하면 상위메소드로 예외를 던진다.

### **5. Property 란?**

코틀린에서는 val, var로 정의되는 변수를 프로퍼티라고 한다.

프로퍼티는 자바의 field+getter/setter 메소드 라고 볼 수 있다. (프로퍼티를 생성하면 getter 와 setter 가 자동으로 생성된다.)
