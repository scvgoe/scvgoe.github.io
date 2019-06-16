---
layout: post
title: '[번역] Principles of Modern Application Development'
subtitle: 모던 어플리케이션 개발 원칙
tags: [translate, web, theory]
image: /img/principles-modern-app-dev-triangle-1.png
---

# 들어가며

NGINX 공식 블로그에 업로드 된 원글 [Principles of Modern Application Development](https://www.nginx.com/blog/principles-of-modern-application-development/)을 저자의 허락을 구해 번역해보았습니다. 피드백은 언제나 환영입니다.

---
---
---

소프트웨어는 점점 유능해지고, 복잡해지고 있습니다. Marc Andreessen이 말한 것 처럼, 소프트웨어는 세계를 집어 삼키고 있습니다.

따라서, 어플리케이션을 개발하고 배포하는 방식은 지난 몇년간 크게 바뀌었습니다. 이러한 변화는 구조적으로 이루어져 팀 구성, 설계, 구현, 어플리케이션을 배포 시에 매우 유용한 몇가지 원칙들을 이끌어 냈습니다.

이 원칙들은 다음과 같이 요약할 수 있습니다. 작게 유지해라, 개발자를 위해 설계해라, 네트워크화해라. 간단한 이 세가지 원칙으로, 견고하고 복잡하지만 빠르고 안전하게 배포될 수 있고, 나아가 쉽게 확장할 수 있는 어플리케이션을 설계할 수 있습니다.

![Key Principles of Modern Application Development](/img/principles-modern-app-dev-triangle-1.png)

세가지의 이 원칙들이 우리의 목표(사용자에게 빠르게 배포되며, 견고하지만 유지하기 쉬운 어플리케이션)에 기여하는지 알아봅시다. 각 원칙들을 정반대의 원칙과 대조하는 방식으로 각각이 의미하는 바를 설명하도록 하겠습니다.

이 블로그 포스트가 모던 어플리케이션을 구축하는데에 몇가지 원칙을 정하는데에 도움이 되기를 바랍니다.

원칙들을 구현함으로써, 최근 소프트웨어 개발 트렌드의 장점들을 취할 수도 있게 될 것입니다. 예를 들어, [DevOps](https://www.nginx.com/blog/tag/devops/) 접근 방식을 비롯하여, [도커](https://www.nginx.com/blog/tag/docker/)와 같은 컨테이너, [쿠버네티스](https://www.nginx.com/blog/tag/kubernetes/)와 같은 컨테이너 오케스트레이션 프레임워크, 마이크로서비스, 마이크로서비스를 위한 [service mesh architecture](https://www.nginx.com/blog/what-is-a-service-mesh/) 등의 트렌드 말이죠.

# What is a Modern App?

모던 어플리케이션, 모던 스택, `'모던'`이 정확히 무슨 뜻인가요? 우리 모두 대충은 알고 있겠지만, 단어의 정확한 정의를 짚고 넘어가도록 하겠습니다.

모던 어플리케이션이란 여러 종류의 클라이언트를 지원하는 것입니다. 클라이언트가 리액트 자바스크립트 라이브러리이든, 안드로이드나 ios 모바일 앱이든 상관없이 말이죠. 모던 어플리케이션은 미리 정의되지 않은 수 많은 클라이언트들에게 데이터와 서비스를 제공할 수 있어야 합니다.

모던 어플리케이션은 데이터와 서비스에 대한 접근을 위해 API를 제공합니다. API는 클라이언트의 종류와 상관없이 일관됩니다. API는 HTTP(S)위에서 동작하며, GUI, CLI를 통해서 모든 기능을 이용할 수 있습니다.

데이터는 어느 플랫폼에서나 쉽게 이용 할 수 있도록 JSON과 같이 일반적인 형태를 띱니다. API는 object나 서비스를 명확하고 구조화된 형태로 매핑합니다. RESTful API나 GraphQL은 이러한 인터페이스를 위한 훌륭한 도구입니다.

모던 어플리케이션은 모던 스택 위에서 만들어지고, 모던 스택은 이러한 유형의 어플리케이션을 지원합니다. 모던 스택을 사용하면 개발자가 HTTP 인터페이스와 명확한 API endpoint를 사용하여 어플리케이션을 쉽게 만들 수 있습니다. 어플리케이션에서 JSON 데이터를 쉽게 사용하고 제공할 수 있도록 해줍니다. 다시 말해 [Twelve-Factor](https://www.nginx.com/blog/microservices-reference-architecture-nginx-twelve-factor-app/)를 쉽게 따를 수 있도록 도와줍니다.

인기있는 이러한 스택들은 Java, Python, Node, Ruby, PHP, Go 등에 기반합니다. [The NGINX Microservices Reference Architecture (MRA)](https://github.com/nginxinc/mra-ingenious)는 각각의 언어로 구현된 모던 스택의 예제들을 제공하고 있습니다.

우리가 마이크로서비스 기반의 어플리케이션 개발을 강제하고 있는 것은 아닙니다. 어떤 사람들은 진화할 필요가 있는 모노리스 환경에 있고, 어떤 사람들은 마이크로서비스로 확장되고 진화하고 있는 SOA 어플리케이션에 있을 수도 있습니다. 또 어떤 사람들은 serverless 어플리케이션으로 옮겨가고 있을 수도 있으며, 위의 모든 것들을 조합한 형태를 사용하고 있을 수도 있습니다. 여기서 이야기할 원칙들은 약간의 변형만으로도 각각의 아키텍쳐에 모두 적용될 수 있습니다.

# The Principles

지금까지 모던 어플리케이션과 모던 스택에 대해 이야기 했습니다. 이제부터는 모던 어플리케이션을 설계, 구현, 유지 할 수 있도록 도와주는 구조적, 개발적인 원칙들에 대해서 알아봅시다.

모던 개발에서의 핵심 원칙 중 하나는 _작게 유지하라_ 입니다. 우리는 매우 복잡하고 많은 어플리케이션을 가지고 잇습니다. 어플리케이션을 작고, 독립적인 컴포넌트로 만드는 것은 어플리케이션을 설계, 유지를 보다 쉽게 해줍니다. ("쉽게"가 아닌 "보다 쉽게"입니다.)

두번째 원칙은 개발자들이 CI/CI, 인프라 등에 대한 걱정없이 기능 개발에만 몰두 할 수 있도록 해주는 것입니다. 따라서 두번째 원칙은 _개발자 중심으로_ 입니다.

마지막으로 모든 어플리케이션은 _네트워킹_ 되어야 합니다. 지난 20년동안 네트워크는 빨라졌고, 어플리케이션은 복잡해졌습니다. 앞으로 우리는 더 서로 네트워킹된 모습으로 나아갈 것입니다. 앞서 말했듯이, 모던 어플리케이션은 다수의 서로 다른 클라이언트들과 네트워크를 통해 통신합니다. 이러한 네트워킹 사고를 적용하는 것은 앞선 두개의 원칙에도 큰 이점을 가져다줍니다.

이 세가지 원칙, _작게 유지하라_, _개발자 중심으로_, _연결_, 을 지킨다면, 어플리케이션을 설계, 구현할 때, 진화를 위한 발판을 마련할 수 있습니다.

이 세가지 원칙들을 상세히 알아봅시다.

## Principle 1: Small

인간의 뇌는 많은 정보를 소화하기에는 어려움이 있습니다. 심리학에서 인지 부하는 정보 처리를 위한 정보들을 유지하는데 들어가는 정신적 노력의 총량을 말합니다. 개발자의 인지부하를 줄이는 것은 중요한데, 그것은 곧 개발자가 어플리케이션 전체 구조를 유지하기 위해 노력할 필요가 없이, 당장 해결해야 하는 문제에만 집중할 수 있다는 뜻이기 때문입니다.

![Key Principles of Modern Application Development - Small](/img/principles-modern-app-dev-small.png)

개발자의 인지 부하를 줄이는 방법에는 다음의 몇가지가 있습니다.

개발팀의 인지 부하를 줄이기 위한 세가지 방법:

1. 새로운 기능을 구축할 때 고려해야 하는 timeframe을 줄여라 - timeframe이 작을 수록 인지 부하가 적습니다.
2. 작업해야 하는, 코드의 양을 줄여라 - 코드가 적다는 것은 적은 인지 부하를 의미합니다.
3. 어플리케이션에 변화를 추가하는 것의 과정을 간단히 해라 - 과정이 간단할수록 인지 부하가 적습니다.

### Short Development Timeframes
폭포수 모델이 개발 프로세스의 표준이던 과거에는, timeframe이 6개월에서 2년까지 이르렀습니다. 개발자들은 통상적으로 관련한 문서들, 예를 들면 product requirements document (PRD), system reference document (SRD), architecture plan 등을 모두 보고 그들의 인지 모델 안에 녹여내야 했습니다. 요구사항이 바뀌면, 팀의 속도를 높이면서 변경된 인지 모델을 유지하는 것은 꽤 부담이 되는 일이었습니다.

어플리케이션 개발 프로세스에서 가장 큰 변화는 애자일 개발 프로세스의 도입이었습니다. 애자일 방법론의 큰 특징 하나는 반복적(iterative) 개발입니다. 이는 개발자가 부담하는 인지부하의 감소로 이어졌습니다.

개발팀이 매우 긴 시간에 걸쳐 한번에 어플리케이션 전체를 다루기보다는, 애자일 접근법은 빠르게 테스트되고 배포 될 수 있도록 더 작은 단위에 집중하게 했고, 이는 사용자로부터 유용한 피드백을 이끌어 냈습니다. 어플리케이션에 대한 인지부하는, 6달에서 2년에 걸친 timeframe과 많은 양의 상세 명세에서, 거대한 어플리케이션의 나머지 부분은 신경쓰지 않은 채 부분적인 2주짜리 기능 단위의 추가 변경으로 변했습니다.

거대한 어플리케이션에 집중하는 것에서, 2주 스프린트에 완성될 수 있는 기능에 집중하는 것(아무리 많아도 다음번 2주 스프린트를 염두해 두는)으로의 변화는 거대했습니다. 개발자들을 더 생산적이게 했고, 인지부하를 줄여주었습니다. 어플리케이션이 원래 계획에서 얼마든지 변할 수 있다고 여겨지는 애자일 방법론에서는 최종 결과물이 경우에 따라서 모호할 수도 있습니다. 오직 매 스프린트 끝에 나온 구체적인 결과물들은 완전히 명확할 뿐입니다.

### Small Codebases
개발자의 인지부하를 줄이는 다음 단계는 codebase의 크기를 줄이는 것입니다. 모던 어플리케이션은 대부분 거대하고 견고합니다. 엔터프라이즈급의 어플리케이션은 수천개의 파일들과 수천줄의 코드를 가질 수 있습니다. 코드와 파일들의 상호관계, 의존성은 파일 구조에 따라 명확하거나 그렇지 않을 수도 있습니다. 코드의 실행과정을 추적하는 것은 코드가 의존, 사용하고 있는 다른 라이브러리들을 디버깅 툴이 얼마나 잘 구분해주는지에 따라 어려울 수 있습니다.

어플리케이션에 대해서 멘탈 모델을 구축하기에는 많은 시간이 걸리고, 개발자에게 인지 부하를 부담합니다. codebase가 크고, 기능 단위 사이의 상호작용이 명확하게 정의되지 않고, 기능 단위 사이의 경계가 명확하지 않아 관심사의 분리가 이뤄지지 않은 모노리스 어플리케이션에서는 더 심합니다.

개발자의 인지부하를 줄이는 효과적인 방법 중 하나는 마이크로서비스를 활용하는 것입니다. 마이크로서비스에서는 개별의 서비스들이 하나의 기능에만 집중한다는 장점이 있습니다. 또 서비스들의 역할이 잘 정의되어 있고, 이해하기 쉽습니다. 서비스 간의 경계가 명확하고 기억하기 쉬우며, 서비스들간의 소통은 API call을 통해 이뤄집니다. 하나의 서비스 내부에서 이뤄지는 작업들은 외부의 다른 서비스로 새나갈 일도 없습니다.

다른 서비스와의 상호작용은 보통 REST와 같이 명확한 API call을 통해 제한적으로 이뤄집니다. 이는 곧 개발자의 인지부하가 상당히 줄어든다는 것을 의미합니다. 트랜잭션과 같은 서비스들간의 상호작용이 다수의 서비스에 걸쳐 어떻게 이뤄지는 지를 이해하는 것은 어려운 일입니다. 결국 마이크로서비스의 사용은 전체 코드의 양을 줄여주고, 명확한 서비스 경계를 갖게 하고, 서비스들간의 명확한 관계 설정을 제공함으로써 개발자의 인지부하를 크게 줄여줍니다.

### Small Incremental Changes
이번 원칙의 마지막 요소는 변화의 관리입니다. "이건 똥이야, 전체 코드를 다시 짜야해"라고 말하는 것은 개발자들에게 꽤나 매혹적입니다. 종종, 그게 맞을 때도 있습니다만, 아닐 수도 있습니다. 이는 커다란 모델을 변경하고 곧 커다란 인지부하의 문제점을 야기시킵니다. 차라리 개발자들이 스프린트 내에 만들고, 전달할 수 있는 변화에만 집중하게 하는 것이 좋습니다. 원래 상상하던 변화랑 닮았지만, 사용자의 요구에 맞춰 테스트되고 수정됩니다.

많은 양의 코드를 재작성할 때, 다른 시스템과의 의존성으로 인해 기능을 제공할 수 없을 수도 있습니다. 변화하기 위해서 기능을 숨겨도 괜찮습니다. 이는 production에 배포를 하지만, 환경변수나 다른 configuration mechanism의 방식을 통해 접근할 수 없게 만드는 것을 의미합니다. 이 코드가 production 수준의 퀄리티이고, 모든 quality process를 통과했다면, 숨겨진 채로 배포되어도 괜찮습니다. 그러나 이런 전략은 기능이 결국에 활성화 되는 경우에만 동작합니다. 그렇지 않다면, 코드에서 개발자가 작업을 수행하기 위해 견뎌야하는 인지 부하를 추가합니다. 변화를 관리하고 수정을 점진적으로 유지하면 개발자에게 미치는 인지부하가 ​​합리적인 수준으로 유지될 수 있도록 합니다.

엔지니어들은 간단한 기능을 구현할 때에도 수많은 복잡성을 다뤄야 합니다. 엔지니어 리드로써, 외부의 인지부하를 줄이는 것은 팀이 기능의 중요한 요소에 집중할 수 있도록 도와줍니다. 엔지니어 매니저로써 할 수 있는 다음의 세가지는 팀에 도움이 됩니다.

1. 애자일 개발 프로세스를 사용하여 팀이 기능을 제공하기 위해 집중해야하는 시간을 제한하십시오.
2. 어플리케이션을 일련의 마이크로서비스로 구현하면 기능의 범위가 제한되고 구현 중에 인지 부하를 낮춰줍니다.
3. 점진적이고 작은 단위로 변경하도록 합니다. 기능을 숨김으로써 변경사항을 추가한 직후 노출되지 않더라도 변경사항을 구현할 수 있습니다.

`small`의 원칙으로 개발 프로세스에 접근하면 팀은 더 행복해지고 필요한 기능을 구현하는 데 더 집중할 것이며 더 높은 품질의 코드를 더 빨리 제공 할 수 있습니다. 이것은 복잡한 일이 없다는 것을 말하는 것이 아닙니다. 실제로, 여러 서비스를 수정해야하는 기능을 구현하는 것이 실제로는 `모노리스`에서보다 더 복잡 할 수 있습니다. 그러나 `small`의 원칙을 준수하면 전반적으로 이점을 누릴 수 있습니다.

## Principle 2: Developer-Oriented
빠른 개발을 저해하는 가장 큰 병목은 아키텍처나 개발 프로세스가 아니라 엔지니어가 비즈니스 로직에 집중하는 데 소요되는 시간입니다. 복잡하고 헤아리기 어려운 코드 기반, 과도한 툴링, 사회적 혼란은 모두 엔지니어링 팀의 생산성을 저해하는 요소입니다. 개발 프로세스를 개발자 지향적으로 만들 수 있습니다. 즉, 개발자의 생산성을 저해하는 요소들로부터 자유롭게 만들어 개발자가 당면한 작업에 보다 쉽게 ​​집중할 수 있습니다.

![Key Principles of Modern Application Development - oriented](/img/principles-modern-app-dev-oriented.png)

팀이 최고의 성과를 내기 위해서는 어플리케이션 생태계가 다음에 중점을 두는 것이 중요합니다.

* 작업하기 쉬운 시스템 및 프로세스
* 이해하기 쉬운 아키텍쳐와 코드
* 툴링 관리를 위한 DevOps

### Developer Environment
이러한 원칙을 구현하면 버그를 수정하고, 새로운 기능을 만들고, 생산성이 떨어지지 않으면서도 한 기능에서 다른 기능으로 쉽게 이동할 수 있는 생산적인 팀을 꾸릴 수 있습니다.

쉽게 일하는 개발 환경의 예시:

1. 개발자가 Github repo를 clone 한다.
2. 개발자가 Makefile에 정의된 명령어 몇개를 입력한다.
3. test를 실행한다.
4. 어플리케이션이 뜨고, 접근 가능해진다.
5. 실행중인 어플리케이션에서 코드 변경분을 확인할 수 있습니다.

반대로 데이터베이스, 지원 서비스, 인프라 구성 요소, 어플리케이션 엔진과 같은 환경설정을 하는데에 많은 노력을 필요로 하는 개발환경은 생산성을 저해하는 커다란 요소입니다.

### Architecture and Code
시스템이 실행되면 어플리케이션 코드와의 인터페이스를 위한 표준 방법이 필요합니다. RESTful API에는 공식 표준이 없지만 일반적으로 쉽게 사용할 수있는 몇 가지 특성이 있습니다.

1. endpoint는 명사로 노출됩니다. 예를들어 이미지를 제공하는 endpoint에는 `/image` 라는 이름을 사용합니다.
2. `Create`, `read`, `update` and `delete` (CRUD) operation은 HTTP 동사를 사용합니다:
	1. 검색을 위해 `GET`
	2. 새로운 컴포넌트의 생성을 위해 `POST`
	3. 컴포넌트의 추가나 업데이트를 위해 `PUT`
	4. 컴포넌트의 하위 요소의 업데이트를 위해 `PATCH`
	5. 컴포넌트의 삭제를 위해 `DELETE`
3. HTTP(S)를 API 접근하기 위한 프로토콜로 사용합니다.
4. 데이터는 JSON 포맷을 사용합니다.
5. API의 documentation을 위해 OpenAPI (Swagger로도 더 잘 알려진) UI 또는 이와 유사한 것을 사용합니다.

이는 RESTful API의 표준 요소이며 개발자가 기존 지식과 도구 (browser, curl 등)를 사용하여 시스템을 이해하고 조작 할 수 있음을 의미합니다. 이것을 RPC와 유사한 호출 방식을 사용하는 바이너리 프로토콜을 사용하는 것과 비교한다면: 개발자가 새로운 도구를 사용할 수도 있고, API가 명사와 동사가 섞여있을 수 있고, API 호출에 부작용이 발생할 수도 있으며, 반환 된 데이터는 바이너리, 인코딩 된, 압축 된, 암호화 된, 해독 불가한 형식일 수도 있습니다.

물론 RESTful API의 몇 가지 단점(특히 여러 객체에 대한 액세스 및 쿼리 기능)을 다루는 GraphQL과 같은 다른 표준이 등장하기도 하지만 REST로 API 투명성을 얻는 데 중점을 두는 것이 어플리케이션의 좋은 출발점입니다.

마이크로 서비스 어플리케이션은 일반적으로 다음과 같은 특성을 가지고 있습니다. 구성 요소 및 인프라가 컨테이너화되어 (Docker 이미지), 서비스 간 API는 RESTful이며 JSON으로 형식이 지정됩니다. 이러한 시스템은 적절한 도구를 통해 개발자가 작업하기가 쉽습니다.

쉽게 이해할 수 있다면 쉽게 작업할 수 있습니다. 그러나 이해하기 쉽다라는 것이 무엇을 의미하나요? 코드가 이해하기 어려울 수 있는 여러 가지 방법이 있습니다. 알고리즘이 복잡하거나, 구성 요소 간의 상호 작용이 복잡하거나, 논리적 모델이 다차원적일 수 있습니다. 이 모든 것은 본질적으로 코드가 복잡한 측면이므로 피할 수 없습니다. 보통, 개발자들은 이러한 유형의 복잡성에 집중하고자 합니다.

코드와 아키텍처를 이해하기 쉽게 만드는 키는 명확한 관심사의 분리와 관련이 있습니다. 사용자 관리 서비스는 사용자 정보 관리에 초점을 맞추어야합니다. 청구 관리 서비스는 청구에 초점을 맞추어야합니다. 그리고 청구 관리 서비스가 사용자 정보를 필요로 할 수도 있지만 사용자 관리 서비스가 코드에 포함되어서는 안됩니다. 코드와 아키텍처를 명확하게 만들기 위해서는 명확한 관심사의 분리가 중요합니다.

코드와 아키텍처를 이해하기 쉽게 만드는 또 다른 방법은 시스템 서비스와 상호 작용할 수 있는 단일 메커니즘 즉, 데이터와 함수에 액세스하기 위한 단일 인터페이스를 제공하는 것입니다. 데이터베이스 스키마의 변경이 다른 부분에 어떻게 영향을 미치는지 항상 명확하지는 않기 때문에, 서비스가 관리하는 데이터를 메소드 호출이나 데이터베이스 수정과 같은 다양한 방법으로 수정할 수 있는 경우, 변경 작업은 어려워집니다. 앱의 데이터 및 기능에 액세스하는 단일 인터페이스를 사용하면 이러한 모든 문제가 명확해집니다.

마이크로서비스로 애플리케이션을 구현하는 것은 명확한 상호 작용 모델과 명확한 관심사 분리를 제공합니다. 마이크로서비스는 정의에 따라 특정 작업에 초점을 맞추고 있습니다. 크기가 적절하면 관심사를 분리하는 좋은 수단이 됩니다. 또한 마이크로서비스는 데이터 액세스, 함수 (일반적으로 RESTful API) 사용을 위한 단일 인터페이스를 제공합니다. RESTful API 및 이를 구동하는 HTTP 동사에 익숙한 엔지니어는 이러한 마이크로 서비스를 사용하는 방법을 쉽게 이해하고 신속하게 생산성을 높일 수 있습니다.

엔지니어가 API, 데이터 구조, 메소드/함수, 데이터 액세스를 위한 object relation mapping (ORM), 데이터 계층에 이르기까지 애플리케이션 코드의 모든 계층에 액세스 할 수 있는 모노리스와 비교해봅시다. 코딩 컨벤션 및 데이터/함수 액세스를 엄격하게 관리하지 않으면 구성 요소가 어플리케이션의 다른 부분과 겹치거나 간섭하는 것이 매우 쉽습니다.

예를 들어, 앱의 한 부분이 데이터베이스의 테이블에 직접 데이터를 작성하고 이 부분에 로컬 최적화를 만드는 것이 일반적입니다. 이 코드는 종종 테이블을 관리하는 서비스의 주요 부분과 떨어져 있으므로 나중에 리팩터링 할 때 고려되지 않습니다. 마이크로 서비스에서는 이러한 종류의 실수가 훨씬 덜 발생하게됩니다.

### DevOps Tools
어플리케이션을 이해하기 쉽고 사용하기 쉽도록 만드는 것 외에도 엔지니어링 팀의 생산성을 향상시키는 방법 중 하나는 개발자가 자체 인프라에 소비하는 시간을 줄이는 것입니다. 개발환경이 갖춰지지 않은 개발자는 개발환경을 구축하는 데 시간을 할애해야합니다. 엔지니어가 시스템 가동, 스크립트 유지 관리, Makefile 작성, CI/CD 파이프 라인 유지 등에 대한 책임으로 엔지니어를 늪에 빠트리는 것은 DevOps의 영역에서 길을 잃게 하는 훌륭한 방법입니다.

더 바람직한 대안으로, 엔지니어링 팀과 함께 DevOps를 두는 것은 개발 인프라의 복잡한 측면을 관리하는 데 전념하는 사람이나 그룹이 있음을 의미합니다. 특히 마이크로서비스 어플리케이션과 같은 복잡한 시스템을 사용하는 경우 개발 환경 인프라 관리를 전담하는 사람을 두는 것이 중요합니다. DevOps는 다양한 요구 사항에 집중할 수 있습니다.

* Makefile이 모든 컴포넌트를 설치합니다.
* 모든 서비스가 적절히 빌드됩니다.
* 오케스트레이션 파일은 컨테이너를 적절한 순서로 로드합니다.
* 데이터 저장소가 초기화됩니다.
* 최신의 컴포넌트가 설치됩니다.
* 안전한 관습을 따라갑니다.
* 배포전에 코드가 테스트 됩니다.
* 코드가 빌드되고 배포를 위해 패키징됩니다.
* 실제 배포환경과 유사하게 개발환경을 구축합니다.
* 인프라 관리를 엔지니어에서 DevOps로 이동시킴으로써, 엔지니어들이 [야크 털 깎기](https://whatis.techtarget.com/definition/yak-shaving)가 아닌 기능을 개발하고 버그를 수정하는데에 집중하게 할 수 있습니다.

## Principle 3: Networked
어플리케이션 디자인은 시간이 지남에 따라 변하고 있습니다. 이전에는 어플리케이션이 호스트 된 시스템에서 사용되고 실행되었습니다. 메인 프레임/미니 컴퓨터 어플리케이션, 데스크톱 어플리케이션 및 심지어 Unix CLI 어플리케이션도 로컬 컨텍스트에서 실행되었습니다. 네트워크 인터페이스를 통해 이러한 시스템에 연결하는 기능은 점차 기능으로 자리 매김을 했지만, 종종 필요악으로 간주되며 일반적으로 "느린" 것으로 간주되었습니다.

![Key Principles of Modern Application Development - networked](/img/principles-modern-app-dev-networked.png)

그러나 어플리케이션이 커짐에 따라 개발과 배포가 점점 더 많이 분산되었습니다. 모든 종류의 네트워크의 속도와 안정성이 높아짐에 따라 어플리케이션은 점점 더 네트워크로 연결됩니다. 먼저 단일 서버에서 3계층 아키텍처로 전환한 다음 서비스 지향 아키텍처 (SOA)로 전환하고, 현재는 마이크로서비스로 전환됩니다. 그러나 네트워킹 어플리케이션에서 "느려지는 것들"에 대한 우려는 계속되고 있습니다.

심지어 네트워크가 로컬 컨텍스트의 통신보다 느리더라도 어플리케이션이 점점 더 네트워크로 연결되고 있습니다. 그 이유는 어플리케이션 아키텍처를 네트워킹하면 배포와 관리가 쉬워질 뿐만 아니라 탄력성이 향상되기 때문입니다.

이제, 네트워킹의 이점에 대해 알아보기 전에 어플리케이션 아키텍처를 네트워킹하는 것에 대한 우려를 다뤄봅시다.

네트워킹에 관한 가장 큰 우려 사항 중 하나는 속도에 대한 우려였습니다. 네트워크를 통해 구성 요소에 액세스하는 것은 메모리에서 동일한 구성 요소에 액세스하는 것보다 훨씬 느립니다. 그러나 최신 데이터 센터는 이전 세대의 네트워킹보다 무한히 빠른 가상 컴퓨터 간 고속 네트워킹을 제공합니다. 구글과 같은 회사들은 네트워킹 요청에 대한 대기 시간을 인 메모리 요청에 가깝도록 만들기 위해 열심히 노력하고 있습니다.

매우 느린 서드 파티 서비스에 액세스하는 것조차 훨씬 빨라졌으며 피어링 연결은 1Gbps보다 훨씬 빠릅니다. 전 세계 POP에서 호스팅되는 인기있는 서드 파티 서비스의 경우 보통 네트워크 홉 거리에 불과합니다. 또한 AWS와 같은 공용 클라우드에서 애플리케이션을 호스팅하는 경우 애플리케이션과 동일한 데이터 센터에서 실행되는 많은 다른 서비스의 이점을 누릴 수 있습니다. 속도는 예전처럼 더이상 문제가 아니며 쿼리 최적화 및 여러 수준의 캐싱과 같은 기술을 통해 최적화 될 수 있습니다.

네트워킹에 대한 또 다른 우려는 네트워크 프로토콜이 불투명하다는 것이었습니다. 과거에 일반적으로 사용된 네트워킹 프로토콜은 독점적이거나 어플리케이션 한정 또는 둘 다였기 때문에 디버깅 및 최적화가 어려웠습니다. HTTP의 편재성과 최신 버전에 추가된 강력한 기능, 접근성으로 인해 HTTP 네트워킹은 매우 강력하지만 브라우저 또는 컬 명령을 통해서도 이용할 수 있습니다. 엔지니어는 커넥션, 데이터 전송, 헤더 수정, 데이터 라우팅, HTTP 연결 부하 분산 방법을 알고 있습니다. HTTP의 광범위한 배포로 일반인이 네트워킹에 액세스 할 수 있게 되었습니다. SSL/TLS 및 압축 기능을 사용하면 사용하기 쉬운 안전하고 이진 효율적인 프로토콜을 얻을 수 있습니다.

속도와 불투명의 관점에서 다루었으므로 이제 네트워크 아키텍처의 이점을 살펴봅시다. 이러한 이점들은 어플리케이션의 탄력성, 배포, 관리를 쉽게 만들어줍니다.

### More Resilient
아키텍처에 네트워킹을 깊숙히 도입함으로써, 특히 [Twelve-Factor-App for Microservices](https://www.nginx.com/blog/microservices-reference-architecture-nginx-twelve-factor-app/)에 설명된 원칙을 사용하여 설계하는 경우, 어플리케이션을 더욱 탄력적으로 만들 수 있습니다. 애플리케이션에 twelve‑factor principles를 적용하여 수평적으로 쉽게 확장할 수 있고 요청 부하를 쉽게 분산시킬 수 있습니다.

즉, 하나의 어플리케이션 컴포넌트의 실패로 전체 어플리케이션이 중단 될 염려없이 모든 어플리케이션 컴포넌트의 여러 인스턴스를 동시에 실행할 수 있습니다. NGINX와 같은 로드 밸런서를 사용하면 서비스를 모니터링하고 요청이 정상적인 인스턴스로 이동하는지 확인할 수 있습니다. 또한 실제로 문제를 일으키는 시스템의 병목을 기반으로 어플리케이션을 쉽게 확장 할 수 있습니다. 모노리스 어플리케이션처럼 모든 어플리케이션 구성 요소를 동시에 확장할 필요가 없습니다.

### Easier to Deploy
어플리케이션을 네트워킹하면 배포가 더욱 간단해집니다. 단일 서비스에 대한 테스트는 전체 모놀리스 어플리케이션에 대한 테스트보다 훨씬 작습니다 (또는 더 간단합니다). 서비스 테스트는 모노리스가 요구하는 전체 회귀 테스트 프로세스보다 단위 테스트 또는 기능 테스트와 훨씬 비슷합니다. 마이크로서비스를 수용한다면 어플리케이션 코드가 한 번 빌드되어 변경없이 CI/CD 파이프라인을 통과하고 프로덕션 환경에서 실행되는 불변 컨테이너에 패키지된다는 것을 의미합니다.

### Easier to Manage
어플리케이션을 네트워크로 연결하면 관리도 쉬워집니다. 실패하거나 다양한 방식으로 확장해야 하는 단일 모노리스에 비해 네트워크화 된 마이크로서비스 지향 어플리케이션은 관리하기가 더 쉽습니다. 이제 구성 요소 컴포넌트가 분리되어보다 쉽게 모니터링 할 수 있습니다. 컴포넌트 간의 상호 통신은 HTTP를 통해 수행되므로 모니터링, 활용 및 테스트가 용이합니다.

NGINX Controller 나 NGINX Amplify와 같은 모니터링 도구는 서비스에 대한 정량화 된 데이터와 그 사이에서 이동하는 요청로드 정보를 효과적으로 제공합니다. 또한 네트워크 어플리케이션을 쉽게 확장 할 수 있습니다. Kubernetes와 같은 도구를 사용하면 전체 어플리케이션 모노리스를 배포하지 않고 개별 서비스를 확장 할 수 있습니다.

네트워크 어플리케이션은 모노리스 어플리케이션에 비해 많은 장점을 제공하며 네트워크 어플리케이션에 대한 일부 우려는 최신 환경에서는 근거가 없는 것으로 입증되었습니다. 네트워크 어플리케이션은 적절한 디자인으로 처음부터 높은 가용성을 제공하기 때문에 복원력이 뛰어납니다. 일반적으로 단일 서비스를 배포 할 때 모든 프로세스를 거칠 필요가 없으므로 네트워크 어플리케이션을 배포하기가 쉽습니다. 마지막으로 네트워크 어플리케이션은 조절 및 모니터링이 더 쉽기 때문에 관리가 더 쉽습니다. 더 많은 트래픽을 처리하도록 애플리케이션을 확장하면 일반적으로 전체 애플리케이션이 아닌 개별 서비스를 확장하는 프로세스가 됩니다. 성능, 네트워크 최적화, 서비스 피어링, 특히 현대 데이터 센터의 하드웨어 등에 대한 우려는 완전히 제거되지는 않더라도 감소합니다.

# How NGINX Fits In

There are a number of tools that facilitate modern application development. One is containers, with deployment of Docker containers becoming [standard practice](https://www.nginx.com/blog/three-ways-nginx-scales-applications-running-in-docker-containers/) for much application development and deployment. Another is the cloud and cloud services, with public cloud providers like Amazon Web Services (AWS), Google Cloud Platform, and Microsoft Azure skyrocketing in popularity.

NGINX is another of these tools, and, like the others, it’s used in both development and deployment. NGINX, Docker, and public cloud have all grown together, with NGINX, for instance, being the most popular download on Docker Hub, and NGINX software powering more than [40% of deployments on AWS](https://www.sumologic.com/wp-content/uploads/Sumo-Logic_State-of-Modern-Apps_Nov2016_12022016.pdf). AWS directly supports a popular load‑balancing implementation that combines the AWS Network Load Balancer (NLB) and NGINX.

But how did NGINX become so popular, and how does it fit with our core principles of modern application development? We’ll tell that story here as best we can, though all NGINX users have their own reasons for adopting it.

NGINX Open Source first became available in 2003, with the commercial version, NGINX Plus, first released in 2013. The use of NGINX software has grown and grown, and NGINX is now used on most of the web’s most popular websites, including [nearly two‑thirds of the 10,000 busiest](https://w3techs.com/technologies/cross/web_server/ranking).

This period of growth parallels almost exactly the emergence of modern application development and its principles: small, developer‑oriented, and networked. Why has NGINX grown so fast during this period?

In this case, correlation is not causation – at least, not entirely. Instead, the underlying reason for the growth of modern application development, and the increased use of NGINX, is the same: the incredibly rapid growth of the Internet. Roughly 10% of US retail commerce is now conducted online, and online advertising affects the vast majority of purchases. Nearly all of the great business success stories of the last few decades have been Internet‑enabled, including the rise of several of the most valuable companies in the world, the FANG group – Facebook, Apple, Netflix, and Google (now the core of the Alphabet corporation). Amazon and the recent, rapid growth of Microsoft are additional Internet‑powered success stories.

NGINX benefits from the growth of the Internet because it’s incredibly useful for powering busy, fast‑growing sites that provide very rapid user response times. There are two use cases. In the first, NGINX replaces an Apache or Microsoft Internet Information Server (IIS) web server, leading to much greater [performance](https://www.nginx.com/blog/nginx-vs-apache-our-view/), capacity, and stability.

In the second use case, NGINX is placed as a reverse proxy server in front of one or more existing web servers (which might be Apache, Internet Information Server, NGINX itself, or nearly anything else). This way, the reverse proxy server handles Internet traffic – much more capably than most web servers – and the web server only has to handle application server and east‑west information transfer duties. As a reverse proxy server, NGINX also provides traffic management, load balancing, caching, security, and more – offloading even more duties from the application and other internal servers. The entire infrastructure now works better.

Both use cases are more attractive to busier, more successful websites than to smaller sites. That’s why it’s the busiest sites, such as [Netflix](https://www.nginx.com/blog/why-netflix-chose-nginx-as-the-heart-of-its-cdn/), that tend to use NGINX more, with [most of the world’s large websites](https://w3techs.com/technologies/cross/web_server/ranking) running NGINX.

There’s also an additional, complementary use case: the use of NGINX at the core of publicly available content distribution networks (CDNs), as shown by [this panel discussion](https://www.nginx.com/nginxconf/2017/schedule/#) from last year’s NGINX conference, as well as [internal CDNs](https://www.nginx.com/blog/learn-to-stop-worrying-build-cdn/) created for the private use of large websites. These largely NGINX‑powered CDNs have made an additional contribution to the performance, capacity, and stability of the entire Internet.

These uses of NGINX – as web server, as reverse proxy server, and at the heart of many CDNs – have contributed immeasurably to the growth of the Internet. Because of NGINX, the Internet, as used by people every day, is faster, stabler, more reliable, and more secure.

But NGINX has also grown, in part, directly because of its support for our core principles of application development:

* **Small**. NGINX works well with small, focused services, because NGINX itself is very small. It’s also very fast, hardly slowing down under heavy loads. This encourages the deployment of small application services, API gateways – some API management tools use NGINX at their core – and, of course, microservices. NGINX is widely used in different [microservices network architectures](https://www.nginx.com/blog/introducing-the-nginx-microservices-reference-architecture/), as an [Ingress controller for Kubernetes](https://www.nginx.com/products/nginx/kubernetes-ingress-controller/), and as a sidecar proxy in our own [Fabric Model](https://www.nginx.com/blog/microservices-reference-architecture-nginx-fabric-model/) and in [service mesh](https://www.nginx.com/blog/what-is-a-service-mesh/) models.
* **Developer‑oriented**. This is a huge reason for the growth of NGINX. Among other benefits, with NGINX, developers can manage load balancing directly, throughout the development and deployment of their apps. This applies whether the app is on‑premises, in the cloud, or both. Hardware load balancers – also called application delivery controllers (ADCs) – are typically managed by network personnel, who have to follow extensive, time‑consuming procedures before implementing a change. With NGINX, developers can make changes instantly, gaining flexibility and retaining control. This isn’t the only reason that NGINX is popular with developers – all of the other reasons for the growth of NGINX given here apply as well, as well as a fair amount of admiration for the solid work of the core NGINX development and support teams. But accessible, inexpensive load balancing capability may be the single largest reason.
* **Networked**. As applications have put more and more of their interservice traffic over the network, the way in which NGINX works to connect services has become a huge part of its popularity with developers. NGINX has always allowed a very high degree of control over HTTP, and has more recently led with support for SPDY, the predecessor to HTTP/2, and HTTP/2 itself. TCP and UDP support has also been added.
Doing all this, while bringing a small memory footprint, speed, security, and stability in all of the many use cases where it’s applied, has made NGINX a very large part of the growth of the Internet and a strong supporting force in the emergence of modern application development.

# Conclusion


앞에서 보았듯이 모던 어플리케이션을 만드는 원칙은 매우 간단합니다. 작게 유지하고, 개발자를 위해 디자인하고, 네트워크화하라 라고 요약 할 수 있습니다. 이 세 가지 원칙을 사용하면 빠르게 전달되고, 쉽게 확장되고, 안전한 견고하고 복잡한 어플리케이션을 설계 할 수 있습니다. NGINX 소프트웨어는 이러한 원칙을 구현하기 위해 널리 사용되는 도구입니다.

---

### 참고
[Principles of Modern Application Development](https://engineering.videoblocks.com/web-architecture-101-a3224e126947)
