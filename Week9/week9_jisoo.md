# 코틀린 FAQ_week9

### 스타 프로젝션과 Any

스타 프로젝션(*)는 어떤 타입이 들어올지 알 수 없어도 안전하게 사용할 수 있다. 하지만 구체적인 타입이 정해지면 해당 타입만 받을 수 있다. *는 구체적인 타입이 정해지기 전까지는 Any?로 취급된다.

Any는 언제든지 모든 타입을 받을 수 있다. 모든 타입이 상속받는 최상위 타입. (자바로 디컴파일하면 Object 타입으로 변환됨.)

### reified

‘구체화’

reified type parameter 와 함께 inline 함수를 만들면 Class<T>를 파라미터로 넘겨주지 않아도 런타임에 타입 T에 접근할 수 있다. 

→ reified는 inline 함수와 조합해서 사용한다.

→ 인라인을 하고싶지 않으면 noinline 키워드를 붙여준다.

⇒ reified type과 인라인 함수가 호출되면 컴파일러는 type argument로 사용된 실제 타입을 알고 만들어진 바이트코드를 직접 클래스에 대응되도록 바꿔준다.

### 변성

공변: 타입a가 타입b의 하위 타입일 때 → list<a>가 list<b>의 하위 타입인 경우

반공변: 타입a가 타입b의 하위 타입일 때 → list<a>가 list<b>의 상위 타입인 경우

무공변: 타입a가 타입b의 하위 타입일 때 → list<a>와 list<b>가 아무 관계도 아닌 경우

→공변, 반공변, 무공변으로 메소드의 인자로 들어오는 파라미터의 타입에 제한을 걸 수 있다.

- 공변의 경우 Kotlin에서는 out이라는 키워드
- 반공변의 경우 Kotlin에서는 in이라는 키워
- 무공변의 경우 in, out 상관이 없다.

![Untitled](https://user-images.githubusercontent.com/113654733/212273646-8592ed53-dcbd-42cd-a9ac-5cbf6e5b7d6e.png)
