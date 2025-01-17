---
title: History of the Web
type: Background
next: http
prev: intro
...

이 장에서는 웹\index{web} 자체의 역사, 즉 웹이 어디에서 비롯되었는지, 그리고 웹과 브라우저가 오늘날까지 어떻게 발전해 왔는지를 탐구합니다. 이 역사는 철저하지 않으며,[^sgml] 웹을 이끈 주요 사건과 아이디어, 그리고 발명가들의 목표와 동기를 중점적으로 다룹니다.

[^sgml]: 예를 들어, HTML의 전신인 표준 일반화 마크업 언어 ([SGML](https://en.wikipedia.org/wiki/Standard_Generalized_Markup_Language))와 관련된 내용은 거의 다루지 않습니다. (이 각주를 제외하고요!)

Memex 개념
=================

::: {.center}  
그림 1: "As We May Think"의 원본 출판. ([Dunkoman][dunkoman], [Wikipedia][the-memex] 및 [CC BY 2.0][cc-by-2]에서 발췌.)  
:::

[dunkoman]: https://www.flickr.com/people/79255326@N00  
[cc-by-2]: https://creativecommons.org/licenses/by/2.0/legalcode  
[the-memex]: https://commons.wikimedia.org/wiki/File:The_Memex_(3002477109).jpg  

컴퓨터가 정보를 혁명적으로 바꿀 수 있는 방법을 탐구한 영향력 있는 초기 에세이 중 하나는 Vannevar Bush가 1945년에 작성한 "[As We May Think](https://en.wikipedia.org/wiki/As_We_May_Think)"입니다. 이 에세이는 개인이 세상의 모든 정보를 보고 탐색할 수 있도록 돕는 [Memex](https://en.wikipedia.org/wiki/Memex)라는 기계를 상상했습니다 (그림 1 참고). 당시의 마이크로필름 화면 기술을 기반으로 설명되었지만, 그 목적과 개념은 오늘날 우리가 알고 있는 웹과 분명히 유사한 부분이 있으며, 사용자 인터페이스와 기술 세부 사항은 다릅니다.

웹의 핵심은 정보를 _표현하고 표시_하는 Memex와 같은 목표를 중심으로 조직되어 있으며, 인간이 효과적으로 배우고 탐험할 수 있는 방법을 제공합니다. 인간의 집단적 지식과 지혜는 이미 한 개인, 조직, 도서관, 국가, 문화, 집단 또는 언어의 용량을 훨씬 초과한 지 오래되었습니다. 그러나 우리는 인간으로서 모든 지식을 알 수는 없지만, 기술을 사용하여 이전보다 더 효율적으로 배우고, 특히 배워야 할 정보를 빠르게 액세스하거나 기억하거나 회상할 수 있습니다. 다음은 Vannevar Bush가 상상했던 연구 세션의 한 예로, 우리가 웹을 사용하는 방식과 놀랍도록 비슷합니다:

> Memex의 소유자는 활과 화살의 기원과 특성에 관심이 있다고 가정해 보겠습니다. [...] 그는 자신의 Memex에 관련 가능성이 있는 책과 기사가 수십 권 있습니다. 먼저 백과사전을 훑어보며 흥미롭지만 대략적인 기사를 찾아내고, 이를 띄워둡니다. 그다음, 역사책에서 또 다른 관련 자료를 찾아내고 두 가지를 연결합니다. 이런 식으로 많은 항목의 흔적을 만들어 나갑니다.

컴퓨터와 인터넷은 우리가 원하는 정보를 _처리하고 저장_할 수 있게 합니다. 그러나 정보를 _구조화하고 찾는_ 데 도움을 주는 것은 바로 _웹_입니다. 이를 통해 지식이 유용해집니다.[^google-mission]

[^google-mission]: 구글의 잘 알려진 [미션](https://about.google/) 선언문인 "세상의 정보를 조직하여 전 세계에서 접근 가능하고 유용하게 만드는 것"은 거의 똑같습니다. 이는 우연의 일치가 아닙니다. 검색 엔진 개념은 본질적으로 웹과 연결되어 있으며, 웹과 관련 전신의 설계에서 영감받았습니다.

"As We May Think"는 Memex의 두 가지 특징인 정보 기록 조회와 관련 기록 간의 연결을 강조했습니다. 실제로, 에세이는 후자의 중요성을 강조하며, 우리는 *알려지지 않은 것을 연결함으로써* 배운다고 설명합니다:

> 어떤 종류의 데이터가 저장소에 저장될 때, 그것은 알파벳 순서나 숫자 순서로 파일이 정리됩니다. [...] 그러나 인간의 마음은 그렇게 작동하지 않습니다. 그것은 연관성을 기반으로 작동합니다.

Bush가 말한 "연관성"이란, 사람이 정리한 링크를 통해 한 기록에서 다음 기록으로의 사고의 흐름을 의미합니다. 그는 단순한 보편적 도서관이 아니라, 우리가 배운 결과를 기록하는 보편적 방법을 상상했습니다.

```
웹의 등장
=========

[하이퍼텍스트][hypertext]\index{hypertext} 문서가 [하이퍼링크][hyperlink]\index{hyperlink}로 연결된다는 개념은
1964–65년 [Project Xanadu][xanadu]에서 Ted Nelson에 의해 발명되었습니다.[^literary-criticism]
하이퍼텍스트는 다른 텍스트로 연결되는 하이퍼링크로 마크업된 텍스트입니다.[^back-button]
익숙하게 들리지 않나요? 웹 페이지\index{web page}는 하이퍼텍스트이며, 웹 페이지 간의 링크는 하이퍼링크입니다.
웹 페이지를 작성하는 포맷은 HTML이고, 웹 페이지를 로드하는 프로토콜은 HTTP입니다. 이 둘의 약어에는 모두 "하이퍼텍스트(HyperText)"라는 말이 들어 있습니다. 초기 Hypertext Editing System의 예시는 그림 2를 참조하세요.

[^back-button]: [Hypertext Editing System]이라는 후속 시스템이 모든 브라우저에 있는 뒤로가기 버튼을 처음 도입했습니다. 이 시스템은 텍스트만 포함하고 있었기 때문에 "버튼" 자체가 텍스트였습니다.

[Hypertext Editing System]: https://en.wikipedia.org/wiki/Hypertext_Editing_System

[hyperlink]: https://en.wikipedia.org/wiki/Hyperlink#History

[^literary-criticism]: 그는 학문 및 문학 커뮤니티에서 오랜 전통으로 자리 잡은 인용과 비평에서 영감을 받았습니다. Project Xanadu의 연구 논문은 이러한 활용 사례에서 많은 동기를 얻었습니다.
```
::: {.center}
그림 2: 1969년 하이퍼텍스트 편집 시스템을 사용하는 컴퓨터 운영자. (Gregory Lloyd, [Wikipedia][wiki-hes], [CC BY-SA 4.0 International][cc-by-sa-4i].)
:::

[wiki-hes]: https://commons.wikimedia.org/wiki/File:HES_IBM_2250_Console_grlloyd_Oct1969.png  
[cc-by-sa-4i]: https://creativecommons.org/licenses/by-sa/4.0/deed.en  

Project Xanadu와는 독립적으로, 최초의 하이퍼링크 시스템은 단일 문서 내에서 스크롤을 가능하게 하기 위해 등장했으며, 이후 여러 문서 간의 연결로 일반화되었습니다. 그리고 원래의 시스템과 마찬가지로 웹은 문서 내 링크 및 문서 간 링크 기능을 모두 가지고 있습니다. 예를 들어, URL `http://browser.engineering/history.html#the-web-emerges`는 "`history.html`"이라는 문서를 참조하며, 그 문서 내 "`the-web-emerges`"라는 이름이 있는 특정 요소를 가리킵니다. 해당 URL에 방문하면 이 장을 로드하고 이 섹션으로 스크롤됩니다.

이 연구는 또한 Douglas Engelbart의 [모든 데모의 어머니(Mother of All Demos)](https://en.wikipedia.org/wiki/The_Mother_of_All_Demos)의 핵심 부분을 형성하고 영감을 주었습니다. 이는 컴퓨팅 역사상 가장 영향력 있는 기술 시연으로 손꼽힙니다(그림 3 참조). 그 데모는 웹의 주요 개념뿐 아니라, 브라우저 UI의 중심 구성 요소인 컴퓨터 마우스와 그래픽 사용자 인터페이스도 소개했습니다.[^even-more]

::: {.center}
그림 3: Doug Engelbart가 "모든 데모의 어머니(Mother of All Demos)"를 발표하는 모습. (SRI International, [Doug Engelbart Institute][dei] 제공.)
:::

[dei]: https://www.dougengelbart.org/content/view/374/464/  

[^even-more]: 그 데모는 이 외에도 많은 것을 제시했습니다. 현재 어느 컴퓨터 시스템에서도 실현되지 않은 부분이 포함되어 있습니다. 한번 시청해 보세요!

[hypertext]: https://en.wikipedia.org/wiki/Hypertext  
[xanadu]: https://en.wikipedia.org/wiki/Project_Xanadu  
```
이 연구와 문서–URL–하이퍼링크로 구성된 웹의 설정 간에는 매우 직접적인 연관성이 있습니다. 이는 하이퍼텍스트 아이디어를 기반으로 하여 실제로 적용한 사례입니다. 예를 들어, [HyperTIES](http://www.cs.umd.edu/hcil/hyperties/) 시스템은 하이라이트된 하이퍼링크를 갖추고 있었으며, 세계 최초의 전자 출판 학술 저널인 1988년 호 [*Communications of the ACM*](https://cacm.acm.org/)을 개발하는 데 사용되었습니다. 팀 버너스 리(Tim Berners-Lee)는 이 1988년 호에서 월드 와이드 웹(World Wide Web)에 대한 영감을 얻었으며,[^world-wide-web-terminology] 링크 개념과 인터넷 가용성을 결합하여 지난 수십 년간의 모든 작업의 원래 목표 중 많은 부분을 실현했습니다.[^realize-web-decades]

"하이퍼링크"라는 단어는 1987년 Apple 컴퓨터의 [HyperCard] 시스템과 관련하여 만들어졌을 가능성이 있습니다. 이 시스템은 사용자가 클릭 같은 이벤트를 처리하고 UI를 개선하는 동작을 수행하는 스크립트\index{script}를 통해 하이퍼텍스트를 증강하는 개념을 도입한 최초의, 혹은 최초 중 하나였습니다. 이는 웹 페이지의 JavaScript와 매우 유사합니다! 또한 대부분의 선구적인 시스템과 달리 텍스트뿐만 아니라 그래픽 UI 요소도 포함하고 있었습니다.

[HyperCard]: https://en.wikipedia.org/wiki/HyperCard

[^world-wide-web-terminology]: 오늘날 월드 와이드 웹(World Wide Web)은 단순히 "웹" 또는 "웹 생태계"라고 불립니다. "생태계"라는 단어는 "World Wide"와 같은 개념을 설명하는 또 다른 방법입니다. 원래의 용어는 여전히 많은 웹사이트\index{website} 도메인 이름에 포함된 "www"\index{WWW}로 남아 있습니다.

[^realize-web-decades]: 웹 자체가 이전의 야망과 꿈을 실현한 것처럼, 오늘날 우리는 웹이 제시한 비전을 실현하기 위해 노력하고 있습니다. (아직 다 이루어진 것은 아닙니다!)
```

1989년에서 1990년 사이 팀 버너스 리(Tim Berners-Lee)에 의해 첫 번째 웹 브라우저인 WorldWideWeb(그림 4 참조)과 첫 번째 웹 서버 `httpd`(HTTP Daemon)가 탄생했습니다. 흥미롭게도, 당시 브라우저의 기능은 이 책에서 만들게 될 브라우저보다 열등했지만,[^no-css] 어떤 면에서는 현대 브라우저에서도 볼 수 없는 기능들을 가지고 있었습니다.[^more-less-powerful] 1990년 12월 20일, [첫 번째 웹 페이지](http://info.cern.ch/hypertext/WWW/TheProject.html)가 작성되었습니다. 이 책에서 구현할 브라우저는 지금도 이 웹 페이지를 렌더링할 수 있습니다.[^original-aesthetics] 1991년, Berners-Lee는 [`alt.hypertext` Usenet 그룹](https://www.w3.org/People/Berners-Lee/1991/08/art-6484.txt)에 브라우저와 그 개념을 광고했습니다.

::: {.center}
그림 4: WorldWideWeb 브라우저의 스크린샷. ([*Communications of the ACM*][cacm94], August 1994 발췌.)
:::

[cacm94]: https://dl.acm.org/doi/10.1145/179606.179671  

[^no-css]: CSS도 없고, JS도 없으며, 이미지도 없습니다!  

[^more-less-powerful]: 예를 들어, 최초의 브라우저는 사이트 내에서 검색하기 위한 인덱스 페이지 개념을 포함하고 있었습니다(이 흔적은 URL 경로가 /로 끝날 때의 “index.html” 관례에서 여전히 볼 수 있습니다). 또한 WYSIWYG 웹 페이지 편집기 기능을 갖추고 있었습니다(HTML의 “contenteditable” 속성이 DOM 요소에서 [16장](invalidation.md)을 참조하세요). 이 속성은 유사한 의미적 동작을 하지만, 내장된 파일 저장 기능은 사라졌습니다). 오늘날, 인덱스는 검색 엔진으로 대체되었고, 웹 페이지 편집기의 개념도 오늘날의 웹 페이지 렌더링이 매우 동적임에 따라 다소 구식이 되었습니다.

[^original-aesthetics]: 또한 이 웹 페이지는 초창기 미학을 그대로 유지한 채 업데이트되지 않았습니다.  

버너스 리(Tim Berners-Lee)의 [웹에 대한 간략한 역사](https://www.w3.org/DesignIssues/TimBook-old/History.html)는 우리가 오늘날 알고 있는 월드 와이드 웹(World Wide Web)이 웹으로 발전하게 된 여러 주요 요인을 강조합니다. 주요 요인 중 하나는 웹의 **탈중앙화된 특성**으로, 그는 자신이 일했던 [CERN](https://home.cern/)의 학문적 문화에서 비롯된 것으로 설명합니다. 월드 와이드 웹은 이전 또는 이후의 많은 시스템과 구별되는 핵심 특징인 탈중앙화 구조를 가지고 있었으며, 그는 이를 다음과 같이 설명했습니다(이탤릭체는 필자의 강조입니다):

> 분명히 모두가 접근할 수 있는 Enquire[^enquire]와 같은 것이 필요했습니다. 
> 저는 두 사람이 독립적으로 이 시스템을 사용하기 시작하고 후에 협업을 시작하더라도, 
> *그들이 다른 어떤 변화를 가하지 않고도 자신들의 정보를 서로 연결할 수 있기를* 원했습니다. 
> 이것이 바로 웹의 개념이었습니다.

[^enquire]: Enquire는 버너스 리가 개발한 초기의 웹과 유사한 데이터베이스 시스템입니다.

이 인용은 웹의 핵심 가치 제안 중 하나인 **탈중앙화**를 포착합니다. 웹이 성공할 수 있었던 이유는 여러 가지가 있었지만, 모두 탈중앙화와 깊은 관련이 있었습니다:

- 어떤 작업을 수행하는 데 있어 중앙에서 관리하는 '게이트키퍼'가 없었기 때문에, 심지어 초보자도 간단한 웹 페이지를 쉽게 만들고 이를 게시할 수 있었습니다.

- 페이지는 단순히 URL로 식별되었기 때문에, 이메일, 소셜 네트워킹, 검색 엔진과 같은 외부 소스에서 웹으로 트래픽이 유입될 수 있었습니다. 또한 사이트 간 호환성과 하이퍼링크의 강력한 연결 효과([네트워크 효과](https://en.wikipedia.org/wiki/Network_effect))가 웹 내부에서의 링크 효과를 더욱 강화했습니다.

- 웹은 단일 기관의 통제를 벗어나 있었고, 이를 표준화 기구를 통해 유지하면서 독점 통제 및 조작의 문제를 피할 수 있었습니다.

브라우저
========

최초로 **광범위하게 배포된** 브라우저는 [ViolaWWW](https://en.wikipedia.org/wiki/ViolaWWW)(그림 5 참조)였을 가능성이 있습니다. 이 브라우저는 애플릿과 이미지 같은 여러 흥미로운 기능을 처음으로 선보였습니다. ViolaWWW는 다시 [NCSA Mosaic](https://en.wikipedia.org/wiki/Mosaic_(web_browser))(그림 6 참조)에 영감을 주었으며, 이 브라우저는 1993년에 출시되었습니다. Mosaic의 두 명의 원작자 중 한 명은 이후 넷스케이프를 공동 창립했으며, 넷스케이프는 최초의 **상업용 브라우저**[^commercial-browser]인 [Netscape Navigator](https://en.wikipedia.org/wiki/Netscape_Navigator)(그림 7 참조)를 1994년에 개발했습니다. 이에 위협을 느낀 [마이크로소프트][internet-tidal-wave]는 1995년 Internet Explorer(그림 8 참조)를 출시했으며, 곧 Windows 95에 이를 번들로 포함시켰습니다.

[internet-tidal-wave]: https://lettersofnote.com/2011/07/22/the-internet-tidal-wave/

::: {.center}
![그림 5: ViolaWWW. ([*Viola in a Nutshell*][violawww-book].)](im/violawww.png)
:::

::: {.center}
![그림 6: Mosaic. ([Wikipedia][wiki-mosaic], [CC0 1.0][cc0-1].)](im/mosaic.png)
:::

::: {.center}
![그림 7: Netscape Navigator 1.22. ([Wikipedia][wiki-netscape].)](im/netscape.png)
:::

::: {.center}
![그림 8: Internet Explorer 1.0. ([Wikipedia][wiki-ie], [Microsoft의 허가][ms-permission]를 받아 사용됨.)](im/ie1.png)
:::

[violawww-book]: https://web.archive.org/web/20200706084621/http://viola.org/viola/book/preface.html
[wiki-mosaic]: https://commons.wikimedia.org/wiki/File:NCSA_Mosaic_Browser_Screenshot.png
[cc0-1]: https://creativecommons.org/publicdomain/zero/1.0/legalcode
[wiki-netscape]: https://en.wikipedia.org/wiki/File:Navigator_1-22.png#filehistory
[wiki-ie]: https://en.wikipedia.org/wiki/File:Internet_Explorer_1.0.png
[ms-permission]: https://www.microsoft.com/en-us/legal/copyright/permissions

["첫 번째 브라우저 전쟁"](https://en.wikipedia.org/wiki/Browser_wars#First_Browser_War_(1995%E2%80%932001))의 시대가 시작되었습니다. 이 전쟁은 Netscape Navigator와 [Internet Explorer] 간의 경쟁을 뜻합니다. 시장 점유율이 작았던 다른 브라우저들도 있었는데, 그중 주목할 만한 예가 [Opera](https://en.wikipedia.org/wiki/Opera_(web_browser))입니다. [WebKit](https://en.wikipedia.org/wiki/WebKit) 프로젝트는 1999년에 시작되었으며, [Safari](https://en.wikipedia.org/wiki/Safari_(web_browser))와 [Chromium](https://www.chromium.org/) 기반 브라우저(예: Chrome 및 최신 버전의 [Edge](https://en.wikipedia.org/wiki/Microsoft_Edge))는 이 코드베이스에서 파생되었습니다. 마찬가지로 [Gecko](https://en.wikipedia.org/wiki/Gecko_(software)) 렌더링 엔진\index{rendering engine}은 1997년에 Netscape에서 처음 개발되었으며, [Firefox](https://en.wikipedia.org/wiki/Firefox) 브라우저는 이 코드베이스에서 파생되었습니다. 첫 번째 브라우저 전쟁 동안 이 책의 간단한 브라우저의 핵심 기능들—CSS, DOM, JavaScript—이 거의 모두 도입되었습니다.

[Internet Explorer]: https://en.wikipedia.org/wiki/Internet_Explorer

[2004–2017](https://en.wikipedia.org/wiki/Browser_wars#Second_Browser_War_(2004%E2%80%932017))로 기술된 "두 번째 브라우저 전쟁"은 다양한 브라우저들 간의 경쟁으로, 특히 Internet Explorer, Firefox, Safari, Chrome이 주요 경쟁자였습니다. 처음에는 Safari와 Chrome이 동일한 렌더링 엔진을 사용했으나, Chrome은 2013년에 [Blink](https://en.wikipedia.org/wiki/Blink_(browser_engine))으로 포크 되었으며, Microsoft Edge는 2020년까지 이를 채택했습니다. 두 번째 브라우저 전쟁 기간 동안, AJAX[^ajax], `<canvas>`와 같은 HTML5 기능, 그리고 서드파티 JavaScript 라이브러리 및 프레임워크의 폭발적인 증가를 포함한 현대 웹의 많은 기능들이 개발되었습니다.

[^ajax]: Asynchronous JavaScript and XML, where XML stands for eXtensible Markup Language.
[^commercial-browser]: 상업용이라는 것은 이윤을 추구하는 기업에 의해 개발되었음을 의미합니다. 초기 Netscape 버전은 또한 무료 소프트웨어가 아니었으며, 상점에서 구매해야 했습니다. 가격은 약 50달러였습니다.

표준화 시대의 도래  
=============

이와 병행하여 또 다른, 똑같이 중요한 발전이 있었는데, 그것은 바로 **웹 API의 표준화**입니다. 1994년 10월, [World Wide Web Consortium(W3C)](https://www.w3.org/Consortium/facts)\index{W3C}이 설립되어 웹 기능에 대한 감독과 표준을 제공하기 시작했습니다. 그 이전까지는 브라우저들이 새로운 HTML 요소나 API를 소개하면, 경쟁 브라우저들이 이를 복사하는 방식이 일반적이었습니다. 그러나 표준화 기구가 생기면서, 이러한 요소들과 API는 합의되고 명세서에 문서화될 수 있게 되었습니다. (오늘날에는, 새로운 기능이 추가되기 전에 초기 논의, 설계, 명세 작성이 선행됩니다.) 이후 HTML 명세는 [WHATWG](https://whatwg.org/)\index{WHATWG}라는 다른 표준화 기구로 이전되었지만, [CSS](https://drafts.csswg.org/)나 기타 기능들은 여전히 W3C에서 표준화되고 있습니다. JavaScript\index{JavaScript}는 또 다른 표준화 기구인 [ECMA](https://www.ecma-international.org/about-ecma/history/)의 [TC39](https://tc39.es/)\index{TC39}(Technical Committee 39)에서 표준화됩니다. [HTTP](https://tools.ietf.org/html/rfc2616)는 [IETF](https://www.ietf.org/about/)\index{IETF}에 의해 표준화됩니다. 중요한 점은, 1990년대 중반에 마련된 표준화 과정이 현재까지 계속 이어지고 있다는 것입니다.

웹이 등장한 초기 몇 년간, 브라우저가 표준을 유지할 것인지 혹은 특정 브라우저가 "승리"하여 또 다른 독점 소프트웨어 플랫폼이 될 것인지가 명확하지 않았습니다. 그러나 이것이 일어나지 않은 데는 여러 가지 이유가 있습니다. 그중에는 컴퓨팅 커뮤니티의 평등주의적 에토스와 W3C의 존재와 강점이 있었습니다. 또 다른 중요한 이유는 웹의 네트워크 특성 덕분에, 웹 개발자들이 대부분의 브라우저에서 페이지가 올바르게 작동하도록 해야 했기 때문입니다. 그렇지 않으면 고객을 잃게 되었기 때문에 독점 확장을 피하려는 경향이 있었습니다. 반면에 브라우저들은 웹 전체를 계속 지원하기 위해 서로의 문서화되지 않은 동작들—심지어 버그까지도—신중하게 재현하려고 노력했습니다.

브라우저가 표준을 무시하고 독자적인 길을 가려는 시도는 실제로 없었습니다. 이는 그러한 일이 발생할 수 있다는 우려에도 불구하고 이루어지지 않았습니다.[^dhtml] 대신, 시장 점유율을 위한 치열한 경쟁은 매우 빠른 혁신과 끊임없이 확장되는 웹 API 및 기능 세트 개발로 이어졌습니다. 오늘날 우리는 이를 단순히 "월드 와이드 웹"이 아니라 *웹 플랫폼*이라고 부릅니다. 이는 웹이 더 이상 문서 뷰잉 메커니즘이 아니라 완전히 실현된 컴퓨팅 플랫폼이자 생태계로 진화했음을 인정하는 것입니다.[^web-os]

[^dhtml]: 아마도 웹이 분열될 뻔한 가장 가까운 사례는 1990년대 후반 [DHTML](https://en.wikipedia.org/wiki/Dynamic_HTML) 기능이 도입되었을 때일 것입니다—이 책에서 배울 문서 객체 모델(DOM)의 초기 버전입니다. Netscape와 Internet Explorer는 처음에는 이러한 기능을 서로 호환되지 않게 구현했으며, 공동 명세를 개발하고 브라우저들에 대한 상당한 압력 캠페인을 벌인 후에야 표준화가 이루어졌습니다. 이에 대한 자세한 이야기는 [Jay Hoffman의 글](https://css-tricks.com/chapter-7-standards/)에서 읽어볼 수 있습니다.

[^web-os]: 심지어 웹을 중심으로 구축된 운영체제들도 존재했습니다! 예로는 일부 Palm 스마트폰을 지원했던 [webOS](https://en.wikipedia.org/wiki/WebOS), 오늘날 [KaiOS](https://en.wikipedia.org/wiki/KaiOS) 기반 폰에서 이어지는 [Firefox OS](https://en.wikipedia.org/wiki/Firefox_OS), 그리고 데스크톱 운영체제인 [ChromeOS](https://en.wikipedia.org/wiki/Chrome_OS)가 있습니다. 이 모든 운영체제는 웹을 모든 애플리케이션의 UI 레이어로 사용하며, 시스템 통합을 위해 JavaScript로 노출된 API를 추가합니다.

결과적으로—다수의 경쟁 브라우저와 잘 개발된 표준이 생겼기에—각 브라우저 "전쟁"에서 어느 브라우저가 "승리"했는지 또는 "패배"했는지는 그리 중요하지 않게 되었습니다. 각각의 경우에서 _웹_이 승리했습니다. 사용자가 늘어나고 기능이 확장되었기 때문입니다.


공개 소스의 역할
=============

**두 번째** 브라우저 전쟁의 또 다른 중요하고 흥미로운 결과는 오늘날의 모든 주류 브라우저들이[^examples-of-browsers-today] **세 가지 오픈 소스 웹 렌더링/JavaScript\index{JavaScript} 엔진**(Chromium, Gecko, WebKit)에 기반을 두고 있다는 점입니다.[^javascript-repo] Chromium과 WebKit은 공통의 조상 코드베이스를 공유하며, Gecko는 Netscape의 오픈 소스 후손으로, 이 세 가지 모두는 1990년대, 즉 웹의 초기 시기로 거슬러 올라갑니다.

[^examples-of-browsers-today]: Chromium 기반 브라우저의 예로는 Chrome, Edge, Opera(2013년에 [Presto](https://en.wikipedia.org/wiki/Presto_(browser_engine)) 엔진에서 Chromium으로 전환됨), Samsung Internet, Yandex Browser, UC Browser, Brave가 있습니다. 또한, 자동차, 휴대폰, TV, 기타 전자 기기 등 다양한 장치에서 세 가지 엔진 중 하나를 기반으로 한 "임베디드" 브라우저들이 많이 존재합니다.

[^javascript-repo]: JavaScript 엔진은 실제로 별도의 저장소(및 기타 하위 구성 요소들)에서 관리되며, 브라우저 외부에서도 JavaScript 가상 머신으로 사용됩니다. 중요한 응용 사례 중 하나는 [V8](https://en.wikipedia.org/wiki/V8_(JavaScript_engine))을 사용해 [node.js](https://en.wikipedia.org/wiki/Node.js)를 구동하는 것입니다. 하지만 각 렌더링 엔진에는 해당하는 JavaScript 구현이 있으므로 두 개념을 혼용해서 사용하는 것도 합리적입니다.

이는 우연이 아니며, 사실 플랫폼 API의 표준적인 집합을 기반으로 렌더링 엔진을 구현하는 가장 비용 효율적인 방식에 대해 흥미로운 점을 알려줍니다. 예를 들어, 브라우저를 명목상 통제하는 회사의 고용을 받지 않은 독립 개발자들이 코드와 기능을 기여하는 것은 흔한 일입니다. 실제로 브라우저 기능 구현을 전문으로 하는 회사와 개인들도 있습니다. 한 브라우저의 기능이 다른 브라우저에서 코드를 복제하는 경우도 흔히 있습니다. 그리고 주요 브라우저들이 모두 오픈 소스라는 점은 표준화 과정에 긍정적인 영향을 미치며, 웹의 탈중앙화를 강화합니다.

결론
=======

요약하면, 웹의 역사는 아래와 같이 전개되었습니다:

1. 정보를 표현하고 탐색하기 위한 기초 연구 수행.
2. 필요한 기술 성숙 이후 웹의 제안과 구현.
3. 웹이 대중적으로 빠르게 확산되며, 다양한 브라우저가 등장.
4. 표준화 기구가 도입되어 브라우저 간 조율 및 독점 방지.
5. 브라우저 경쟁을 통해 빠른 기능 혁신과 복잡도 증가.
6. 모든 장치(데스크톱, 모바일, 임베디드 등)로 브라우저 확산.
7. 결국, 모든 주요 렌더링 엔진의 오픈 소스화.

웹은 비약적인 발전을 이루어왔지만, 아직 끝나지 않았습니다. 미래에는 무엇이 기다리고 있을까요?

Exercises
=========

**iii-1** *앞으로의 변화는?*  
이 장에서 배운 내용을 바탕으로, 웹의 미래 발전 방향에 대해 어떤 흐름을 예측할 수 있을까요? 예를 들어, 웹이 다른 비웹 기술 및 플랫폼과 효과적으로 경쟁할 수 있을 것이라고 생각하나요?

**iii-2** *원래의 아이디어는 어떻게 되었는가?*  
웹이 실현한 방식은 Memex의 개념과 상당히 다릅니다. 한 가지 주요 차이점으로는 사용자가 웹에서 직접 페이지 간 링크를 추가하거나 주석을 달 수 있는 기본 제공 방식이 없다는 것입니다. 왜 이런 차이가 발생했다고 생각하시나요? 추가로, 초기 연구에서 제안되었으나 아직 구현되지 않은 목표가 떠오르나요?  

