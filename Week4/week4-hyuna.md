## 1. 중첩 클래스와 내부 클래스
용어를 보면 결국 둘 다 어떤 클래스 안에 클래스를 또 쓰는 형태인 것 같은데, 둘의 차이점은 무엇일까?
정의를 보면 아래와 같다.

>중첩클래스 : 클래스간의 연게성을 표현하여, 코드의 가독성 및 작성 편의성이 올라갈 수 있다.
내부클래스 : 외부 클래스의 객체를 참조할 수 있다.

차이점은 `외부 클래스의 객체를 참조` 할 수 있는가의 여부다.

중첩 클래스의 경우 _그냥 클래스 안에 쓰인 클래스일 뿐_이다.
연계성을 표현하기 위해 내부에 써주기는 했지만 사실상 별개로 작동하는 클래스나 다름없다.
```kotlin
class Outer {
    class Nested {
        //
    }
}
```


그러나 내부 클래스의 경우_ 외부 클래스의 객체를 참조할 수 있는 클래스_이다.
따라서 내부 클래스는 특별히 클래스 앞에 `inner`키워드를 추가해준다.
```kotlin
class Outer {
    inner class Inner {
        //
    }
}
```
즉, 그냥 안에 쓰인 클래스는 중첩 클래스, 그 중에서도 `inner`키워드를 사용하여 외부 클래스의 객체를 참조할 수 있도록 한 클래스는 내부 클래스다.

</br>

## 2. 주생성자와 부생성자
### 왜 Kotlin에서는 생성자가 두 개의 종류로 나뉠까?
자바는 `디폴트 파라미터`를 지정할 수 없기 때문에 생성자를 많이 사용하게 된다. 그러나 코틀린의 경우 `디폴트 파라미터`를 지정할 수 있어 애초에 생성자를 많이 쓸 일이 없다. 그래도 부 생성자가 필요한 이유는 자바와의 상호운용성을 보장해주기 위함이다.

하지만 부 생성자가 필요한 다른 경우도 있다. 클래스 인스턴스를 생성할 때 파라미터 목록이 다른 생성 방법이 여럿 존재하는 경우에는 부 생성자를 여럿 둘 수밖에 없다.

그래서 만약 생성자를 쓸 일이 생기면 파라미터 써주듯이 메소드의 선언부에 써주고 초기화는 함수 내부에 init으로 해주되, 혹시라도 생성자를 쓸 일이 더 생긴다면 앞서 한 번 초기화해놓은 놈(주생성자)을 그냥 호출해서 부생성자들을 더 만들어주는 식으로 작동하는 것이다.

### 생성자의 작성
```kotlin
class User constructor(_nickname: String) { // 파라미터가 하나만 있는 주 생성자
		val nickName: String

		init { // 초기화 블록
				nickName = _nickName
		}
 ```

</br>

## 3. by 키워드
```kotlin
class Hyundai(private val premiumCar: Car): Car {
	override val tire: String = premiumCar.tire
    override val body: String = premiumCar.body
    override fun manufacture() = premiumCar.manufacture()
}
```

위 코드처럼 어떤 클래스가 상속받은 인터페이스를 구현하고자 하는 상황을 가정해보자. 그런데 실제로 구현하는 코드를 작성하고보니 그 내용이 다 상속받은 내용일 뿐 클래스 자체에서는 스스로 한게 하나도 없어 보인다. 여러 줄을 열심히 썼지면 결론은 `그냥 객체 생성하는 니가 다 해놓으면 내가 그거 가져다 쓸게~` 를 줄줄이 늘어좋은 셈이 되는거다. 

이렇게 하는 일이 하나도 없는 클래스를 줄줄이 작성하고 있어야 할까? ~~쓸모없는 코드를 개발자가 싫어합니다.~~

그래서 나온 것이 바로 `by`이다. 이 키워드는 그냥 한마디로, `다 위임합니다. 알아서 하세요.`의 의미다. 
의미없이 줄줄이 나열했던 코드를 한 줄로 표현하도록 해주는 것이다.

```kotlin
class DelegatedHyundai(preminumCar: Car): Car by preminumCar
```

위의 무의미한 코드들이 `by` 키워드를 이용해 아주 깔끔하게 한 줄로 정리된 것을 볼 수 있다.

</br>

## 4. companion object
### 싱글턴 패턴

자바에서는 보통 클래스의 생성자를 private으로 제한하고 정적인 필드에 그 클래스의 유일한 객체를 저장하는 싱글턴 패턴을 통해 이를 구현한다. 반면에 코틀린은 객체 선언 기능을 통해 싱글턴을 언어에서 기본 지원한다. 객체 선언은 클래스 선언과 그 클래스에 속한 단일 인스턴스의 선언을 합친 선언이다.

### companion object의 필요성
private 생성자를 호출하기 좋은 위치를 알려준다는 했던 사실을 기억하는가? 바로 동반 객체가 private 생성자를 호출하기 좋은 위치다. 동반 객체는 자신을 둘러싼 클래스의 모든 private 멤버에 접근할 수 있다.

```kotlin
class User private constructor(val nickname: String) {
    companion object {
        fun newSubscribingUser(email: String) =
            User(email.substringBefore('@'))

        fun newFacebookUser(accountId: Int) =
            User(getFacebookName(accountId))
    }
}
```

### static과 companion object
static 키워드와 동일하게 다른 클래스에서 객체 생성 없이 companion object에 선언된 변수와 함수에 접근이 가능하다. 이렇게 static과 유사하게 사용되고 있지만 static과 다른 차이점도 존재하는데, companion object는 static과 다르게 런타임에 생성되고 클래스 정의와 동시에 인스턴스가 생성된다.


---
**참고 및 출처**
https://developer88.tistory.com/457?category=298822
