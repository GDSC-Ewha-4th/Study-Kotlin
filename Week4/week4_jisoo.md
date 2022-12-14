# 코틀린 FAQ_week4

## 중첩 클래스와 내부 클래스

자바에서는 A클래스 안에 B클래스를 선언하면 내부 클래스이지만 코틀린에서는 A클래스 안에 B클래스를 선언하면 중첩 클래스이다.

A클래스 안의 B클래스 앞에 Inner. 키워드를 붙이면 내부 클래스가 된다. 내부 클래스에서는 B클래스가 A클래스의 인스턴스에 대해 접근할 수 있는 권한이 생기고, 중첩 클래스는 바깥 클래스에 대해 접근할 수 없다. 

Inner 키워드를 붙여 내부 클래스를 사용하는 것이 더 편리해 보이긴 하지만 간혹 누수가 생길 수 있기 때문에 중첩 클래스를 사용하는 것을 지향해야 한다. 

✨ **그렇다면 중첩 클래스를 왜 사용해야 하는가?**

>> 중첩 클래스는 바깥 클래스와 그 안의 클래스 간의 높은 연계성을 보여주기 위해 사용한다. 

## 주 생성자와 부 생성자

주 생성자는 초기화 블록과 함께 사용된다.

부 생성자는 상위 클래스를 여러 방식으로 초기화할 수 있다. 만약 주 생성자가 없이 부 생성자만 있다면 컴파일러가 파라미터가 없는 키워드 ex. super() 등 을 통해 상위 클래스의 생성자를 호출 할 수 있다.

✨ **그런데 부 생성자는 왜 존재하는가?**

자바는 생성자를 여러 개 만들 수 있지만 코틀린은 주 생성자를 선언부에 하나만 만들 수 있기 때문에 생성자를 여러 개 만들고 싶은 경우 부 생성자를 본문에 더 만들 수 있는 것이다.

자바에서는 생성자를 여러 개 만들어야 하는 경우가 많지만, 코틀린은 디폴트 파라미터가 있기 때문에 생성자가 여러 개 필요한 경우가 적다. 그래서 애초에 코틀린은 주 생성자만 있어도 되기 때문에 만약에 더 생성자를 만들 일이 있다면 부 생성자를 만드는 것이다. 

⭐ 그러니까 자바보다 코틀린이 더 번거로운 것이 아니고, 코틀린에는 편리하게 쓸 수 있는 **‘디폴트 파라미터’**가 있기 때문에 주 생성자와 부 생성자를 구분하는 것이다!!! 

## Decorator 패턴, Delegate 패턴, by 키워드

상속: 클래스와 클래스간

위임: 객체와 객체 간

**Decorator 패턴**: 

어떤 객체에 책임(기능)을 동적으로 추가

>> 기존 코드를 변형하지 않고도 기능을 확장 가능하다.

>> 위임을 사용하는 이유는 상속은 코드를 쓸 때 기존 코드에 가지 않아도 코드를 써가면서 그때 그때 수정을 할 수 있기 때문에 편하다.

**by 키워드:** 

by 키워드를 통해 그 인터페이스에 대한 구현을 다른 객체에게 위임 중이라는 것을 명시할 수 있다. 

Delegate 패턴을 쓰기 위해 by 키워드를 사용한다.

- Delegate 패턴이란?
    
    어떤 객체에게 기능을 수행하기 위한 기능을 다른 객체에게 위임하는 패턴이다.
    
    ✨ **왜 상속이 아니라 Delegate 패턴을 사용할까?**
    
    어떤 인터페이스 A를 B에서 구현했다고 쳐보자. B를 상속하는 C 객체를 만든다면 C는 B에서 불필요한 요소도 상속받아야 한다. 이럴 바에는 C에게 B의 기능을 위임하게 해서, 동적으로 필요한 부분만 활용해서 쓸 수 있도록 하는 것이 더 편리하다.
    

## 동반 객체 companion object

**동반 객체(companion object)**는 클래스에 있는 객체이기 때문에 싱글턴이다. 

동반 객체는 팩토리 메서드를 담을 때 쓰인다.

팩토리 메서드란, 부모 클래스가 서브 클래스에게 구현을 위임하는 것이다. 

✨**싱글턴 패턴**이란? 

>> 어떤 클래스에 오직 하나의 객체만 있음을 보장하고, 이 객체를 바깥의 클래스에서 접근할 수 있다.

>> 어플리케이션의 시작부터 종료까지 1번의 생성으로 고정된 메모리 영역을 가지기 때문에 메모리를 효율적으로 사용할 수 있다. 

🎉 동반 객체의 멤버는 클래스 선언과는 관계 없는 최상위 멤버라서, 동반 객체를 둘러싼 어떤 클래스의 private 멤버에도 접근가능하다!!

>> 동반 객체의 멤버 == 최상위 멤버, 클래스 선언과 관계 없음

마치 자바에서 static 키워드를 붙이면 클래스 선언하지 않아도 어떤 멤버에도 접근가능하다는 것과 비슷하다.

⭐ 원래 private멤버는 다른 곳에서 호출하지 못하도록 막아둔 것이라서 수동으로 호출을 했어야 했는데, 코틀린에서는 companion object이라는 개념을 만들어서 private 호출이 더 편리하도록 만들어 둔 것이다.
