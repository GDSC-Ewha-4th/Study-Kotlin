# Kotlin Study 3주차 함수의 정의와 호출

이번 스터디에서는 모두 어렵게 느끼는 부분이 비슷해서 확장함수와, 확장 프로퍼티,  중위호출 위주로 살펴보았다.

코틀린에는 낯설게 다가오는 개념이 많은 것 같다! 

우리가 우리 나름대로 이해해서 결론을 내리기는 했지만, 이게 정확한 개념이 아닐 수도 있다. 

이 정도로 이해하면 쓸 때 무리가 되지 않을 것 같게 되었을때 결론을 내린 바를 적겠다!



## 확장 함수

확장함수란 클래스 바깥에서 선언이 되고 이클립스에 불러서 사용할 수 있는 것이라고 다들 이야기를 하였다. 이렇게 들으면 대체 무슨 소리인지 모르겠는데 예시를 살펴보면 좀 이해가 된다.

```kotlin
fun <T> Collection<T>.joinToString(
	separator: String = ", ",
	prefix: String = "",
	postfix: String = ""
): String {
		val result = StringBuilder(prefix)
		for ((index, element) in this.withIndex()) {
			if (index > 0) result.append(separator)
			result.append(element)
		}
	result.append(postfix)
	return result.toString()
}
fun main(args: Array<String>) {
	val list = arrayListOf(1, 2, 3)
	println(list.joinToString(" "))
}
```

여기서는 Collection의 확장함수로 joinToString을 만들어주었다.

이렇게 생겨난 함수 joinToString은 Collection 클래스의 일부가 아니다. 

그리고 확장함수는 **오버라이드가 불가**하다!



>  만일 확장한 함수와 그 클래스의 멤버 함수의 이름과 시그니처가 같다면 확장 함수가 아니라 멤버 함수가 호출된다고 한다!



아래 링크를 확인하면 더 잘 이해될 것 같아 첨부한다!

[범위 지정 함수와 수신 객체에 대하여](https://learnrecord.tistory.com/8)



확장함수는 수신 객체에 빈 값이 들어가는게 가능하다.

바로 아래를 보자.

**Nullable Receiver**

확장자는 nullable한 타입으로 정의되어 있다는 것에 주의. 이러한 확장은 그 값이 null 인 경우에도 객체 변수에서 호출 가능하고 본문에서 `this == null` 으로 체크할 수 있다. 이것은 null을 체크하지 않고 Kotlin에서 toString()을 호출할 수 있는 이유.

```kotlin
fun Any?.toString(): String {
    if (this == null) return "null"
    // after the null check, 'this' is autocast to a non-null type, so the toString() below
    // resolves to the member function of the Any class
    return toString()
}
```



## 확장 프로퍼티

확장함수와 이어서 어려웠던 개념은 확장 프로퍼티인데, 확장 프로퍼티를 사용하면 기존 클래스 객체에 대한 프로퍼티 형식의 구문으로 사용할 수 있는 API를 추가할 수 있다.

우리는 여기서 API에 꽂혀서.. 대체 왜 API라는 용어를 사용하는지에 대해 계속 토의했다.

(프론트/클라에서 개발해본 사람이 많아서 API는 명세서로 밖에 안받았다고.. 대체 뭐냐며.. 얘기했다..ㅎ)

API와 관해서는 다음과 같이 이해했다

기존 클래스에 새로운 객체를 추가해준다는 의미 ⇒ API를 추가한다.



프로퍼티의 **예시**를 보자. 

```kotlin
val String.lastChar: Char

get() = get(length -1)
```

여기서 프로퍼티는 일반 프로퍼티랑 똑같고 수신객체가 호출된다.

프로퍼티 문법으로 더 짧게 코드를 작성이 가능하여 편한 경우가 있다. 



# 중위 호출

그 다음으로 어려웠다고 얘기한 부분은 중위 호출인데, 생각보다 어려운 개념이 아니었다.

실제 안드 개발하는 현아가 to를 엄청 많이 사용한다고 얘기해서 나는 어떤 Mapping 함수겠거니 생각헀는데 중위호출이라는 기능 자체는 어디든 많이 사용되는 기능이었다.

(책에서 예시로 나왔던 to는 hashMap 같이 사용되는 기능을 가지고 있었다.)



중위 호출은 값의 쌍을 다룰 때 많이 사용되는데, 확장함수를 통해서도 사용할 수 있다. 만일 함수 자체를 중위호출에 사용하고 싶으면 함수 앞에 `infix` 변경자를 추가해야한다.



아래 링크들을 참고하여 함께 논의하였다.

https://smoh.tistory.com/238

https://lwndnjs93.tistory.com/2
