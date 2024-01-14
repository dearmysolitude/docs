모든 권리는 영상 제작자에게 있습니다.

![토스ㅣSLASH 22 - 지속 성장 가능한 코드를 만들어가는 방법](https://www.youtube.com/watch?v=RVO02Z1dLF8)

## 발화

토스에서는 코드 품질에 관심을 가지고 확장 가능한 방식으로 코드를 관리한다. ==처음부터 최고의 설계나 품질을 유지하려는 것보다는, 최소 규칙을 지켜 동작하는 소프트웨어를 만들고, 코드에 관심을 두고 성장시켜 나간다.== 코드 한줄 한줄에 의미를 두고 "왜?"라는 질문을 하는 것을 권장한다.

가령, 생성자문(文)을 통해 생성자 안의 의존성이 필요한지 생각해 볼 수 있다. 

이 영상에서는 대개는 IDE에 의해 접혀있는 **import을 살펴보고 코드를 어떤 방향으로 개선할 수 있는지 이야기해 볼 것이다.**

## 본론

### 1. Package

Package 구성에는 다양한 관점과 방법이 있다. 

 햄버거 세트를 어떻게 포장해야 하는지 관리해야 한다고 생각해 보자.
 
 1. 햄버거 세트는 콜라와 감자 튀김, 햄버거로 구성되고, 세트 전체를 포함하여 각각에 적절한 포장(Package)에 대한 고민을 해야 한다.
 2. Package 전략은 고정되지 않고 유연해야 한다. 햄버거를 햄버거 가게 정책의 변경으로 햄버거 세트 최소 주문 수량이 100개가 되었다고 생각해보자. 햄버거를 한 박스에, 감자 튀김을 한 박스에, 콜라를 한 박스에 모아 담는 등의 전략을 사용할 수 있다. 

이와 같이 packaging의 유연성은 전략에 따라 응집에 대해서 지켜내야하는 가치이다.

다음과 같은 예시를 살펴보자(Kotlin).

```kotlin
package com.tosspayments.payments.core.service

import ...

@Service
class CardService(
	private val orderValidator : OrderValidator,
	private val storeReader : StoreReader,
	private val cardReader : CardReader,
	private val cardValidator : CardValidator,
	private val cardPaymentProcessor : CardPaymentProcessor
) {
	fun addPayment(cardPaymentRequest: CardPaymentRequest){
		val order: Order = cardPaymentRequest.toOrder()
		orderValidator.validate(order)
		
		val customerCard: CustomerCard = cardPaymentRequest.toCustomerCard()
		val card: Card = cardReader.read(customerCard)
		cardValidator.validate(card)
		
		val store: Store = storeReader.read(cardPaymentRequest.storeId)
		
		cardPaymentProcessor.newPayment(store, card)
	}
}
```

생성자를 통해 무엇을 할 지 예상할 수 있고, addPayment의 로직도 흐름이 있는 것을 확인할 수 있다.
Import 문을 펼쳐서도 한 번 살펴보자.

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216818&authkey=%21APHGRl7GU5mpfN8&width=710&height=609)

카드에 대한 개념의 응집이 잘 이루어지지 않았다. 카드 서비스는 카드라는 개념에 속한 클래스인데, Card, CardPaymentRequest, CustomerCard 같이 같은 개념에 속하는 것으로 보이는 클래스들 import로 사용해야 하는 점이 아쉬우며, import 문도 아주 다른 결로 나뉘어, 패키지가 분산되어 구성되어 있다는 느낌을 받을 수 있다: 마치 빵, 패티, 야채가 따로 포장되어 있는 것처럼 느껴진다.

CardService 생성자에 있는 CardReader, CardValidator, CardPaymentProcessor도 Card에 대한 컴포넌트로 유추되는데, import문이 필요한지에 대한 의문도 가질 수 있다.

이를 응집을 생각하며 수정하면 다음과 같이 수정할 수 있다.

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216810&authkey=%21ABBz24L2L8NPhF0&width=697&height=457)

1. import 양이 줄어들었다. 
2. Card관련 클래스들의 import문이 사라짐
3. CardReader, CardValidator, CardPaymentProcessor는 이제 import가 필요 없다: 즉, **CardService가 존재하려면 이 클래스들이 필요하면서 가까운 곳에 응집되어 있다**는 의미이다.

기존에는 package가 역할에 따라 나뉘어 있었다면, 현재는 개념끼리 묶여있는 것이다. 즉, ==사람들이 이해하기 쉬운 모양이 되었다.== 하지만 이렇게 개념별로 패키지를 묶었을 때, 하나의 개념에 클래스가 너무 많다면 어떻게 해야할까? import 를 줄여야하니 Package를 더 나누면 안되는 걸까?

import를 줄이기 위해 너무 한 package에  너무 많은 클래스에 넣게되면 오히려 응집이 깨지게 된다: 적절한 시기에 개념을 더 세분화 해야 한다. 카드에 대한 개념이 너무 커지고 있다면, 그 속에서 구체적인 개념화해야한다.

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216811&authkey=%21ABChfWK0_twIVT0&width=673&height=535)

이 코드에서는 카드 소유에 대한 새로운 개념을 추가하여 새로운 package를 구성하였다. 이 경우 card 패키지가 import되었으나, 다른 결의 import문이 아니라 카드 하위에 개념이 생기는 것으로 인식할 수 있다.

이렇게 개념을 구체화하고 응집하며 import를 살펴 봄으로써 더 나은 패키지를 만들어나갈 수 있다.

### 2. Layer

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216812&authkey=%21AMviLCCT5H5fWug&width=729&height=454)

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216813&authkey=%21AFEl9kiId17A0Yc&width=812&height=463)

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216814&authkey=%21ACmzyeJNABHL1Fg&width=734&height=471)

TOSS 서버 개발자의 표준 Layer 구조

```kotlin
package com.tosspayments.payments.core.controller

import com.tosspayments.payments.core.domain.mobile.MobileService
import org.springframework.web.bind.annotation.PostMapping
import org.springframework.web.bind.annotation.RequestBody
import org.springframework.web.bind.annotation.RestController

@RestController
class MobilePaymentController(
	val mobileService: MobileService
) {
	@PostMapping( “/payments/mobile”)
	fun addMobilePayment(@RequestBody request: MobilePaymentHttpRequest): MobilePaymentHttpResponse {
		return mobileService.addPayment(request)
	}
}
```

Presentation Layer에서 Business Layer로 객체를 그대로 전달한다. MobilePaymentHttpRequest는 import문에 선언되지 않았다: 같은 package에 존재한다. 이 코드 만으로는 문제를 발견할 수 없다. Business Layer를 살펴보자.

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216815&authkey=%21ABMfupAquhbwg9c&width=1100&height=410)

Layer는 역류하지 말아야 한다는 규칙을 위반한다: Business Layer가 Presentation Layer 접근하고 있다. 이를 다음과 같이 변경할 수 있다.

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216816&authkey=%21ALJ2nSJaPhexeqc&width=1622&height=658)

Presentation Layer 에서 Business Layer로 전달할 때 개념화된 클래스로 변환하여 전달하게 하였다. Business Layer는 어떻게 변할까?

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216817&authkey=%21AENAFYCGopO8438&width=1598&height=533)

Layer의 역류가 일어나지 않는다. MobileService가 다른 어떤 개념에 의존적인지도 쉽게 알 수 있다. 

앞선 Layer의 혼선은 장기적으로 코드의 복잡도를 높이고, 확장에 발목을 잡거나 문제를 만들어낸다. 이러한 Layer간의 잠재적 문제를 imort문을 통해서 점검할 수 있다. import도 코드의 일부이며, 지속적으로 검토해야 하는 것것임을 다시 한번 알 수 있다.
### 3. Module

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216807&authkey=%21AKSWhBXfKTWSb3k&width=947&height=533)

TOSS 서버 개발자의의 표준 module 구조. 화살표 방향은 Gradle 구성에서 의존하는 방향을 의미한다. Runtime시에는 runnable한 module을 중심으로 의존성이 주입되서 실행된다. 회색 원은 외부 기능을 확장할 때 만들게되는 새로운 module을 의미한다.

이러한 구조의 장점으로는,
1. 기술을 격리할 수 있다.
2. 각 module별 테스트가 가능하다.
3. 역할과 경계를 뚜렷하게 나눌 수 있게 된다.

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216819&authkey=%21AMyIXa0n7iOvWSA&width=614&height=396)

Business Logic 안에 특정 라이브러리에 대한 의존이 들어간 경우를 살펴보자. 이 경우, 라이브러리의 업데이트나 내부 요구 사항의 변경이 발생하면 business logic을 수정해야 하는 일이 발생한다. 

Module을 분리하고 격리하여 의존성 침투를 막을 수 있다. TOSS의 모듈 구조처럼 라이브러리에 대한 부분을 격리하여 사용하면, business logic에서 특정 라이브러리에 대한 import를 할 수 없게 된다(다음 그림 참조).

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216823&authkey=%21AFYzIJOFKmhWzuY&width=612&height=401)

이 경우, business logic은 더 뚜렷해지고, 기술에 대한 격리의 이점을 통해 추후 라이브러리가 교체될 때에 business logic에 영향 없이 작업할 수 있게된다.

두 번째 예제 코드를 살펴보자.

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216822&authkey=%21AKoLEH-mUPk91tk&width=745&height=456)

이번에는 Layer를 역류하는 import문이 발생하였다. 유추해보자면, Store 클래스에도 같은 import문이 있을거라고 생각할 수 있다. 상황에 따라 다르지만, presentation layer를 모듈화 시켜 격리할 수 있다. 그리고 Spring도 격리하는 설계를 적용할 수 있다. 잠시 TOSS의 서버 구조를 살펴보자.

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216820&authkey=%21ANQ2wGJWwLp6sEs&width=573&height=502)

TOSS의 서버 구조에서 Payments API는 domain을 가진 형태로 존재한다. 또한 API 서빙을 하기 위해 Spring에 대한 의존성을 가지고 있다. Domain 코드가 잘 정리되어 있고, 외부 의존성 정리를 잘 해두었다면 module구조를 어렵지 않게 바꿀 수 있다. 변경해 볼 module 구조를 보면,

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216821&authkey=%21AIN69mW3enXrgGU&width=572&height=495)

Domain 모듈이 Payments API에서 분리되었고, Spring가 멀어져 의존성이 격리되었다. 그리고 선택에 따라 Storage 모듈을 은닉할 수 있다: Strorage 모듈을 런타임으로 의존하게 하고, storage 모듈의 domain 모듈의 명세에 따라 구현체의 역할만 하는 구조로 수정할 수 있다. 이 경우, Payments API는 domain 모듈만 알고 있고, storage 모듈이 어떻게 구성되어 있는지, 어떤 구현체를 쓰는지 알 수 없게 된다: HTTP 응답으 JPA entity를 사용한다거나 도메인이 HTTP 스펙을 알고있는 문제를 만들지 않을 수 있다. 결과적으로, 더 격리된 환경에 비즈니스를 확장할 수 있게 된다.

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216824&authkey=%21AAzC137kDkHsuQ0&width=684&height=426)

Business layer에서 presentation layer에 존재하는 클래스에  import 할 수 없게 된다(Spring에 대한 클래스도 마찬가지). 

## 결론

모든 코드는 각자 신호를 보내고 있고, 프로그래머는 그 속에서 트레이드 오프하면서 다음 수준의 설계를 고민해야 한다. 트레이드 오프로 인해 완벽한 코드는 존재할 수 없고, 소프트웨어는 하나의 생명처럼 계속 진화해야 하는 것이다. 그러므로 현실에서 선택을 하고, 그 선택을 설계와 코드에 녹여내야 한다. 지속 가능한 소프트웨어는 지속 가능한 코드 기반에서 나오는 것이다. 

그렇다면 토스 페이먼츠는 왜 코드를 중요시하는가? 두 가지 목적 있다.

1. 통제: 코드에 관심을 두어 소프트웨어를 통제하고자 한다. 앞서 살펴 본 package 응집을 관리하고 layer 규칙을 준수하며, 적절한 module화로 기술을 격리하는 것 모두, 다양한 변화에 대응하여 코드를 통제할 수 있도록 하기 위함이다.
2. 제어: 1의 통제를 기반으로, 소프트웨어를 자유 자재로 제어하고자 함이다. 오랜기간 운영되어야 하는 서비스의 특성상, 코드를 통제하고 제어하는 것으로 원활하게 서비스를 제공할 수 있기 때문이다. 수명이 긴 코드이므로, 지속적인 관리와 성장이 필요하다.

결국 위 두 목적은 효과적이고 안정적으로 지속 성장하는 소프트웨어를 만들어내어 단순히 사업을 유지하는 것이 아니라, 산업을 이끌어가는 시스템을 구축하고자 함이다. 비즈니스와 기술의 변화에 유연하게 대응하고, 진화할 수 있는 소프트웨어를 구축하여 결제 산업을 혁신시키는 것이 궁극적인 목표이기 때문이다.