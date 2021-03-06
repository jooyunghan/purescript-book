# 소개

## 함수형 스타일의 JavaScript

이제 JavaScript에서도 함수형 프로그래밍을 자주 보게 되었다.

- [Underscore.js](http://underscorejs.org)같은 라이브러리는
`map`, `filter`, `reduce`와 같은 검증된 함수들을 제공하며, 개발자들은 이를 이용하여
합성(composition)을 통해 프로그램을 키워나갈 수 있다.

    ```javascript
    var sumOfPrimes =
        _.chain(_.range(1000))
         .filter(isPrime)
         .reduce(function(x, y) {
             return x + y;
         })
         .value();
    ```

- Node.js에서 흔히 볼 수 있는 비동기 프로그래밍 방식은 콜백을 정의하기 위해 일급 객체로서의 함수를 이용한다.

    ```javascript
    require('fs').readFile(sourceFile, function (error, data) {
      if (!error) {
        require('fs').writeFile(destFile, data, function (error) {
          if (!error) {
            console.log("File copied");
          }
        });
      }
    });
    ```

- [React](http://facebook.github.io/react/)나 [virtual-dom](https://github.com/Matt-Esch/virtual-dom) 모델은 애플리케이션의 상태를 순수 함수로 접근한다.

함수라는 단순한 형태의 추상화는 생산성을 크게 높여준다. 하지만 JavaScript로 함수형 프로그래밍을 하는데는
한계가 있다. JavaScript는 군더더기가 많고, 타입을 지원하지도 않으며, 더 강력한 추상화 도구가 부족하다.
JavaScript는 자유도가 높다보니 등식 추론(equational reasoning)을 하기도 매우 어렵다.

PureScript는 이러한 문제들을 극복하기 위한 프로그래밍 언어다. 가벼운 문법 구조는 표현력이 우수하면서도 깔끔하고 읽기 쉬운 코드를 작성할 수 있게 한다. 강력한 추상화를 지원하는 우수한 타입 시스템 덕분이다.
컴파일 결과로 생성되는 JavaScript 코드는 빠를 뿐 아니라 사람이 직접 읽고 이해할 수도 있다. 이 점은 JavaScript 혹은 JavaScript로 컴파일되는 다른 언어와 함께 작업하고자 할 때 중요한 특징이다.
무엇보다도, 순수 함수형 프로그래밍이라는 이론적 파워와 JavaScript의 즉각적이고 느슨한
프로그래밍 스타일 사이에 매우 실용적인 균형점을 찾은 것이 바로 PureScript라고 말하고 싶다.

## 타입과 타입 추론

정적 타입 언어와 동적 타입 언어에 관한 논쟁은 잘 정리되어 있다. PureScript는 **정적 타입** 언어이며,
올바른 프로그램이라면 그 동작에 대해 타입으로 명시되며 이를 컴파일러가 검증할 수 있다는 것을 의미한다.
거꾸로 보자면 타입을 정확하게 지정할 수 없는 프로그램이라면 올바르지 않다는 의미이며, 컴파일러가 이를 확인해준다. PureScript는 동적 타입 언어와 달리 **컴파일 시점**에만 타입이 존재하며 런타임에 따라다니지 않는다.

여러가지 측면에서 PureScript의 타입이 Java나 C#의 타입과 다르다는 점을 분명히 하자.
크게 봐서는 목적이 같지만 PureScript의 타입은 ML이나 Haskell 같은 언어에서 크게 영향을 받았다.
PureScript 타입은 표현력이 우수해서 개발자가 프로그램의 동작을 더 분명하게 드러낼 수 있다.
무엇보다도 PureScript의 타입 시스템은 **타입 추론**을 지원한다. 다른 언어들이
타입을 일일이 지정하도록 하는 것과 비교하자면 PureScript에서는 대부분의 경우에 생략이 가능하며,
이 때문에 타입 시스템이 걸림돌이 되기보다는 **도구**로 사용될 수 있다.
간단한 예로서 어떤 **수(number)**를 정의하는 다음 코드를 보자. 코드에 `Number`라는 타입이 전혀 나타나지 않는다.

```haskell
iAmANumber =
  let square x = x * x
  in square 42.0
```

조금 더 복잡한 예를 살펴보자. 타입을 전혀 지정하지 않았지만
타입 오류가 없는 올바른 코드이다. 아래 코드는 컴파일러조차도 아직 어떤 타입이 사용될지 모르는 상황이다.

```haskell
iterate f 0 x = x
iterate f n x = iterate f (n - 1) (f x)
```

여기서 `x`의 타입은 결정되지 않았다. 하지만 컴파일러는 `x`의 타입이 무엇이든 상관없이 
`iterate`가 타입 시스템의 규칙을 따르고 있음을 확인할 수 있다.

이 책에서 나는, 정적 타입이 프로그램의 정확성에 대한 확신을 주는 차원을 넘어 그 자체로 개발을 도와주는 역할을 한다는 점을 여러분에게 확인시켜주고 싶다. 가장 단순한 형태의 추상화에만 의존하는 JavaScript로는 커다란 프로그램을 리팩터링하기가 매우 어려울 수 밖에 없지만, 타입을 확인할 수 있는 강력한 타입 시스템의 도움을 받는다면 리팩터링도 즐거운 경험이 될 수 있다.

타입 시스템이 제공하는 안전망 위에서 우리는 더 고급진 추상화를 활용할 수 있다. 실제로 PureScript가 제공하는
강력한 추상화 도구가 바로 Haskell 언어를 통해 유명세를 얻은 타입 클래스(type class)이며, 이는 근본적으로 타입에 기반한 것이다.

## 폴리글랏 웹 프로그래밍

함수형 프로그래밍은 나름 성공한 분야가 있다. 데이터 분석, 파싱, 컴파일러 구현, 제네릭 프로그래밍, 병렬화 등등.

PureScript 같은 함수형 언어로 처음부터 끝까지 개발하는 것도 가능하다.
또한 PureScript는 기존 JavaScript 코드를 불러와 임포트된 값이나 함수에 타입을 부여함으로써
보통의 PureScript 코드에서 그 함수들을 사용할 수 있는 방법을 제공한다.
기존 코드와 붙여 개발하는 방법은 이 책 뒤에서 다룰 것이다.

하지만 PureScript의 강점 중 하나는 바로 JavaScript를 타겟으로 하는 다른 언어들과 상호 연동 가능하다는 점이다. 여러분이 개발하는 애플리케이션의 아주 작은 부분만 떼어내어 PureScript로 개발하고, 나머지 JavaScript 부분을 다른 언어로 개발하는 방법도 있다.

몇 가지 케이스를 살펴보자.

- 핵심 로직은 PureScript로 작성하고 UI는 JavaScript로 작성
- JavaScript 혹은 JavaScript로 컴파일되는 다른 언어로 애플리케이션을 작성하되 테스트는 PureScript로 작성
- 기존 애플리케이션의 UI 테스트를 자동화하는데 PureScript 활용

이 책에서는 PureScript로 작은 문제들을 푸는데 집중할 것이다.
그 풀이들이 더 큰 애플리케이션을 개발하는데 쓰일 수 있다. 물론 JavaScript에서 PureScript를 호출하거나 혹은 그 반대로 호출하는 방법도 살펴볼 것이다.

## 준비

이 책을 따라가는데 필요한 소프트웨어는 몇 개 되지 않는다. 1장에서 개발 환경을 설정하는 방법을 알려줄 것이다.
필요한 도구들은 대부분의 OS를 지원한다.

PureScript 컴파일러는 바이너리로 배포되는 것을 다운로드하거나 소스 코드를 직접 빌드하여 얻을 수 있다. 직접 빌드하려면 GHC Haskell 컴파일러가 필요하다. 이 방법은 다음 장에서 살펴볼 것이다.

이 책의 코드는 PureScript 컴파일러 `0.11.*` 버전을 대상으로 한다.

## 이 책은 누구를 위한 것인가

나는 JavaScript 기초는 이미 알고 있는 독자를 가정했다.
JavaScript 생태계에서 흔히 볼 수 있는 npm이나 Gulp같은 도구들을 알고 있다면
여러분이 필요에 맞게 빌드 설정을 손보고자 할 때 도움이 되겠지만 꼭 필요하지는 않다.

함수형 프로그래밍에 대해서는 어떤 사전 지식도 필요하지 않다. 하지만 이미 알고 있다고 문제 될 것은 없다.
새로운 개념들은 실제적인 예제와 더불어 소개할 것이고 여러분은
함수형 프로그래밍 개념들에 대해 감을 잡을 수 있을 것이다.

Haskell을 잘 알고 있는 독자들이라면 이 책이 소개하는 개념이나 문법을 더 쉽게 알아볼 수 있을 것이다.
PureScript가 Haskell의 영향을 많이 받았기 때문이다. 하지만 그런 독자들이라면 PureScript와 Haskell이
어떻게 다른지 이해해야 한다. 여기서 소개하는 많은 개념들이 Haskell에서도 비슷하게 적용될 수는 있겠지만
서로 다른 언어인 만큼 모든 개념들이 완벽히 대응되지는 않는다.

## 이 책을 어떻게 읽을까

각 장들을 읽는 데 필요한 내용들은 함께 담으려 노력했다. 하지만 함수형 프로그래밍 경험이 전무한 초보자라면 이 책의 순서를 차근차근 따라갈 것을 추천한다. 이 책의 앞 부분 장들이 다루는 내용은 책의 후반부를 이해하는데 기초 역할을 한다. 함수형 프로그래밍에 익숙한 독자라면(ML이나 Haskell같은 강력한 타입 언어를 알고 있다면) 굳이 앞 부분 장들을 읽지 않더라도 뒷 장들의 코드를 읽고 이해하는데 부족함이 없을 것이다.

장마다 하나씩 실제적인 예제에 집중한다. 예제들은 새로운 아이디어가 왜 등장하게 되었는지 이해하는데 도움을 줄 것이다.
각 장의 코드는 [GitHub 저장소](https://github.com/paf31/purescript-book)에서 얻을 수 있다.
본문의 코드 조각들은 저장소의 소스 코드에서 가져온 것이다. 이 책을 읽으면서 저장소의 코드도 살펴보는 것이 도움이 될 것이다. 좀더 긴 섹션에서는 인터랙티브 모드에서 직접 실행해 볼 수 있는 짧은 코드들도 제공한다.

예제 코드에는 모노스페이스 폰트를 사용했다.

```haskell
module Example where

import Control.Monad.Eff.Console (log)

main = log "Hello, World!"
```

명령줄에 입력할 내용 앞에는 달러 표시($)를 붙여 놓았다.

```text
$ pulp build
```

보통 이런 명령들은 Linux나 macOS 사용자들에게 맞춰져 있으므로 Windows 사용자라면 경로구분자나 쉘 명령을 Windows에 맞게 고쳐 입력해야 할 것이다.

PSCi 인터랙티브 모드에서 입력해야 하는 명령 앞에서는 꺽은 괄호(>)를 붙여 놓았다.

```text
> 1 + 2
3
```

각 장에는 연습 문제가 있다. 문제마다 난이도를 표시했다. 각 장의 내용을 완전히 이해하기 위해서는
연습 문제를 꼭 풀어보길 바란다.

이 책의 목적은 PureScript를 처음 시작하는 이들에게 적절한 입문서 역할을 하는 것이다. 문제와 해답 위주의 책이 아니다. 초보자들에게 이 책은 즐거운 도전이 될 것이다. 책을 읽고, 연습 문제를 풀어 보고, 무엇보다 여러분 손으로 직접 코드를 작성해 보길 바란다. 얻는 게 많을 것이다.

## 도움 구하기

어찌하면 좋을지 모를 막막한 상황에 처한다면 다음 자료나 사이트를 참고하기 바란다.

- PureScript IRC 채널: 여러분이 마주친 문제를 물어보기에 가장 좋은 곳이다. irc.freenode.net에서 #purescript 채널로 연결하면 된다.
- [PureScript 웹사이트](http://purescript.org)에서 학습 자료나 코드 샘플, 비디오 등 초보자를 위한 자료들을 찾을 수 있다.
- [PureScript 위키](http://wiki.purescript.org)에는 PureScript 개발자나 사용자들이 작성한 다양한 주제의 아티클들이 있다.
- [Try PureScript!](http://try.purescript.org)는 웹브라우저에서 직접 PureScript 코드를 컴파일 해 볼 수 있는 환경을 제공하는 사이트다. 몇가지 간단한 예제 코드도 볼 수 있다.
- [Pursuit](http://pursuit.purescript.org)는 PureScript의 타입과 함수를 검색할 수 있는 데이터베이스다.

예제를 통해 배우길 원한다면 `purescript`, `purescript-node`, `purescript-contrib`의 GitHub 조직을 찾아보라. PureScript 코드 예제를 많이 찾을 수 있다.

## 저자 소개

나는 PureScript 컴파일러를 개발했다. 로스앤젤레스에 살고 있으며, 어렸을 적 8비트 PC에서 BASIC을 한 것이 프로그래밍의 시작이었다. 지금은 다양한 언어들(Java, Scala, C#, F#, Haskell, PureScript)로 일하고 있다.

개발자 커리어를 시작한지 오래지 않아 함수형 프로그래밍, 그리고 수학과의 연결 고리 등에 매료되었다. Haskell를 통해 함수형 개념들을 배우길 즐겼다.

내가 PureScript 컴파일러를 개발하게 된 계기는 바로 JavaScript 개별 경험 때문이다.
Haskell 같은 언어에서 가져온 함수형 프로그래밍 기법들을 사용하면서도 그 기법들이 좀더 엄격하게 적용되는
환경을 원했다. 당시에는 Haskell의 시맨틱을 유지하면서 JavaScript로 컴파일하려는 다양한 시도들(Fay, Haste, GHCJS)이 있었다. 하지만 나는 다른 측면, 즉 Haskell의 타입 시스템이나 문법적 요소는 차용하면서
JavaScript의 시맨틱을 유지하는 접근법이 어떨까 하는 데 흥미가 있었다.

[블로그](http://blog.functorial.com)와 [트위터](http://twitter.com/paf31)를 하고 있다.

## 감사의 글

PureScript가 현재 수준에 이를 수 있도록 도와준 많은 컨트리뷰터들에게 고마움을 전하고 싶다.
모두의 노력이 담긴 컴파일러, 도구들, 라이브러리, 문서, 테스트가 없었다면 이 프로젝트는 실패하고 말았을 것이다.

PureScript 로고는 Gareth Hughes가 만든 것으로 [크리에이티브 커먼즈 저작자표시 4.0 라이센스](https://creativecommons.org/licenses/by/4.0/deed.ko)를 따른다.

마지막으로 이 책의 내용과 오류에 대해 피드백을 해 준 모두에게 감사드린다.