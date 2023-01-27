**Anko**
- 안드로이드에서 코드를 작성할 때 복잡한 코드 또는 상용구를 제거하기 위해 JetBrains®에서 특별히 개발한 확장 기능 및 DSL 모음

 

https://github.com/Kotlin/anko 를 참조하여 Anko 라이브러리에 대해 참조할 수 있다.

 

***Anko 의 구성***은 총 네 가지로, 아래와 같다.
Anko Commons: 인텐트, 다이얼로그, 로그 등을 편리하게 사용하는 라이브러리
Anko Layouts: 안드로이드 레이아웃을 코드로 쉽게 작성하는 라이브러리
Anko SQLite: SQLite를 쉽게 사용하는 라이브러리
Anko Coroutions: 코루틴을 쉽게 사용하는 라이브러리
 
***Anko 라이브러리 추가하기***
1. Anko의 모든 구성 요소를 한 번에 추가하려면 아래와 같은 코드를 사용한다.

```
dependencies {
    def anko_version = '0.10.8'
    implementation "org.jetbrains.anko:anko:$anko_version"
}
```

2. 다른 기능을 추가하려면 아래와 같은 코드를 사용한다.

```
dependencies {
    // Anko Commons
    implementation "org.jetbrains.anko:anko-commons:$anko_version"

    // Anko Layouts
    implementation "org.jetbrains.anko:anko-sdk25:$anko_version" 
    implementation "org.jetbrains.anko:anko-appcompat-v7:$anko_version"

    // Coroutine listeners for Anko Layouts
    implementation "org.jetbrains.anko:anko-sdk25-coroutines:$anko_version"
    implementation "org.jetbrains.anko:anko-appcompat-v7-coroutines:$anko_version"

    // Anko SQLite
    implementation "org.jetbrains.anko:anko-sqlite:$anko_version"
}
``` 

***Anko를 이용하여 Toast와 Alert를 간단하게 작성해보기***

 

1. Toast

```
// default
Toast.makeText(this,"안녕하세요.",Toast.LENGTH_LONG).show() 

// anko      
toast("안녕하세요.")
```
 

2. Alert

Anko 없이 alert를 작성하는 것은 번거롭다. 

DialogBuilder을 사용하거나 Dialog 클래스를 확장하는 두 가지 방법이 있다.

하지만 Anko 라이브러리를 사용하면 간단하게 alert 코드를 작성할 수 있다.

```
alert ("Are you sure?"){
  yesButton {toast("Thank you!")}
  noButton {toast("Okay!")}
}.show()
```
또는
```
alert {
   yesButton { "Aha!" }
   noButton { "Burrrr!" }
 }.apply {
   title = "Naaa"
   show()
 }
 ```

그 밖의 더 많은 코드를 간결하고 단순하게 작성할 수 있다.
