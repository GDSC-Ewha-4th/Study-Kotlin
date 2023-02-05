## DSL
> DSL 구조의 장점은 같은 문맥을 함수 호출 시마다 반복하지 않고도 재사용할 수 있다는 점이다.

위 문장에서 _같은 문맥을 함수 호출 시마다 반복하지 않고도 재사용할 수 있다_는 것은 어떤 의미일까?

SQL을 예시로 들어보겠다. 우리가 여러 데이터가 존재하는 테이블에서 특정 열의 값만을 선택해서 보고 싶다고 가정해보자. 

만약 범용 언어를 이용해서 데이터의 값을 선별하려면 해당 기능을 하는 메서드를 직접 작성하고, 특정 인자를 넘겨주고 받으며 원하는 데이터 값을 반환하도록 함수를 호출해주는 과정이 필요하다. 만약 다음에도 또 비슷한 유형의 데이터 값을 선별하고 싶다면? 그 함수에 다시 인자를 넘겨서 호출한 뒤 반환값을 받아오는 일련의 과정을 또 거쳐야만 한다. 

즉, 번거롭다!

그런데 여기서 SQL이라는 DSL을 쓰게 된다면 상황이 달라진다.

SQL은 DSL인 만큼 필요한 결과 집합의 여러 측면을 기술하는 메서드 호출을 조합해서 만든 질의로 이미 구조가 만들어져 있다. 이 예시에서는 `SELECT` 문이 적합한 예시가 될 것이다. `SELECT` 하나만 이용하면 원하는 그 특정 열의 값을 쉽게 불러올 수 있고, 나중에도 언제든 이 `SELECT` 를 재사용할 수 있다.

이처럼 DSL의 구조는 같은 문맥을 여러 번 반복하지 않아도 된다는 장점을 갖고 있다.

<br>

그렇다면 이러한 DSL은 실제로 어떻게 쓰일 수 있을까?

<br>

책에서 제시하는 안드로이드 분야의 DSL로는 Anko가 있었다. 
그래서 이 Anko에 대해 제대로 알아보기 위해 구글링을 한 순간, Anko가 deprecated 되었다는 사실을 알게 되었다 .....😇
![image](https://user-images.githubusercontent.com/63237214/216827088-e7559852-984e-44e2-ab56-981a990620df.png)
눈물나는 GOODBYE.md ...

그리고 이 마크다운 아래에는 Anko를 대체할 수 있는 것들에 대해 설명하고 있었는데,
![image](https://user-images.githubusercontent.com/63237214/216827199-325c71e6-b14b-47fe-8a3d-899be5ef0e02.png)

Jetpack Compose?????
<br>

이럴수가.
<br>
<br>
요즘 안드로이드 개발에서 빼놓을 수 없는 개념인 Jetpack Compose가 그 대체제라니, 더이상은 이렇게 Jetpack compose를 모르는 채로 버틸 수 없겠구나 싶어서 Jetpack Compose에 대해 알아보기로 하였다.
<br>

## Android Jetpack
Jetpack Compose를 다루기에 앞서, Jetpack이 무엇인지부터 알아보자.

Android Jetpack은 Components, tools, guidance의 집합으로, 기존의 Support Library 및 Arichitecture Componets를 결합하여 아래 그림과 같이 네 가지 카테고리로 나누어 제공한다.

![](https://velog.velcdn.com/images/akimcse/post/a1173dca-8705-4830-8729-fc1c3dfef4e5/image.png)

Data binding, Lifecycles, Navigation, Room, ViewModel, Transition, Fragment, Layout, 등... 

안드로이드 개발을 해본 사람이라면 한번쯤은 다 들어봤을 법한 것들을 이렇게 묶어서 제공하고 있다.

#### 이런 게 왜 필요하지?
1. 원하는 대로 갖다 쓴다!
이 component들은 Android 플랫폼이 아닌 `unbundled` 라이브러리로 제공되어 _원하는 속도와 시간에 각 component를 채택할 수 있도록_ 하였다.


2. 호환성을 높여 준다!
어떤 버전이든 독립적으로 기능을 제공하도록 빌드되었기 때문에 이전 버전과의 호환성을 제공하여 _다양한 버전의 플랫폼에서 실행될 수 있도록_ 하였다.

따라서 기존보다 적은 코드로도 퀄리티 있는 앱을 만들 수 있도록 한다는 것이 안드로이드 공식 블로그에서 소개하고 있는 내용이다.

> 쉽게 말하자면 개발을 효율적으로, 그리고 편하게 할 수 있도록 만들어 놓은 모든 것들의 집합이라고 할 수 있다.

<br>

## Jetpack Compose
Jetpack 까지는 대충 알겠다. 그럼 여기서 Jetpack Compose는 또 뭘까?

Jetpack Compose는 Android Jetpack의 수많은 라이브러리 중 하나로, 모양과 데이터 종속 항목을 설명하는 구성 가능한 함수를 사용하여 프로그래매틱 방식으로 UI를 정의할 수 있도록 해주는...

이렇게 말하면 하나도 와닿지 않는다! ^___^

다른 설명을 가져와보면, Jetpack Compose는 Android 네이티브 UI를 빌드하기 위한 현대적인 선언형 UI 툴킷으로, 뷰를 명령형으로 변형하지 않고도 앱 UI를 렌더링할 수 있게 하는 선언형 API를 제공하여 UI 개발을 간소화하고 가속화한다고 한다.

>즉, 선언형 API를 통해 Android UI를 더 빠르고 쉽게 빌드할 수 있도록 도와준다는 것이다.

_그런데 선언형이 도대체 무엇이길래 UI 빌드가 더 쉬워진다는 것일까?_

### 선언형 프로그래밍 패러다임
Compose 이전의 기본 Android 뷰 계층 구조는 UI 위젯의 트리로 표시할 수 있었다.
보통은 xml을 통해 뷰를 구성하여 UI 구조를 짜고, Kotlin 파일에서 그 뷰를 참조해 쓰는 식으로 코드를 작성하게 된다.

그런데 이러한 방법의 경우 모종의 이유로 뷰를 조작하게 될 때 오류가 발생할 가능성이 커진다.

데이터를 여러 위치에서 렌더링한다면 그 모든 위치의 뷰를 다 업데이트해야 하는데, 하나라도 빼먹는 순간 오류가 생기고.. 개발자는 뭐가 문제인지 찾아다니기 시작하고... (xml단의 오류는 에러 메세지가 제대로 뜨지도 않는다! 젠장!)

이렇게 업데이트가 필요한 뷰의 수가 많아질수록 소프트웨어의 유지관리 복잡성이 증가하게 된다.

이에 따라 지난 몇 년에 걸쳐 업계 전반에서는 _선언형 UI 모델_ 로 전환하기 시작했는데, 이는 처음부터 화면 전체를 개념적으로 재생성한 후 필요한 변경사항만 적용하는 방식으로 작동하게 된다. xml을 이용하던 명령형 UI 모델보다 훨씬 효율적으로 작업이 가능하다는 것이다.

우리가 흔히 아는 Flutter나 Swift UI의 경우에도 이 선언형 UI를 채택하고 있다.

### 동적 콘텐츠
Compose를 사용하면 데이터를 받아서 UI components를 내보내는 구성 가능한 함수(composable functions) 집합을 정의하여 UI를 빌드한다. 즉, xml이 아닌 Kotlin으로 작성되는 이 함수는 메인 로직을 처리하는 다른 Kotlin 코드들과 마찬가지로 동적으로 작동할 수 있다.

- if문을 사용할 수 있고
- 루프를 사용할 수 있으며
- 도우미 함수를 호출할 수 있다

즉, 기본적으로 언어가 제공하는 모든 유연성이 UI 빌드에도 적용되는 것이다.

<br>

## Compose를 채택하는 이유
대충 훑어보니, 선언형 UI를 통해 더 쉽고 직관적으로 UI를 빌드할 수 있도록 한다는 건 알겠다.

근데 기존에도 xml과 뷰바인딩을 이용해서 대충 잘 개발해왔던 것 같은데, 굳이 이렇게 완전히 새로운 패러다임으로 넘어가려는 이유가 뭘까?

구글 공식 문서에서 설명하는 Compose의 장점은 아래와 같다.

### 코드 감소
> Compose를 사용하면 Android의 View 시스템을 사용할 때에 비해 더 적은 코드로 많은 작업을 할 수 있다.

작성하는 코드를 Kotlin과 xml로 나누지 않고 Kotlin으로만 작성하기 때문에 빌드 중인 대상을 보다 쉽고 간단하게 유지관리할 수 있다. 또한 복잡한 component의 코드도 쉽게 읽을 수 있다고 한다.

### 직관적
> Compose는 선언적 API를 통해 Compose가 나머지를 알아서 다 처리하므로 개발자는 UI를 설명하기만 하면 된다.

'어떻게' UI를 구성해야 하는지 모두 명령해야 했던 이전과는 달리, 개발자는 compose에게 '무엇을' 구성할 지만 전달하면 된다.

### 빠른 개발 과정
>Composet는 기존의 모든 코드와 호환되어 Navigation, ViewModel, Kotlin 코루틴과 같은 대부분의 일반적인 라이브러리와 함께 작동한다.

언제 어디서든 원하는 라이브러리를 채택하여 빠르게 필요한 코드들을 작성할 수 있게 된다.

### 강력한 성능
> Compose는 Android 플랫폼 API에 직접 액세스하고 머터리얼 디자인, 다크모드, 애니메이션 등을 기본적으로 지원한다.

특별한 도구가 없어도 애니메이션을 만들 수 있으며, 쉽고 빠르게 앱의 기능을 구현할 수 있다.

<br>
<br>

Jetpack compose는 기존 선언형 UI의 붐에 힘입어 공개 이후로 꾸준히 안드로이드 생태계의 이슈가 되어오곤 했다.

늦은 감이 없지 않아 있지만... 오늘 Jetpack Compose에 대해 알아본 것을 시작으로 하여 실제 Compose를 이용한 안드로이드 앱 개발까지 도전해보고자 한다.

따라서 본 시리즈의 다음 포스팅부터는 실제 Compose를 앱에서 어떻게 적용하는지에 대해 다뤄보도록 하겠다.

-----
참고 및 출처

https://android-developers.googleblog.com/2018/05/use-android-jetpack-to-accelerate-your.html
https://developer.android.com/jetpack/androidx/releases/compose
https://developer.android.com/jetpack/compose
https://developer.android.com/jetpack/compose/why-adopt
https://developer.android.com/jetpack/compose/mental-model
