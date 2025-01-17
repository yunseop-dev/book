---
title: 웹 페이지 다운로드
chapter: 1
prev: history
next: graphics
...

웹 브라우저는 URL로 식별된 정보를 표시합니다. 그리고 첫 번째 단계는 이 URL을 사용하여 인터넷의 어딘가에 있는 서버에 연결하고 정보를 다운로드하는 것입니다.

서버에 연결하기
================

인터넷 브라우징은 URL\index{URL}[^url]이라는 짧은 문자열에서 시작됩니다. 이것은 브라우저가 방문해야 할 특정 웹 페이지를 식별합니다.

[^url]: "URL"은 "uniform resource locator(통합 자원 식별자)"의 약자로, 웹 페이지(자원\index{web resource})를 식별하는 휴대 가능한(통합된) 방법이며, 또한 파일에 접근하는 방법(위치)을 설명합니다.

::: {.cmd .web-only .center html=True}
    python3 infra/annotate_code.py <<EOF
    [http][tl|Scheme]://[example.org][bl|Hostname][/index.html][tl|Path]
    EOF
:::

::: {.center .web-only}
그림 1: URL의 구문.
:::

::: {.print-only}
![그림 1: URL의 구문.](im/http-url.png)
:::

URL은 세 가지 주요 부분으로 구성됩니다(그림 1 참조): scheme\index{scheme}은 정보를 *어떻게* 가져올지를 설명합니다. 호스트 이름은 정보를 *어디서* 가져올지를 나타냅니다. 경로\index{path}는 어떤 정보를 가져올지를 설명합니다. 또한 URL에는 포트, 쿼리, 프래그먼트와 같은 선택적 부분도 있으며, 이는 이후에 살펴보겠습니다.

URL로부터 브라우저는 웹 페이지를 다운로드하는 과정을 시작할 수 있습니다. 브라우저는 먼저 로컬 운영 체제(OS)에 *호스트 이름*으로 설명된 *서버*와 연결을 요청합니다. OS는 *도메인 이름 시스템*(DNS) 서버와 통신하여 `example.org`와 같은 호스트 이름을 `93.184.216.34`와 같은 *목적지 IP 주소*로 변환합니다.[^ipv6] 그런 다음 OS는 라우팅 테이블이라는 것을 사용하여 해당 목적지 IP 주소와 통신하는 데 가장 적합한 하드웨어(예: 무선 또는 유선)를 결정한 후, 장치 드라이버를 사용하여 신호를 선이나 공중으로 전송합니다.[^skipped-steps] 이러한 신호는 일련의 *라우터*[^switch-ap]에 의해 수신 및 전송됩니다. 각 라우터는 메시지를 최종 목적지로 보낼 최적의 방향을 선택합니다.[^network-tracing] 메시지가 서버에 도달하면 연결이 생성됩니다. 요컨대, 브라우저는 OS에 "Hey, `example.org`와 연결해 줘"라고 말하고, OS는 이를 수행합니다.

[^dns]: [nslookup.io](https://nslookup.io)나 `dig` 명령과 같은 DNS 조회 도구를 사용하여 직접 이 변환을 수행할 수 있습니다.

[^ipv6]: 오늘날에는 두 가지 버전의 IP(인터넷 프로토콜)가 있습니다: IPv4와 IPv6. IPv6 주소는 훨씬 길며 일반적으로 16진수로 작성되지만, 여기에서는 차이가 중요하지 않습니다.

[^skipped-steps]: 여기에서는 단계를 생략했습니다. 유선 통신에서는 이더넷 프레임에 통신을 감싸야 하고, 무선 통신에서는 그보다 더 많은 작업이 필요합니다. 간략히 설명하기 위해 생략했습니다.

[^switch-ap]: 또는 스위치, 액세스 포인트일 수도 있습니다. 가능한 장치가 많지만, 최종적으로 라우터에 도달합니다.

[^network-tracing]: 라우터는 메시지가 어디에서 왔는지도 기록할 수 있으며, 이를 통해 응답을 다시 전달합니다.

많은 시스템에서 `telnet` 프로그램을 사용하여 이러한 유형의 연결을 설정할 수 있습니다. 예를 들어,^["80"은 아래에서 설명할 포트를 의미합니다.]

```
telnet example.org 80
```

::: {.web-only}
(참고: 회색 윤곽선이 보이면 해당 코드가 단지 예일 뿐이며, 브라우저의 실제 코드에 속하지 않음을 의미합니다.)
:::

::: {.print-only}
(참고: 검은색 프레임이 보이면 해당 코드가 단지 예일 뿐이며, 브라우저의 실제 코드에 속하지 않음을 의미합니다.)
:::

::: {.installation}
`telnet`은 기본적으로 비활성화되어 있는 경우가 많으므로 설치해야 할 수도 있습니다.
Windows에서는 제어판에서 [프로그램 및 기능 / Windows 기능 켜기 또는 끄기](https://www.lifewire.com/what-is-telnet-2626026)를 사용하여 활성화할 수 있으며, 재부팅이 필요합니다. 실행하면 화면이 지워지지만 그 외에는 정상적으로 작동합니다. macOS에서는 `telnet` 대신 `nc -v` 명령을 사용할 수 있습니다:

``` {.example}
nc -v example.org 80
```

출력 형식은 조금 다르지만 동일하게 작동합니다. 대부분의 Linux 시스템에서는 패키지 관리자를 통해 `telnet`이나 `nc`를 설치할 수 있으며, 보통 `telnet`과 `netcat`이라는 패키지에서 찾을 수 있습니다.
:::

이 명령을 실행하면 다음과 같은 출력이 표시됩니다:

``` {.output}
Trying 93.184.216.34...
Connected to example.org.
Escape character is '^]'.
```

이는 OS가 `example.org`라는 호스트 이름을 `93.184.216.34`로 변환했고, 성공적으로 연결되었음을 의미합니다.[^10] 이제 `example.org`와 통신할 수 있습니다.

[^10]: escape character에 대한 라인은 `telnet`의 잘 알려지지 않은 기능을 사용하는 방법에 대한 지침일 뿐입니다.

::: {.further}
URL 구문은 [RFC 3987](https://tools.ietf.org/html/rfc3986)에 정의되어 있으며, 이 명세의 첫 번째 저자는 팀 버너스 리입니다—전혀 놀랍지 않죠! 두 번째 저자인 Roy Fielding은 HTTP 설계에 중요한 공헌을 했으며, 그의 [박사 학위 논문][rest-thesis]에서 웹의 *표현 상태 전이*(REST) 아키텍처를 설명한 것으로도 잘 알려져 있습니다. 이 논문은 REST가 웹이 탈중앙화 방식으로 성장할 수 있게 한 방법을 설명합니다. 오늘날 많은 서비스들이 이러한 원칙을 따르는 "RESTful API"를 제공합니다. 하지만 [일부 혼란][what-is-rest]도 있는 것 같습니다.
:::

[rest-thesis]: https://ics.uci.edu/~fielding/pubs/dissertation/fielding_dissertation_2up.pdf
[what-is-rest]: https://twobithistory.org/2020/06/28/rest.html

정보 요청하기
======================

연결이 완료되면 브라우저는 *경로*(path)를 제공하여 서버에 정보를 요청합니다. 이 경로는 URL에서 호스트 이름 뒤에 오는 부분으로, `/index.html`과 같은 형태입니다. 요청의 구조는 그림 2에 나와 있습니다. 이를 `telnet`에 입력하여 직접 시도해 보세요.

::: {.cmd .web-only html=True}
    python3 infra/annotate_code.py <<EOF
    [GET][tl|Method] [/index.html][tr|Path] [HTTP/1.0][tl|HTTP Version]
    [Host][bl|Header]: [example.org][bl|Value]

    EOF
:::

::: {.center .web-only}
그림 2: 주석이 추가된 HTTP GET 요청.
:::

::: {.print-only}
![그림 2: 주석이 추가된 HTTP GET 요청.](im/http-get.png)
:::

여기서 `GET`\index{GET}이라는 단어는 브라우저가 정보를 받고 싶다는 것을 의미합니다.[^11] 그 다음에는 경로가 오고, 마지막으로 `HTTP/1.0`이라는 단어가 있습니다. 이는 브라우저가 HTTP의 버전 1.0을 사용하고 있음을 호스트에 알리는 것입니다. HTTP에는 여러 버전이 있습니다([0.9, 1.0, 1.1, 2.0, 3.0](https://medium.com/platform-engineer/evolution-of-http-69cfe6531ba0)). HTTP 1.1 표준은 keep-alive와 같은 다양한 유용한 기능을 추가하지만, 단순화를 위해 우리 브라우저에서는 이를 사용하지 않을 것입니다. 또한 HTTP 2.0 역시 구현하지 않는데, 이는 1.*x* 계열보다 훨씬 복잡하며, 우리 브라우저로는 실행할 수 없는 크고 복잡한 웹 애플리케이션을 대상으로 하기 때문입니다.

[HTTP]: https://developer.mozilla.org/en-US/docs/Web/HTTP

첫 번째 줄 이후에는 각 줄이 *헤더*(header)로 구성됩니다. 헤더는 이름(예: `Host`)과 값(예: `example.org`)을 가집니다. 서로 다른 헤더는 서로 다른 의미를 갖습니다. 예를 들어, `Host` 헤더는 서버에게 당신이 생각하는 호스트가 누구인지를 알려줍니다.[^13] 보낼 수 있는 다양한 헤더들이 있지만, 여기서는 `Host`만 다루도록 하겠습니다.

마지막으로, 헤더 다음에는 빈 줄이 하나 옵니다. 이 빈 줄은 헤더 입력이 끝났음을 호스트에 알립니다. 따라서 `telnet`에 요청의 두 줄을 입력한 후 두 번 Enter를 눌러 빈 줄을 추가하세요. 그러면 `example.org`로부터 응답을 받을 수 있습니다.

[^11]: 정보를 보내야 하는 경우 `POST`라고 작성할 수 있으며, 이 외에도 더 잘 알려지지 않은 옵션들이 있습니다.

[^13]: 동일한 IP 주소에 여러 호스트 이름이 연결되어 다수의 웹사이트(`example.com`과 `example.org` 등)를 호스팅하는 경우, `Host` 헤더는 어떤 웹사이트를 원하는지를 서버에 알려줍니다. 이러한 웹사이트들은 기본적으로 `Host` 헤더가 있어야 제대로 작동합니다. 단일 컴퓨터에서 여러 도메인을 호스팅하는 것은 매우 일반적입니다.

::: {.further}
HTTP/1.0은 [RFC 1945](https://tools.ietf.org/html/rfc1945)에 표준화되어 있으며, HTTP/1.1은 [RFC 2616](https://tools.ietf.org/html/rfc2616)에 표준화되어 있습니다.
HTTP는 단순히 이해하고 구현할 수 있도록 설계되어, 다양한 종류의 컴퓨터가 이를 쉽게 채택할 수 있게 합니다. HTTP를 `telnet`에 직접 입력할 수 있는 것도 우연이 아닙니다! HTTP가 "줄 기반 프로토콜"로 설계된 것도 마찬가지입니다. 이것은 이메일에 사용되는 간단한 메일 전송 프로토콜([SMTP][SMTP])과 유사하게 일반 텍스트와 줄바꿈을 사용합니다. 결국 이러한 패턴은 초기 컴퓨터가 줄 기반 텍스트 입력만 제공했던 데서 비롯되었습니다. 사실, 최초의 두 브라우저 중 하나는 [라인 모드 UI][line-mode]를 갖추고 있었습니다.
:::

[SMTP]: https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol  
[line-mode]: https://en.wikipedia.org/wiki/Line_Mode_Browser

서버의 응답
=====================

서버의 응답은 그림 3에 나와 있는 줄로 시작됩니다.

::: {.cmd .web-only html=True}
    python3 infra/annotate_code.py <<EOF
    [HTTP/1.0][tr|HTTP Version] [200][bl|Response Code] [OK][tl|Response Description]
    EOF
:::

::: {.center .web-only}
그림 3: 주석이 추가된 HTTP 응답의 첫 번째 줄.
:::

::: {.print-only}
![그림 3: 주석이 추가된 HTTP 응답의 첫 번째 줄.](im/http-status.png)
:::

이 줄은 호스트가 `HTTP/1.0`을 지원하며, 요청이 "OK"(숫자 코드 200으로 표현됨)라고 응답했음을 알려줍니다. 여러분은 `404 Not Found`에 익숙할 수도 있는데, 이는 또 다른 숫자 코드와 응답입니다. 그 외에도 `403 Forbidden`이나 `500 Server Error`와 같은 코드들이 있습니다. 이러한 상태 코드는 여러 개가 있으며 잘 정리된 체계를 따릅니다:^[`OK`와 같은 상태 텍스트는 사실 사람을 위한 정보일 뿐이며, 기계가 사용하는 것은 아닙니다.]

- 100번대: 정보성 메시지;
- 200번대: 요청이 성공했음을 의미;
- 300번대: 후속 조치 요청(주로 리디렉션);
- 400번대: 잘못된 요청을 보냈음을 의미;
- 500번대: 서버가 요청을 잘못 처리했음을 의미.

브라우저와 서버 중 잘못이 누구에게 있는지를 알려주는 두 가지 에러 코드 집합(400번대와 500번대)을 두었다는 점이 매우 탁월합니다.^[보다 정확히 말하면, 서버가 잘못이 누구에게 있는지 판단한 내용을 알려줍니다.] 다양한 코드 목록은 [Wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)에서 확인할 수 있으며, 새로운 코드가 추가되기도 합니다.

`200 OK` 줄 이후, 서버는 자체 헤더를 보냅니다. 제가 이 작업을 실행했을 때 받은 헤더는 다음과 같았습니다(하지만 여러분이 받는 헤더는 다를 수도 있습니다):

``` {.example}
Age: 545933
Cache-Control: max-age=604800
Content-Type: text/html; charset=UTF-8
Date: Mon, 25 Feb 2019 16:49:28 GMT
Etag: "1541025663+gzip+ident"
Expires: Mon, 04 Mar 2019 16:49:28 GMT
Last-Modified: Fri, 09 Aug 2013 23:54:35 GMT
Server: ECS (sec/96EC)
Vary: Accept-Encoding
X-Cache: HIT
Content-Length: 1270
Connection: close
```

여기에는 요청된 정보(`Content-Type`, `Content-Length`, `Last-Modified`)에 대한 내용과 서버(`Server`, `X-Cache`)에 대한 정보, 브라우저가 이 정보를 얼마나 오래 캐시해야 하는지(`Cache-Control`, `Expires`, `Etag`)에 대한 정보, 그리고 기타 다양한 정보들이 포함되어 있습니다. 지금은 이 정도로 넘어가겠습니다.

헤더 다음에는 빈 줄이 하나 오고, 이어서 다수의 [HTML]\index{HTML} 코드가 이어집니다. 이것은 서버 응답의 *본문*(body)이라 불리며, 브라우저는 이를 `Content-Type` 헤더를 통해 `text/html`임을 확인하고 HTML로 인식합니다. 이 HTML 코드가 웹 페이지 자체의 콘텐츠를 포함합니다.

[html]: https://developer.mozilla.org/en-US/docs/Web/HTML

HTTP 요청/응답 거래는 그림 4에 요약되어 있습니다. 이제 직접적인 통신에서 Python으로 전환해 보겠습니다.

::: {.center}
![그림 4: HTTP 요청과 응답 쌍을 통해 웹 브라우저는 웹 서버로부터 웹 페이지를 가져옵니다.](im/http-request-2.gif)
:::

::: {.further}
Wikipedia에는 [HTTP 헤더][headers]와 [응답 코드][codes]에 대한 훌륭한 목록이 있습니다. HTTP 응답 코드 중에는 거의 사용되지 않는 코드들도 있습니다. 예를 들어, [402 "Payment Required"](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/402)는 디지털 현금 또는 (소액) 결제 시스템을 위해 사용하려고 했던 코드입니다. 전자 상거래는 응답 코드 402 없이도 잘 작동하고 있지만, [마이크로페이먼트][micropayments]는 아직 큰 주목을 받지 못했습니다. 저를 포함한 많은 사람들이 좋은 아이디어라고 생각하지만 말이죠!
:::

[headers]: https://en.wikipedia.org/wiki/List_of_HTTP_header_fields  
[codes]: https://en.wikipedia.org/wiki/List_of_HTTP_status_codes  
[micropayments]: https://en.wikipedia.org/wiki/Micropayment  

Telnet in Python
================

지금까지 우리는 `telnet`을 사용해 다른 컴퓨터와 통신했습니다. 하지만 `telnet`은 매우 간단한 프로그램이며, 이를 프로그래밍적으로 구현할 수도 있습니다. 이 작업에는 URL에서 호스트 이름과 경로를 추출하고, *소켓*(socket)을 생성하고 요청을 보내며 응답을 받는 과정이 포함됩니다.[^why-not-parse]

[^why-not-parse]: Python에는 URL을 파싱하기 위한 `urllib.parse`라는 라이브러리가 있지만, 직접 구현해보는 것이 학습에 유익할 것입니다. 또한, 이 방법은 이 책을 Python에 특화되지 않도록 만듭니다.

URL 파싱부터 시작하겠습니다. URL을 파싱하여 `URL` 객체를 반환하도록 만들고, 파싱 코드를 생성자 내부에 작성하겠습니다:

```
class URL:
    def __init__(self, url):
        # ...
```

`__init__` 메서드는 클래스 생성자를 위한 Python의 독특한 문법이며, 항상 첫 번째 매개변수로 지정해야 하는 `self`는 Python에서 C++ 또는 Java의 `this`에 해당합니다.

먼저, URL에서 `://`로 구분된 scheme(스킴)을 처리해 보겠습니다. 우리 브라우저는 `http`만 지원하므로 이를 확인합니다:

```
class URL:
    def __init__(self, url):
        self.scheme, url = url.split("://", 1)
        assert self.scheme == "http"
```

이제 호스트와 경로를 나눠야 합니다. 호스트는 첫 번째 `/` 이전에 있고, 경로는 그 슬래시와 뒤쪽의 모든 것입니다:

```
class URL:
    def __init__(self, url):
        # ...
        if "/" not in url:
            url = url + "/"
        self.host, url = url.split("/", 1)
        self.path = "/" + url
```

`# ...`가 포함된 코드 블록을 본다면, 이는 기존 메서드나 코드 블록에 코드를 추가하고 있다는 뜻입니다. `split(s, n)` 메서드는 문자열을 `s`에서 최대 `n`번 나눕니다. 호스트 이름과 경로 사이의 (선택적인) 슬래시를 처리하는 논리가 여기에 포함되어 있습니다.

`URL` 객체에 `host`와 `path` 필드가 생겼으니, 해당 URL에서 웹 페이지를 다운로드할 수 있습니다. 이를 위해 새로운 `request` 메서드를 추가하겠습니다:

```
class URL:
    def request(self):
        # ...
```

Python에서는 `self` 매개변수를 항상 메서드에 작성해야 합니다. 앞으로는 메서드를 정의할 때 이를 따로 강조하지 않을 것입니다—코드 블록에 존재하지 않던 메서드나 함수를 추가하는 경우, 이는 우리가 그것을 정의하고 있다는 뜻입니다.

웹 페이지를 다운로드하는 첫 단계는 호스트와 연결하는 것입니다. 운영 체제는 "소켓"(socket)이라는 기능을 제공합니다. 소켓은 다른 컴퓨터와 통신하기 위한 것으로, 데이터를 주고받는 데 사용하는 인터페이스입니다. 소켓은 다양한 종류가 있습니다. 이를 결정하려면 다음과 같은 요소를 선택해야 합니다:

-   소켓의 *주소 계열*: 다른 컴퓨터를 찾는 방법을 결정합니다. 이름은 `AF`로 시작하며, 우리는 `AF_INET`을 사용하지만, 예로 `AF_BLUETOOTH`도 있습니다.
-   소켓의 *유형*: 대화의 종류를 설명합니다. 이름은 `SOCK`으로 시작하며, 우리는 `SOCK_STREAM`을 사용합니다. 이는 두 컴퓨터가 임의의 양의 데이터를 주고받을 수 있음을 의미합니다. 다른 예로는 `SOCK_DGRAM`이 있습니다.[^dgram]
-   소켓의 *프로토콜*: 두 컴퓨터가 연결을 설정하는 절차를 설명합니다. 프로토콜 이름은 주소 계열에 따라 다르며, 우리는 `IPPROTO_TCP`를 사용합니다.[^quic]

[^dgram]: `DGRAM`은 "datagram"(데이터그램)을 의미하며, 엽서와 비슷한 개념으로 생각하면 됩니다.

[^quic]: 최신 HTTP 버전에서는 [QUIC](https://en.wikipedia.org/wiki/QUIC)를 사용하기도 하지만, 우리의 브라우저는 HTTP 1.0에 머무릅니다.

이 모든 옵션을 선택하여 소켓을 생성할 수 있습니다:[^sockets]

[^sockets]: 이 코드에서는 Python의 `socket` 라이브러리를 사용하지만, 대부분의 언어에서 비슷한 API를 제공합니다. Python에서 전달하는 매개변수는 디폴트값이며, 생략하고 `socket.socket()`을 호출할 수도 있습니다. 다른 언어를 사용하는 독자를 위해 명시적으로 작성했습니다.

```
import socket

class URL:
    def request(self):
        s = socket.socket(
            family=socket.AF_INET,
            type=socket.SOCK_STREAM,
            proto=socket.IPPROTO_TCP,
        )
```

소켓이 생성되면 이를 다른 컴퓨터와 연결해야 합니다. 이를 위해 호스트와 *포트*\index{port}가 필요합니다. 사용하는 프로토콜에 따라 포트 번호가 다르며, 여기서는 80을 사용합니다.

```
class URL:
    def request(self):
        # ...
        s.connect((self.host, 80))
```

이 코드는 `example.org`와 통신하여 연결을 설정하고 데이터를 교환할 준비를 합니다.

::: {.quirk}
당연히, 오프라인 상태에서는 작동하지 않습니다. 또한 프록시 뒤에 있거나 더 복잡한 네트워킹 환경에서는 작동하지 않을 수도 있습니다. 해결책은 설정에 따라 다릅니다—프록시를 비활성화하는 것처럼 간단한 방법일 수도 있고, 더 복잡한 절차가 필요할 수도 있습니다.
:::

`connect` 호출 안에 두 개의 괄호가 있는 것을 주의하세요. `connect`는 하나의 인수를 받으며, 그 인수는 호스트와 포트를 포함하는 쌍입니다. 주소 계열에 따라 인수의 수가 다르기 때문입니다.

::: {.further}
Python이 소켓 API를 거의 그대로 구현하고 있지만, 이는 1983년 4.2 BSD Unix에서 설계된 "[버클리 소켓][bsd-sockets]" API에서 유래합니다. macOS와 iOS는 [BSD Unix 코드를 여전히 사용][mac-bsd]하지만, Windows와 Linux는 이를 재구현한 것입니다.
:::

[bsd-sockets]: https://en.wikipedia.org/wiki/Berkeley_sockets  
[mac-bsd]: https://developer.apple.com/library/archive/documentation/Darwin/Conceptual/KernelProgramming/BSD/BSD.html  

요청과 응답
====================

이제 연결이 되었으니 다른 서버에 요청을 보낼 수 있습니다. 이를 위해 `send` 메서드를 사용해 데이터를 전송합니다:

```
class URL:
    def request(self):
        # ...
        request = "GET {} HTTP/1.0\r\n".format(self.path)
        request += "Host: {}\r\n".format(self.host)
        request += "\r\n"
        s.send(request.encode("utf8"))
```

`send` 메서드는 요청을 서버로 전송합니다.[^send-return] 이 코드에서 정확해야 하는 몇 가지가 있습니다. 첫째, 줄바꿈에 `\n` 대신 `\r\n`을 사용하는 것이 매우 중요합니다. 또한 요청의 끝에 빈 줄을 추가하기 위해 두 개의 `\r\n`을 꼭 넣어야 합니다. 이를 잊으면 상대 컴퓨터는 줄바꿈을 기다리고, 여러분은 응답을 기다리게 됩니다.[^literal]

[^send-return]: `send`는 숫자를 반환하며, 예를 들어 `47`은 다른 컴퓨터에 보낸 데이터 바이트 수를 나타냅니다. 네트워크 연결이 데이터 전송 중간에 실패한 경우, 얼마나 전송했는지 확인할 수 있습니다.

[^literal]: 컴퓨터는 언제나 철저히 문자 그대로 처리합니다.

전송할 데이터를 준비할 때, `encode` 호출에 주의하세요. 데이터를 전송할 때는 텍스트, 이미지, 비디오 등 원시 바이트를 전송해야 합니다. 그러나 Python의 문자열은 텍스트를 구체적으로 나타내기 위해 사용됩니다. `encode` 메서드는 텍스트를 바이트로 변환하며, 이를 반대로 수행하는 `decode` 메서드도 있습니다.[^charset]

[^charset]: `encode`와 `decode`를 호출할 때 사용하려는 *문자 인코딩*을 지정해야 합니다. 이는 복잡한 주제입니다. 여기서는 `utf8`을 사용했는데, 이는 일반적으로 많은 페이지에서 작동합니다. 하지만 실제 세계에서는 더 신중해야 합니다.

```
>>> type("text")
<class 'str'>
>>> type("text".encode("utf8"))
<class 'bytes'>
```

`str`과 `bytes` 관련 오류가 발생하면, 어딘가에서 `encode` 또는 `decode` 호출을 빼먹었을 가능성이 있습니다.

서버의 응답을 읽으려면 소켓에서 `read` 함수를 사용할 수 있습니다. 이는 이미 응답된 비트들을 제공합니다. 그런 다음, 도착한 데이터를 수집하는 루프를 작성해야 합니다. 그러나 Python에서는 `makefile` 도우미 함수를 사용할 수 있어 해당 루프를 숨깁니다:[^socket-loop]

[^socket-loop]: 다른 언어에서는 `socket.read`만 사용할 수 있을 수도 있습니다. 이 경우 소켓 상태를 확인하는 루프를 직접 작성해야 합니다.

```
class URL:
    def request(self):
        # ...
        response = s.makefile("r", encoding="utf8", newline="\r\n")
```

`makefile`은 서버에서 받은 모든 바이트를 포함하는 파일과 유사한 객체를 반환합니다. 여기서 Python에 `utf8` 인코딩(바이트를 문자로 연결하는 방법)과 HTTP의 특이한 줄바꿈을 처리하도록 지시하고 있습니다.[^utf8]

[^utf8]: `utf8`을 하드코딩하는 것은 정확한 방법은 아니지만, 대부분의 영어 웹사이트에서는 적절히 작동합니다. 사실, `Content-Type` 헤더에는 본문의 인코딩을 지정하는 `charset` 선언이 포함되어 있는 경우가 많습니다. 부재 시에도 브라우저는 `utf8`을 기본값으로 설정하지 않고, 문자 빈도를 기반으로 추측합니다. 이 과정에서 잘못된 추측이 이루어지면 이상한 `�`, `strange áççêñ£ß` 등의 문자가 나타날 수 있습니다.

이제 응답을 여러 부분으로 나누겠습니다. 첫 번째 줄은 상태 줄입니다:^[200만 이 브라우저가 지원하는 코드이므로 이를 요구할 수도 있습니다. 그러나 서버는 일반적으로 에러 코드에서도 이해하기 쉬운 HTML 에러 페이지를 제공합니다. 이는 웹이 점진적으로 구현하기 쉬운 방식 중 하나입니다.]

```
class URL:
    def request(self):
        # ...
        statusline = response.readline()
        version, status, explanation = statusline.split(" ", 2)
```

여기서 서버의 HTTP 버전이 나와 동일한지 확인하지 않습니다. 좋은 아이디어처럼 보이지만, 잘못 구성된 서버는 HTTP 1.0으로 요청하더라도 HTTP 1.1로 응답합니다.^[다행히 프로토콜이 충분히 유사하므로 혼란을 초래하지 않습니다.]

상태 줄 뒤에는 헤더가 옵니다:

```
class URL:
    def request(self):
        # ...
        response_headers = {}
        while True:
            line = response.readline()
            if line == "\r\n": break
            header, value = line.split(":", 1)
            response_headers[header.casefold()] = value.strip()
```

헤더는 각 줄을 처음 나오는 콜론에서 나누어, 헤더 이름과 값의 매핑을 채웁니다. 헤더는 대소문자를 구분하지 않으므로 소문자로 통일합니다.[^casefold] 또한, HTTP 헤더 값에서 공백은 중요하지 않으므로, 앞뒤의 여분 공백을 제거합니다.

[^casefold]: [`casefold`][casefold]는 여러 언어를 더 잘 지원하므로 `lower` 대신 사용합니다.

[casefold]: https://docs.python.org/3/library/stdtypes.html#str.casefold  

특정 헤더는 우리가 액세스하려는 데이터가 비정상적으로 전송되고 있음을 나타낼 수 있습니다. 이런 헤더들이 없는지 확인합시다:[^if-te]

[^if-te]: 1-9번 연습 문제는 이러한 헤더가 있을 경우 브라우저가 어떻게 처리해야 하는지를 설명합니다.

```
class URL:
    def request(self):
        # ...
        assert "transfer-encoding" not in response_headers
        assert "content-encoding" not in response_headers
```

보낸 데이터를 가져오는 일반적인 방법은 헤더 이후의 모든 데이터를 읽는 것입니다:

```
class URL:
    def request(self):
        # ...
        content = response.read()
        s.close()
```

이제 표시할 데이터는 본문이므로, 이를 반환합니다:

```
class URL:
    def request(self):
        # ...
        return content
```

이제 응답 본문에 포함된 텍스트를 실제로 출력해 보겠습니다.

::: {.further}
[`Content-Encoding`][ce-header] 헤더는 서버가 웹 페이지를 전송하기 전에 압축할 수 있도록 합니다. 큰 텍스트 기반 웹 페이지는 압축 효율성이 좋으며, 이로 인해 페이지 로딩 속도가 빨라집니다. 브라우저는 요청에 [`Accept-Encoding` 헤더][ae-header]를 포함하여 지원하는 압축 알고리즘 목록을 전달해야 합니다. [`Transfer-Encoding`][te-header]도 유사하며 데이터를 "청크"로 전송할 수 있습니다. 많은 서버들이 압축과 함께 이를 사용합니다.
:::

[ce-header]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Encoding  
[te-header]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Transfer-Encoding  
[ae-header]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Encoding  

HTML 표시
===================

응답 본문에 포함된 HTML 코드는 브라우저에서 `http://example.org/index.html`로 이동했을 때 표시되는 콘텐츠를 정의합니다. 향후 장에서 HTML에 대해 훨씬 더 자세히 다룰 예정이지만, 지금은 아주 단순하게 설명하겠습니다.

HTML에는 *태그*와 *텍스트*가 있습니다. 각 태그는 `<`로 시작하고 `>`로 끝납니다. 일반적으로 태그는 콘텐츠의 종류를 나타내며, 텍스트는 실제 콘텐츠입니다.[^content-tag] 대부분의 태그는 시작 태그와 종료 태그로 쌍을 이루는데, 예를 들어, 페이지의 제목은 `<title>`과 `</title>` 태그 쌍으로 감싸져 있습니다. 각 태그 내부의 꺾쇠 괄호에는 태그 이름\index{tag name} (여기서는 `title`)이 있으며, 선택적으로 공백 뒤에 *속성*이 올 수 있습니다. 종료 태그에는 `/`와 태그 이름이 포함되며 속성은 없습니다.

[^content-tag]: 그러나 `img`와 같은 일부 태그는 콘텐츠 자체이며, 이를 설명하는 정보가 아닙니다.

따라서, 아주 단순한 웹 브라우저를 만들어 페이지 HTML을 가져오고 태그가 아닌 텍스트만 출력해 보겠습니다.[^python2] 이를 위해 새로운 함수 `show`를 작성합니다:^["show"는 `URL` 클래스에 속하지 않는 전역 함수입니다.]

[^python2]: 이 예제가 마지막 줄의 `end`와 관련된 `SyntaxError`를 발생시킬 경우, Python 2를 실행 중일 가능성이 높습니다. Python 3을 사용하는지 확인하세요.

```
def show(body):
    in_tag = False
    for c in body:
        if c == "<":
            in_tag = True
        elif c == ">":
            in_tag = False
        elif not in_tag:
            print(c, end="")
```

이 코드는 다소 복잡합니다. 요청 본문을 문자 단위로 처리하며, 두 가지 상태를 가집니다: `in_tag`는 현재 꺽쇠 괄호 쌍 사이에 있는 경우이고, 그렇지 않을 경우는 `not in_tag` 상태입니다. 현재 문자가 꺽쇠 괄호인 경우 상태가 전환되며, 태그 안에 들어 있지 않은 일반 문자는 출력됩니다.[^python-newline]

[^python-newline]: `end` 인자는 Python에게 문자를 출력한 후 줄바꿈을 하지 않도록 지시합니다. 기본적으로 Python은 출력 후 줄바꿈을 수행합니다.

이제 `request`와 `show`를 결합하여 웹 페이지를 로드할 수 있습니다:^["show"처럼 이것도 전역 함수입니다.]

```
def load(url):
    body = url.request()
    show(body)
```

다음 코드를 추가하여 명령줄에서 `load`를 실행해 보세요:

```
if __name__ == "__main__":
    import sys
    load(URL(sys.argv[1]))
```

첫 번째 줄은 Python에서의 `main` 함수로, 이 스크립트를 명령줄에서 실행할 때만 실행됩니다. 이 코드는 명령줄에서 첫 번째 인자(`sys.argv[1]`)를 읽고 이를 URL로 사용합니다. 다음 명령어로 이 코드를 실행해 보세요:

```
python3 browser.py http://example.org/
```

공식 예제 웹 페이지로 환영하는 짧은 텍스트를 확인할 수 있을 것입니다. 또한 [이 장의 링크](http://browser.engineering/http.html)로도 시도해 볼 수 있습니다!

::: {.further}
HTML은 URL이나 HTTP처럼 기본적으로 간단히 파싱하고 표시하기 쉽도록 설계되었습니다. 초기에는 HTML의 기능이 매우 제한적이어서, 여기서 다룬 것보다 약간 더 정교한 수준으로 구현하더라도 여전히 사용 가능한 방식으로 콘텐츠를 표시할 수 있었습니다. 우리의 매우 간단하고 기본적인 HTML 파서도 이미 [browser.engineering](https://browser.engineering/) 웹사이트의 텍스트를 출력할 수 있습니다.
:::

암호화된 연결
=====================

지금까지 우리 브라우저는 `http` 스킴을 지원했습니다. 이는 매우 일반적인 스킴이지만, 점점 더 많은 웹사이트가 `https`\index{HTTPS} 스킴으로 이전하고 있으며, 일부 웹사이트는 이를 필수로 요구합니다.

`http`와 `https`의 차이점은 `https`가 훨씬 더 **보안적**이라는 것입니다. 하지만 좀 더 구체적으로 살펴보겠습니다. `https`, 또는 더 정확히는 TLS\index{TLS}\index{SSL}(Transport Layer Security)을 사용하는 HTTP는 일반 `http` 스킴과 동일하지만, 브라우저와 호스트 간의 모든 통신이 암호화된다는 점에서 차이가 있습니다. 여기에는 어떤 암호화 알고리즘이 사용되는지, 공통 암호화 키를 어떻게 설정하는지, 브라우저가 올바른 호스트에 연결되었는지를 확인하는 방법 등 많은 세부 내용이 있습니다. 프로토콜 계층의 차이는 그림 5에서 확인할 수 있습니다.

::: {.center}
![그림 5: HTTP와 HTTPS의 차이점은 TLS 계층의 추가입니다.](im/http-tls-2.gif)
:::

다행히도 Python의 `ssl` 라이브러리는 이러한 세부 사항을 모두 구현해 주기 때문에, 암호화된 연결을 설정하는 것은 일반 연결을 설정하는 것만큼 쉽습니다. 이 간편함은 상황에 따라 부적절할 수 있는 기본 설정을 허용하는 데서 오는 것이지만, 학습 목적 상으로는 충분합니다.

`ssl`을 사용하여 암호화된 연결을 설정하는 것은 간단합니다. 이미 `s`라는 소켓을 생성하고 이를 `example.org`에 연결했다고 가정해 보겠습니다. 암호화를 위해 `ssl.create_default_context`를 사용하여 *컨텍스트* `ctx`를 생성하고 이 컨텍스트를 사용해 소켓 `s`를 *래핑*합니다:

```
import ssl
ctx = ssl.create_default_context()
s = ctx.wrap_socket(s, server_hostname=host)
```

`wrap_socket`은 새로운 소켓을 반환하며, 이를 다시 `s` 변수에 저장합니다. 이는 원래의 소켓을 통해 데이터를 보내지 않기 위해서인데, 그렇게 하면 암호화되지 않을 뿐 아니라 혼란을 초래할 수도 있기 때문입니다. `server_hostname` 인자는 브라우저가 올바른 서버에 연결되었는지 확인하는 데 사용됩니다. 이 값은 `Host` 헤더와 일치해야 합니다.

::: {.installation}
macOS에서는 Python의 `ssl` 패키지를 대부분의 웹사이트에서 사용하기 전에 ["Install Certificates" 프로그램 실행][macos-fix]이 필요할 수 있습니다.
:::

[macos-fix]: https://stackoverflow.com/questions/52805115/certificate-verify-failed-unable-to-get-local-issuer-certificate  

이제 위 코드를 `request` 메서드에 추가해 보겠습니다. 먼저 어떤 스킴을 사용하는지 확인해야 합니다:

```
import ssl

class URL:
    def __init__(self, url):
        self.scheme, url = url.split("://", 1)
        assert self.scheme in ["http", "https"]
        # ...
```

(여기서는 기존 스킴 파싱 코드를 이 새로운 코드로 교체해야 합니다. 문맥과 코드 자체에서 무엇을 교체해야 하는지 명확하게 알 수 있을 것입니다.)

암호화된 HTTP 연결은 보통 포트 443을 사용하며, 일반 HTTP는 포트 80을 사용합니다:

```
class URL:
    def __init__(self, url):
        # ...
        if self.scheme == "http":
            self.port = 80
        elif self.scheme == "https":
            self.port = 443
```

소켓을 생성할 때 이 포트를 사용할 수 있습니다:

```
class URL:
    def request(self):
        # ...
        s.connect((self.host, self.port))
        # ...
```

다음으로, `ssl` 라이브러리로 소켓을 래핑합니다:

```
class URL:
    def request(self):
        # ...
        s.connect((self.host, self.port))
        if self.scheme == "https":
            ctx = ssl.create_default_context()
            s = ctx.wrap_socket(s, server_hostname=self.host)
        # ...
```

이제 브라우저가 HTTPS 사이트에 연결할 수 있습니다.

이 기회에 URL에서 호스트 이름 뒤에 콜론을 삽입하여 지정할 수 있는 커스텀 포트도 지원하도록 추가해 보겠습니다. 그림 6을 참조하세요.

::: {.cmd .web-only html=True}
    python3 infra/annotate_code.py <<EOF
    http://example.org:[8080][tl|Port]/index.html
    EOF
:::

::: {.center .web-only}
그림 6: URL에서 포트가 위치하는 곳.
:::

::: {.print-only}
![그림 6: URL에서 포트가 위치하는 곳.](im/http-ports.png)
:::

URL에 포트가 포함된 경우 이를 파싱하여 사용할 수 있습니다:

``` {.python}
class URL:
    def __init__(self, url):
        # ...
        if ":" in self.host:
            self.host, port = self.host.split(":", 1)
            self.port = int(port)
```

커스텀 포트는 디버깅에 유용합니다. Python에는 컴퓨터 파일을 제공하는 데 사용할 수 있는 내장 웹 서버가 있습니다. 예를 들어, 다음 명령을 실행하면:

```
python3 -m http.server 8000 -d /some/directory
```

`http://localhost:8000/`로 이동하면 해당 디렉토리의 모든 파일을 확인할 수 있습니다. 이를 통해 브라우저를 테스트하기에 적합합니다.

::: {.further}
TLS는 매우 복잡합니다. 자세한 내용을 [RFC 8446](https://tools.ietf.org/html/rfc8446)에서 읽어볼 수 있지만, 직접 구현하는 것은 권장되지 않습니다. TLS를 올바르고 안전하게 구현하는 것은 매우 어렵습니다.
:::

이제 여러분은 프로그램을 모든 웹 페이지에서 실행할 수 있어야 합니다. 간단한 예제에 대해 출력값은 다음과 같아야 합니다:

[example-simple]: examples/example1-simple.html

```

  
    This is a simple
    web page with some
    text in it.
  

```

요약
=======

이 장에서는 빈 파일에서 시작해 기본적인 웹 브라우저 기능을 구현했습니다. 이 브라우저는 다음을 수행할 수 있습니다:

- URL을 스킴(scheme), 호스트(host), 포트(port), 경로(path)로 파싱합니다.
- `socket` 및 `ssl` 라이브러리를 사용해 해당 호스트에 연결합니다.
- `Host` 헤더를 포함한 HTTP 요청을 해당 호스트에 전송합니다.
- HTTP 응답을 상태 줄, 헤더, 본문으로 분리합니다.
- 본문에 있는 텍스트(태그가 아닌 텍스트)를 출력합니다.

물론, 이 브라우저는 여전히 명령줄 도구에 더 가깝지만, 이미 브라우저의 핵심 기능 몇 가지를 갖추고 있습니다.

::: {.signup}
:::

개요
=======

우리의 브라우저에서 구현된 모든 함수, 클래스, 메서드의 전체 목록은 다음과 같을 것입니다:

::: {.web-only .cmd .python .outline html=True}
    python3 infra/outlines.py --html src/lab1.py --template book/outline.txt
:::

::: {.print-only .cmd .python .outline}
    python3 infra/outlines.py src/lab1.py --template book/outline.txt
:::

연습 문제
=========

**1-1 HTTP/1.1**: `request` 함수에서 `Host`와 함께 `Connection` 헤더를 추가하여 `"close"` 값을 설정하세요. 이제 브라우저는 `HTTP/1.1`을 사용한다고 선언할 수 있습니다. 또한 `User-Agent` 헤더를 추가하세요. 이 값은 브라우저를 호스트에 식별하는 데 사용되며, 임의의 값으로 설정할 수 있습니다. 이후에 추가 헤더를 쉽게 추가할 수 있도록 구현하세요.

**1-2 파일 URL**: 브라우저가 `file` 스킴을 지원하도록 추가하세요. 이는 로컬 파일을 여는 기능을 제공합니다. 예를 들어, `file:///path/goes/here`는 컴퓨터의 `/path/goes/here` 위치에 있는 파일에 대응해야 합니다. 또한, 브라우저 실행 시 URL을 지정하지 않으면 특정 컴퓨터 파일을 열도록 설정하세요. 이를 빠른 테스트를 위한 파일로 사용할 수 있습니다.

**1-3 데이터 URL**: 또 다른 스킴은 `data`로, HTML 콘텐츠를 URL 자체에 인라인으로 포함할 수 있습니다. 예를 들어, `data:text/html,Hello world!` URL을 실제 브라우저에서 탐색해 보세요. 이 스킴을 브라우저에 추가 지원하세요. `data` 스킴은 테스트 작성 시 별도 파일 없이도 편리하게 사용할 수 있습니다.

**1-4 엔터티**: `<`(`&lt;`) 및 `>`(`&gt;`) 엔터티를 지원하세요. 이를 각각 `<` 및 `>`로 출력해야 합니다. 예를 들어, HTML 응답이 `&lt;div&gt;`일 경우 브라우저의 `show` 메서드는 `<div>`를 출력해야 합니다. 엔터티는 브라우저가 태그로 해석하지 않도록 특별 문자를 포함할 수 있게 해줍니다.

**1-5 view-source**: `view-source` 스킴을 지원하세요. `view-source:http://example.org/`로 이동하면 렌더링된 페이지 대신 HTML 소스가 표시되어야 합니다. 이 스킴을 구현하고 HTML 파일 전체를 텍스트처럼 출력해 보세요. 연습 문제 1-4를 구현했다면 유용할 것입니다.

**1-6 Keep-alive**: 연습 문제 1-1을 구현하되, `Connection: close` 대신 `Connection: keep-alive` 헤더를 전송하세요. 소켓에서 본문을 읽을 때 `Content-Length` 헤더에 명시된 바이트 수만 읽고 소켓을 닫지 마세요. 대신 동일한 서버에 대해 추가 요청이 있을 경우 새 소켓을 생성하는 대신 기존 소켓을 재사용하세요. (이 작업을 위해 `makefile`에 `"rb"` 옵션을 전달해야 합니다.)

**1-7 리다이렉트**: 300번대 에러 코드는 리다이렉트를 요청합니다. 브라우저가 이를 발견하면 `Location` 헤더에 명시된 URL로 새 요청을 만들어야 합니다. `Location` 헤더가 전체 URL일 수도 있지만, 호스트와 스킴 없이 `/`로 시작할 수도 있습니다. 이 경우 원래 요청과 동일한 호스트와 스킴을 사용해야 합니다. 새 URL이 또 다른 리다이렉트를 발생시킬 수도 있으므로 이 경우도 처리하세요. 단, 리다이렉트 루프에 갇히지 않도록 한 번에 따라갈 수 있는 리다이렉트 수를 제한하세요.

**1-8 캐싱**: 일반적으로 동일한 이미지, 스타일, 스크립트는 여러 페이지에서 재사용됩니다. 이를 반복적으로 다운로드하는 것은 리소스 낭비입니다. 요청이 `GET`이고 응답이 `200`인 경우 HTTP 응답을 캐싱하세요. 서버는 `Cache-Control` 헤더를 사용하여 캐시를 제어합니다. 이 헤더에서 `no-store`와 `max-age` 값을 지원하세요. `Cache-Control` 헤더에 이 두 값 외의 다른 값이 포함된 경우 응답을 캐시하지 않는 것이 좋습니다.

**1-9 압축**: 브라우저가 서버에 압축된 데이터를 수신할 수 있음을 알리는 `Accept-Encoding` 헤더(`gzip` 값)를 전송하도록 구현하세요. 서버가 압축을 지원하면 응답은 `Content-Encoding` 헤더에 `gzip` 값이 포함되고 본문은 압축됩니다. 데이터를 압축 해제하려면 `gzip` 모듈의 `decompress` 메서드를 사용할 수 있습니다. GZip 데이터는 `utf8`로 인코딩되지 않으므로, `makefile`에 `"rb"` 옵션을 전달하여 바이너리 데이터를 처리하세요. 대부분의 웹 서버는 압축된 데이터를 [`chunked`][chunked]를 사용해 전송하므로 이 기능도 추가 지원해야 합니다.

[chunked]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Transfer-Encoding  
```