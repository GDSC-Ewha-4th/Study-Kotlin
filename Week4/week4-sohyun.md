## 주 생성자와 부 생성자에 대해

- 주 생성자 : 주로 사용하는 간략한 생성자로, 클래스 본문 밖에서 정의
- 부 생성자 : 클래스 본문 안에서 정의
  
  ```kotlin
  class User(val nickname: String)
  ```
  
  이렇게 클래스 이름 뒤에 오는 괄호로 둘러싸인 코드를 주 생성자(primary constructor)라고 부른다. 주 생성자는 생성자 파라미터를 지정하고 그 생성자 파라미터에 의해 초기화되는 프로퍼티를 정의하는 두 가지 목적에 쓰인다.  
  실제로는 아래와 같은 일이 벌어진다.
  
  ```kotlin
  class User constructor(_nickname:String){ // 파라미터가 하나만 있는 주 생성자
      val nickname: String
      init {
          nickname = _nickname
      }
  }
  ```
  
  새로운 키워드
  - `constructor` : 주 생성자나 부 생성자 정의를 시작할 때 사용
  - `init` : 초기화 블록을 시작. 초기화 블록에는 클래스의 객체가 만들어질 때(인스턴스화 될때) 실행될 초기화 코드가 들어간다.
  
  이 예제에서는 nickname 프로퍼티를 초기화하는 코드를 nickname 프로퍼티 선언에 포함시킬 수 있어서 초기화 코드를 초기화 블록에 넣을 필요가 없다. 또한 주 생성자 앞에 별다른 애노테이션이나 가시성 변경자가 없다면 constructor를 생략해도 된다.
  
  ```kotlin
  class User(_nickname: String){
      val nickname = _nickname;
  }
  ```
  
  > 프로퍼티를 초기화하는 식이나 초기화 블록 안에서만 주 생성자의 파라미터를 참조할 수 있다.
  
  클래스를 정의하는 여러가지 방법
  
  ```kotlin
  // 주 생성자 파라미터 이름 앞에 val을 추가하는 방식으로 프로퍼티 정의와 초기화를 간략히 사용할 수 있음
  // val은 이 파라미터에 상응하는 프로퍼티가 생성된다는 뜻
  class User(val nickname: String)
  // 생성자 파라미터에 대한 디폴트 값을 제공
  class User(val nickname: String, val isSubscribed: Boolean = true)
  ```
  
  클래스의 인스턴스를 만드려면 new 키워드 없이 생성자를 직접 호출하면 된다.
  
  ```kotlin
  // isSubscribed 파라미터에는 디폴트 값이 쓰인다.
  ```

## 부 생성자: 상위 클래스 다른 방식으로 초기화

팁

> 인자에 대한 디폴트 값을 제공하기 위해 부 생성자를 여럿 만들지 말라. 대신 파라미터의 디폴트 값을 생성자 시그니처에 직접 명시하라.

여러 가지 방법으로 인스턴스를 초기화할 수 있게 다양한 생성자를 지원해야하는 경우가 있다.  
코틀린으로 표현한 경우 아래와 같다.

```kotlin
open class View{
    consturctor(ctx: Context){
        // 부생성자
    }
    constructor(ctx: Context, attr: AttributeSet){
        // 부생성자
    }
}
```

이 클래스를 확장하면 똑같이 부 생성자를 정의할 수 있다.

```kotlin
class MyButton: View{
    constructor(ctx: Context): super(ctx){ // 상위 클래스의 생성자를 호출한다.
        // ...
    }
    constructor(ctx: Context, attr: AttributeSet): super(ctx, attr){
        // ...
    }
}
```

자바와 마찬가지로 생성자에게 this()를 통해 클래스 자신의 다른 생성자를 호출할 수 있다.

```kotlin
class MyButton: view{
    // 다른 생성자에게 위임
    constuctor(ctx: Context): this(ctx, MY_STYLE){ 
        // ...
    }
    constuctor(ctx: Context, attr: AttributeSet): super(ctx, attr){
        // ...
    }
}
```

클래스에 주 생성자가 없다면 모든 부 생성자는 반드시 상위 클래스를 초기화하거나 다른 생성자에게 생성을 위임해야한다.
