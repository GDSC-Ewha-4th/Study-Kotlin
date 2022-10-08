# Kotlin 스터디 1주차 정리

1주차에는 각자가 생각하는 Kotlin의 장점에 대해 간략히 정리해보았습니다.


## Coroutine이란?

coroutine하면 어떤 것들이 연상되는가?
MainRoutine 과 SubRoutine을 떠올렸다면 당신은 진정한 개발자다. (?)

MainRoutine이 SubRoutine을 호출하면 진입 시점부터 코드를 실행하다가 return문을 만나는 시점에 해당 SubRoutine을 탈출하게 된다.

>즉, SubRoutine은 진입 시점과 탈출 시점이 명확하게 하나씩 정의되어 있는 루틴이다.

</br>

그러나 coroutine의 경우는 조금 다르다.
coroutine에 대해 Kotlin 공식 문서에서는 이렇게 설명하고 있다.

>A coroutine is an instance of suspendable computation.

즉, 언제든 일시중단 가능한, 탈줄 시점이 한 개가 아닌 routine이라고 할 수 있다.
이러한 특징은 coroutine의 큰 장점이라고 할 수 있는데, 바로 동시성 프로그래밍을 스레드보다 효율적으로 가능하게 하기 때문이다.

</br>

## Coroutine과 동시성 프로그래밍

스레드는 일종의 프로세스 실행 단위이고, 동시성 프로그래밍을 위해 멀티 스레딩을 하려면 _context switching_이 필요하다.
쉽게 말해 여러 개의 스레드를 이용해 동시성 프로그래밍을 하게 될 경우 이 스레드에서 한 번, 저 스레드에서 한 번, 점유했다, 놓아줬다, 왔다 갔다를 계속 반복해야 한다는 것이다.

하지만 coroutine을 이용하면 이러한 동시성 프로그래밍을 좀 더 효율적으로 할 수 있다.
앞서 말했듯, coroutine도 일종의 routine이다. 
따라서 **하나의 스레드** 내부에서 그냥 routine만 옮겨다니며 동시성 프로그래밍을 할 수 있게 되는 것이다. 

</br>


## Kotlin과 Coroutine

이렇듯 coroutine 자체는 언뜻봐도 너무 좋은 개념이지만 언어에 따라 coroutine을 지원하지 않는 경우도 있다... ([지원 언어 목록 참고](https://en.wikipedia.org/wiki/Coroutine#Native_support))
Kotlin의 경우는 특정 coroutine을 언어가 지원하는 형태는 아니고 coroutine을 구현할 수 있는 기본 도구를 언어가 제공하는 형태다. 
또한 1.3부터는 설치만 하면 별도의 설정 없이도 coroutine 지원 기본 기능들을 사용할 수 있게 되었다.
Kotlin에서 coroutine을 어떻게 구현하는 지에 대해서는 Kotlin을 제대로 공부한 후에 더 깊이있게 알아볼 계획이다.

</br>

코틀린 이해에 도움이 될 만한 공식 자료들 (아래 내용을 참고하여 작성하였습니다.)
- https://developer.android.com/kotlin/coroutines?hl=ko
- https://kotlinlang.org/docs/coroutines-guide.html