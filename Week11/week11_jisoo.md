# 코틀린 FAQ_week11

### DSL을 사용하면 반복되는 코드의 효율적인 재사용이 가능하다?

메서드를 반복해서 호출하지 않고도 메서드를 호출할 수 있다.

→ Dsl은 메서드 호출 조합에 대한 인테페이스를 제공하기 때문에 일련의 과정을 반복하지 않고도 원하는 메서드를 불러올 수 있다.

그래서, 잘 설계된 dsl로 구현한 코드는 쉽게 유지 보수할 수 있다. → 이는 가장 빈번히 바뀌는 애플리케이션 부분에 특히 중요하다.

또한, 프로그래머가 특정 코드에 집중할 수 있어서 결과적으로 생산성이 좋아진다. 

### 내부 DSL

**내부 DSL**이란

‘독립적인 문법 구조를 가진 SQL과 같은 위부 DSL과는 달리 DSL의  핵심 장점을 유지하면서 범용언어(코틀린)를 동일한 문법으로 사용하는 것’

과제가 생겨버렸다.. **분야별 내부 dsl 파헤쳐보기**