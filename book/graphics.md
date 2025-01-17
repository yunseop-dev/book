---
title: 화면에 그리기
chapter: 2
prev: http
next: text
...

웹 브라우저는 단순히 웹 페이지를 다운로드하는 것만이 아닙니다. 사용자가 그 페이지를 볼 수 있도록 화면에 표시해야 합니다. 21세기에는 이것이 그래픽 애플리케이션을 의미합니다.[^text-browsers] 그래서 이 장에서는 브라우저에 그래픽 사용자 인터페이스를 추가할 것입니다.

[^text-browsers]: 텍스트 기반 브라우저도 몇 가지 존재합니다. 예를 들어, 저는 2011년 대부분의 시간 동안 `w3m`을 메인 브라우저로 사용했지만, 지금은 더 이상 사용하지 않습니다.

창 생성하기
================

데스크톱과 랩톱 컴퓨터는 창, 버튼, 마우스를 제공하는 *데스크톱 환경*을 지원하는 운영 체제를 실행합니다. 따라서 책임은 다음과 같이 분리됩니다:

- 프로그램은 새 창을 요청하고, 데스크톱 환경이 실제로 이를 화면에 표시합니다.
- 프로그램은 창에 그리기를 하고, 데스크톱 환경이 이를 화면에 반영합니다.
- 데스크톱 환경은 프로그램에 클릭과 키 입력을 전달하고, 프로그램은 이에 응답하여 창을 다시 그립니다.

이 모든 작업을 수동으로 처리하는 것은 번거로우므로, 프로그램은 보통 이러한 단계를 단순화하기 위해 *그래픽 툴킷*을 사용합니다. Python은 `tkinter`라는 Python 패키지에 포함된 Tk\index{Tk}라는 툴킷을 기본적으로 제공합니다.[^tcl]\index{Tkinter} 이것을 사용하는 것은 상당히 간단합니다:

[^tcl]: 이 라이브러리는 Tk라고 불리며, 원래 Tcl이라는 다른 언어를 위해 작성되었습니다. Python은 이를 인터페이스 형태로 포함하고 있으므로 이름이 이렇게 붙었습니다.

``` {.python expected=False}
import tkinter
window = tkinter.Tk()
tkinter.mainloop()
```

여기서 `tkinter.Tk()`는 데스크톱 환경에 새 창을 생성하도록 요청하며, 창에 그리기를 할 수 있는 객체를 반환합니다. `tkinter.mainloop()` 호출은 다음과 같은 루프를 실행합니다:[^infinite-loop]

[^infinite-loop]: 이 의사 코드가 컴퓨터를 멈추게 하는 무한 루프처럼 보일 수 있지만, 그렇지 않습니다. 운영 체제가 스레드와 프로세스 간 멀티태스킹을 수행하거나, `pendingEvents` 호출이 이벤트가 있을 때까지 대기하거나 혹은 두 가지를 모두 수행합니다. 따라서 다른 코드가 실행되어 이벤트를 생성하고, 이 루프가 이에 응답합니다.

<a name="eventloop"></a>

``` {.python .example}
while True:
    for evt in pendingEvents():
        handleEvent(evt)
    drawScreen()
```

::: {.center}
Figure 1: 이벤트 처리 사이클의 플로우차트.
:::

여기서 `pendingEvents`는 먼저 데스크톱 환경에 최근의 마우스 클릭이나 키 입력을 요청합니다. 그런 다음 `handleEvent`는 애플리케이션을 호출하여 상태를 업데이트하며, 마지막으로 `drawScreen`이 창을 다시 그립니다. 이 *이벤트 루프*\index{event loop} 패턴(그림 1 참조)은 웹 브라우저에서 비디오 게임에 이르기까지 복잡한 그래픽 애플리케이션에서 공통적입니다. 이는 모든 이벤트가 결국 처리되고 화면이 갱신되도록 보장하기 때문입니다.

::: {.further}
당신이 자신의 브라우저를 데스크톱 컴퓨터에서 작성하고 있을 가능성이 높지만, 많은 사람들이 전화기나 태블릿과 같은 모바일 기기를 통해 웹에 접근합니다. 모바일 기기에서도 화면, 렌더링 루프, 그리고 이 책에서 다루는 대부분의 것들이 여전히 존재합니다.[^same-code-on-mobile]

하지만 몇 가지 주목할 만한 차이점이 있습니다. 애플리케이션은 보통 전체 화면으로 실행되며, 한 번에 단일 애플리케이션만 화면에 표시됩니다. 마우스가 없고 가상 키보드만 제공되므로 주요 상호작용 형태는 터치입니다. "시각적 뷰포트"라는 개념이 등장하며, 이는 데스크톱에는 없지만 "데스크톱 전용" 사이트와 "모바일 대응" 사이트를 수용하기 위해 존재합니다. 또한 핀치 줌 등이 가능합니다.[^meta-viewport] 화면 픽셀 밀도는 훨씬 높지만 전체 화면 해상도는 보통 더 낮습니다. 이러한 차이를 지원하는 것은 가능하지만 상당히 많은 작업을 요구합니다. 이 책에서는 이와 관련된 구현에 대해서는 별도의 연습 문제를 제외하고는 다루지 않을 것입니다.

또한 전력 효율성이 더 중요합니다. 기기가 배터리로 작동하고, 중앙 처리 장치(CPU)와 메모리가 훨씬 느리고 성능이 낮기 때문입니다. 따라서 그래픽 처리 장치(GPU)를 최대한 활용하는 것이 매우 중요하며, 느린 CPU로 인해 우수한 성능을 달성하기가 더욱 어렵습니다. 모바일 브라우저는 도전 과제입니다!
:::

[^same-code-on-mobile]: 예를 들어, 실제 브라우저의 대부분은 데스크톱과 모바일 버전을 모두 가지고 있으며, 렌더링 엔진 코드는 거의 동일합니다.

[^meta-viewport]: [이 장의 웹 페이지](https://browser.engineering/graphics.html)의 소스를 살펴보세요. `<head>` 부분에 "viewport" `<meta>` 태그를 볼 수 있습니다. 이 태그는 브라우저에 해당 페이지가 모바일 기기도 지원한다고 알립니다. 이 태그가 없으면 브라우저는 사이트가 "데스크톱 전용"이라고 가정하고 이를 다르게 렌더링합니다. 예를 들어, 사용자가 핀치-줌 또는 더블 탭 제스처를 사용하여 페이지의 특정 부분을 확대할 수 있도록 허용합니다. 확대되면 화면에 보이는 페이지의 일부는 "시각적 뷰포트"이고, 문서 전체의 경계는 "레이아웃 뷰포트"가 됩니다. 이는 보통 데스크톱에서는 없는 방식으로 스크롤링과 줌을 혼합한 것입니다.

창에 그리기
=====================

우리의 브라우저는 웹 페이지 텍스트를 *캔버스*\index{canvas}에 그릴 것입니다. 이는 원, 선, 텍스트 등을 그릴 수 있는 직사각형 Tk 위젯입니다. 예를 들어, 다음과 같이 Tk를 사용하여 캔버스를 생성할 수 있습니다:[^canvas]

[^canvas]: HTML의 `<canvas>` 요소에 익숙한 분이라면 이것이 비슷한 개념임을 알 수 있습니다. 이는 이차원 직사각형 안에 도형을 그릴 수 있도록 합니다.

```
window = tkinter.Tk()
canvas = tkinter.Canvas(window, width=800, height=600)
canvas.pack()
```

첫 번째 줄은 창을 생성하고, 두 번째 줄은 그 창 내부에 `Canvas`를 생성합니다. Tk가 캔버스를 어디에 표시할지 알 수 있도록, 창 객체를 인자로 전달합니다. 다른 인자들은 캔버스 크기를 정의합니다. 저는 800 × 600을 선택했는데, 이는 옛날에 흔히 사용되던 모니터 크기였기 때문입니다.[^svga] 세 번째 줄은 Tk의 특이점으로, 캔버스를 창 안에 배치합니다. Tk에는 버튼이나 대화 상자 같은 위젯도 있지만, 우리의 브라우저는 이를 사용하지 않을 것입니다. 우리는 캔버스가 제공하는 더 세밀한 외형 제어가 필요하기 때문입니다.[^widgets]

[^widgets]: 데스크톱 애플리케이션이 웹 페이지보다 더 균일한 이유는 데스크톱 애플리케이션이 일반적으로 공통 그래픽 툴킷에서 제공하는 위젯을 사용하기 때문인데, 이는 애플리케이션을 비슷한 외형으로 만듭니다.

[^svga]: 이 크기는 Super Video Graphics Array(SVGA)라고 불리며, 1987년에 표준화되었습니다. 당시에는 꽤 "슈퍼"하게 보였을 것입니다.

이 코드를 정리하기 위해 클래스를 사용해보겠습니다:

``` {.python}
WIDTH, HEIGHT = 800, 600

class Browser:
    def __init__(self):
        self.window = tkinter.Tk()
        self.canvas = tkinter.Canvas(
            self.window, 
            width=WIDTH,
            height=HEIGHT
        )
        self.canvas.pack()
```

캔버스를 생성한 후에는, 그 위에 도형을 그리는 메서드를 호출할 수 있습니다. 이를 새로운 `Browser` 클래스의 `load` 메서드 안에서 해보겠습니다:

``` {.python expected=False}
class Browser:
    def load(self, url):
        # ...
        self.canvas.create_rectangle(10, 20, 400, 300)
        self.canvas.create_oval(100, 100, 150, 150)
        self.canvas.create_text(200, 150, text="Hi!")
```

이 코드를 실행하려면, `Browser`를 생성하고, `load`를 호출한 다음 Tk의 `mainloop`를 시작하세요:

```
if __name__ == "__main__":
    import sys
    Browser().load(URL(sys.argv))
    tkinter.mainloop()
```

결과적으로 캔버스의 왼쪽 위 모서리 근처에서 시작하여 중앙에서 끝나는 직사각형, 그 직사각형 내부의 원, 그리고 원 옆에 "Hi!"라는 텍스트를 볼 수 있습니다. (그림 2 참조)

::: {.center}
![그림 2: 직사각형, 원, 텍스트가 포함된 예상 출력 결과.](im/graphics-example.png)
:::

Tk의 좌표는 *x* 위치는 왼쪽에서 오른쪽으로, *y* 위치는 위에서 아래로 참조됩니다. 즉, 화면 하단 부분의 *y* 값이 더 *크다*는 것을 의미하며, 이는 수학에서 익숙한 것과는 반대일 수 있습니다. 위 좌표 값을 조작해보며 각각의 인자가 무엇을 의미하는지 파악해보세요.[^tkdocs]

[^tkdocs]: [온라인 문서][tkdocs]에서 답을 확인할 수 있습니다.

[tkdocs]: https://anzeljg.github.io/rin2/book2/2405/docs/tkinter/canvas.html

::: {.further}
Tk의 캔버스 위젯은 여기서 사용하는 것보다 훨씬 더 강력합니다. [튜토리얼](https://tkdocs.com/tutorial/canvas.html)에서 볼 수 있듯이, 캔버스에 그린 각 개체를 이동하거나, 각 개체의 클릭 이벤트를 감지하는 등 다양한 기능을 사용할 수 있습니다. 이 책에서는 그러한 기능을 사용하지 않을 것입니다. 대신, 이러한 기능을 스스로 구현하는 방법을 배우는 데 초점을 맞추고 있습니다.
:::

텍스트 배치하기
===============

이번에는 이 캔버스 위에 간단한 웹 페이지를 그려보겠습니다. 지금까지 브라우저는 웹 페이지 소스 코드를 문자(character) 단위로 읽어 들여 태그는 빼고 텍스트만 콘솔 창에 출력했습니다. 이제는 그 문자를 캔버스 위에 그리려고 합니다.

먼저, 이전 장의 `show` 함수를 `lex`[^foreshadowing]라는 함수로 바꾸겠습니다. 이 함수는 HTML 문서의 텍스트 내용을 반환만 하고 출력은 하지 않습니다.

[^foreshadowing]: 앞으로의 발전을 암시합니다...

``` {.python}
def lex(body):
    text = ""
    # ...
    for c in body:
        # ...
        elif not in_tag:
            text += c
    return text
```

그런 다음 `load` 메서드가 해당 텍스트를 문자 단위로 캔버스에 그리게 해보겠습니다:

``` {.python expected=False}
def load(self, url):
    # ...
    for c in text:
        self.canvas.create_text(100, 100, text=c)
```

이제 실제 웹 페이지에서 테스트해보겠습니다. 조금 이상하게 느껴질 수도 있겠지만,[^delay] 중국 고전 소설 *서유기*의 첫 번째 장[^instructions]을 테스트 페이지로 사용해 보겠습니다. 이 URL을 `request`, `lex`, `load` 함수에 통과시키면, 창의 왼쪽 상단에서 조금 떨어진 지점에 검은색 픽셀 덩어리가 나타날 것입니다.

[^delay]: 기본 타이포그래피에 대해서는 다음 장에서 논의하기 위해 잠시 보류합니다.

[^instructions]:
    링크를 마우스 오른쪽 버튼으로 클릭하고 "URL 복사"를 선택하세요.

왜 텍스트가 아니라 픽셀 덩어리로 보일까요? 당연히, 모든 문자를 동일한 위치에 그렸기 때문입니다. 따라서 겹쳐져 있는 것입니다! 이를 수정해보겠습니다:

``` {.python expected=False}
HSTEP, VSTEP = 13, 18
cursor_x, cursor_y = HSTEP, VSTEP
for c in text:
    self.canvas.create_text(cursor_x, cursor_y, text=c)
    cursor_x += HSTEP
```

`cursor_x`와 `cursor_y` 변수는 다음 문자가 어디에 그려질지를 나타냅니다. 마치 워드 프로세서에 텍스트를 입력하는 것처럼 말이죠. 저는 13과 18이라는 값을 여러 번 테스트해보고 읽기 좋은 값으로 선택했습니다.[^font-metrics]

[^font-metrics]: [3장](text.md)에서 이 마법 같은 숫자들은 폰트 메트릭스(Font Metrics)로 대체될 것입니다.

텍스트는 이제 왼쪽에서 오른쪽으로 한 줄을 형성합니다. 하지만 800픽셀 너비의 캔버스에서 문자당 13픽셀을 사용하면, 한 줄에 약 60자만 들어갈 수 있습니다. 소설을 읽기 위해서는 이보다 많은 텍스트가 필요하므로, 화면 가장자리에 도달하면 텍스트를 *줄바꿈*해야 합니다:

``` {.python indent=8}
for c in text:
    # ...
    if cursor_x >= WIDTH - HSTEP:
        cursor_y += VSTEP
        cursor_x = HSTEP
```

이 코드는 `cursor_x`가 787픽셀을 초과하면 `cursor_y`를 증가시키고 `cursor_x`를 리셋합니다.[^not-800] 이 동작 흐름은 그림 3에 나와 있습니다. 텍스트를 이와 같이 줄바꿈하면 한 줄 이상을 읽을 수 있게 됩니다.

::: {.print-only}
![그림 3: 문자가 그려질 때 커서가 이동하는 흐름에 대한 플로우차트.](im/graphics-cursor.png)
:::

[^crlf]: 과거 타자기에서 *y* 값을 증가시키는 것은 새 줄을 삽입하는 것을 의미했고, *x* 값을 리셋하는 것은 활자(carriage)를 페이지 왼쪽 가장자리로 "되돌리는(return)" 것을 의미했습니다. 그래서 미국 표준 코드(ASCII)는 "캐리지 리턴(carriage return)"과 "줄 바꿈(line feed)" 두 가지 문자를 표준화했으며, 이는 당시 전신 타자기에서 직접 실행할 수 있도록 설계되었습니다. 이 때문에 HTTP 헤더는 현대 컴퓨터가 기계적인 활자를 사용하지 않음에도 불구하고 `\r\n`으로 구분됩니다.

[ascii]: https://en.wikipedia.org/wiki/ASCII

[^not-800]: 시작 지점을 픽셀 13으로 설정했으며 양쪽에 균등한 간격을 남기고 싶기 때문입니다.

이제 [우리의 예제 페이지][ex-monkey]를 브라우저에서 로드하면 그림 4와 유사하게 보일 것입니다.

::: {.center}
![그림 4: 브라우저 내부에서 렌더링된 *서유기* 첫 장의 모습.](examples/example2-text-screenshot.png)
:::

[ex-monkey]: https://browser.engineering/examples/xiyouji.html

이제 많은 텍스트를 읽을 수 있게 되었지만 여전히 모든 텍스트를 다 볼 수는 없습니다. 텍스트가 충분히 길다면 일부 줄은 화면에 표시되지 않을 것입니다. 사용자가 페이지의 다른 부분을 볼 수 있도록 페이지를 *스크롤*\index{scroll}할 수 있도록 해야 합니다.

::: {.further}
영어 텍스트에서는 단어 중간에서 줄바꿈을 하면 안 되지만(적어도 하이픈 없이), 중국어에서는 기본적으로 단어 중간에서 줄바꿈이 가능합니다. 예를 들어, <span lang="zh">开关</span>은 "스위치"를 의미하며 <span lang="zh">开</span>("켜기")와 <span lang="zh">关</span>("끄기")로 이루어져 있습니다. 하지만 <span lang="zh">开</span> 뒤에서 줄바꿈을 해도 문제가 없습니다.

CSS의 `word-break` 속성을 사용해 기본 동작을 변경할 수 있습니다. `break-all`은 어디서든 줄바꿈을 허용하고, `auto-phrase`는 중국어나 일본어에서 단어 또는 구문 내부에서 줄바꿈을 방지합니다. 여기서 "auto"는 단어가 작성자에 의해 명시되지 않고 자동으로 감지된다는 것을 의미합니다. 이는 종종 [동적 프로그래밍][icu-wb]과 [단어 빈도 테이블][cjdict]을 기반으로 이루어집니다.
:::

[icu]: https://icu.unicode.org/
[icu-wb]: https://unicode-org.github.io/icu/userguide/boundaryanalysis/break-rules.html#details-about-dictionary-based-break-iteration
[cjdict]: https://github.com/unicode-org/icu/blob/master/icu4c/source/data/brkitr/dictionaries/cjdict.txt
[word-break-css]: https://www.w3.org/TR/css-text-4/#word-break-property
[chinese-line-breaking]: https://en.wikipedia.org/wiki/Line_breaking_rules_in_East_Asian_languages

텍스트 스크롤하기
==============

스크롤은 페이지 좌표(예: "이 텍스트는 *페이지*의 위에서 132픽셀 떨어져 있음")와 화면 좌표(예: "스크롤을 60픽셀 아래로 내려서 이 텍스트는 *화면*의 위에서 72픽셀 떨어져 있음") 사이에 간접적인 레이어를 추가합니다—그림 5 참조. 일반적으로 브라우저는 페이지를 *배치*합니다(layout)—페이지의 모든 요소가 어디에 있을지를 결정함—이는 페이지 좌표를 기준으로 이루어집니다. 그리고 페이지를 *래스터화*합니다(raster)—모든 것을 그림—이는 화면 좌표를 기준으로 이루어집니다.[^screen-coordinates]

[^screen-coordinates]: 정확히는 페이지가 먼저 비트맵이나 GPU 텍스처로 그려지고, 그런 다음 스크롤에 따라 해당 비트맵/텍스처가 이동되며, 최종 결과가 화면에 렌더링됩니다. 이에 대해서는 [11장](visual-effects.md)에서 자세히 다룹니다.

![그림 5: 페이지 좌표와 화면 좌표의 차이.](im/graphics-coords-2.gif)

우리 브라우저도 동일한 구조를 따를 것입니다. 현재 `load` 메서드는 문자 각각의 위치를 계산하고 그것을 그립니다. 즉, **배치**\index{layout}와 **렌더링**\index{rendering}을 모두 수행합니다. 이제는 각 문자의 위치를 계산하고 저장하는 `layout` 함수와, 저장된 위치를 기반으로 각 문자를 그리는 별도의 `draw` 함수로 나눌 것입니다. 이를 통해 `layout`은 페이지 좌표로 작업하고, 화면 좌표는 오직 `draw`만 신경 쓰면 됩니다.

우선 `layout`을 구현해 보겠습니다. 문자마다 `canvas.create_text`를 호출하는 대신, 위치와 함께 리스트에 추가하도록 하겠습니다. `layout`은 `Browser` 클래스에 접근하지 않아도 되므로 독립적인 함수로 작성할 수 있습니다:

``` {.python}
def layout(text):
    display_list = []
    cursor_x, cursor_y = HSTEP, VSTEP
    for c in text:
        display_list.append((cursor_x, cursor_y, c))
        # ...
    return display_list
```

이렇게 생성된 리스트는 *디스플레이 리스트*라고 불립니다.^[디스플레이 리스트라는 용어는 표준입니다.] `layout`은 페이지 좌표를 기준으로 작동하므로, 스크롤을 지원하기 위해 다른 변경은 필요하지 않습니다.

디스플레이 리스트가 계산되면, `draw` 함수가 이를 반복하며 각 문자를 그려야 합니다. `draw`는 캔버스에 접근해야 하므로 `Browser` 클래스의 메서드로 작성합니다:

``` {.python expected=False}
class Browser:
    def draw(self):
        for x, y, c in self.display_list:
            self.canvas.create_text(x, y, text=c)
```

이제 `load` 메서드는 `layout`을 호출한 다음 `draw`를 호출하도록 수정합니다:

``` {.python}
class Browser:
    def load(self, url):
        body = url.request()
        text = lex(body)
        self.display_list = layout(text)
        self.draw()
```

이제 스크롤 기능을 추가해 보겠습니다. 먼저 스크롤 정도를 저장할 필드를 추가합니다:

``` {.python}
class Browser:
    def __init__(self):
        # ...
        self.scroll = 0
```

페이지 좌표 `y`는 이제 화면 좌표 `y - self.scroll`이 됩니다:

``` {.python}
def draw(self):
    for x, y, c in self.display_list:
        self.canvas.create_text(x, y - self.scroll, text=c)
```

`scroll` 값을 변경하면 페이지가 위아래로 스크롤됩니다. 그러나 *사용자*는 어떻게 `scroll` 값을 변경할 수 있을까요?

대부분의 브라우저는 위/아래 화살표 키를 누르거나, 스크롤 휠을 돌리거나, 스크롤 바를 드래그하거나, 화면에 터치 제스처를 실행하면 페이지를 스크롤합니다. 여기에서는 간단히 아래쪽 화살표 키만 구현해 보겠습니다.

Tk는 키에 *함수 바인딩*을 허용하여, 키가 눌릴 때 해당 함수를 호출하도록 지시할 수 있습니다. 예를 들어, 아래쪽 화살표 키에 바인딩하려면 다음과 같이 작성합니다:

``` {.python}
def __init__(self):
    # ...
    self.window.bind("<Down>", self.scrolldown)
```

여기서 `self.scrolldown`은 *이벤트 핸들러*로, 아래쪽 화살표 키가 눌릴 때 Tk가 호출하는 함수입니다.[^event-arg] 이 함수는 단순히 `scroll`을 증가시키고 캔버스를 다시 그리면 됩니다:

[^event-arg]: Tk는 `scrolldown`에 *이벤트 객체*를 인자로 전달합니다. 하지만 스크롤을 내리는 작업에는 키 입력 정보가 필요하지 않으므로, `scrolldown`은 해당 이벤트 객체를 무시합니다.

``` {.python}
SCROLL_STEP = 100

def scrolldown(self, e):
    self.scroll += SCROLL_STEP
    self.draw()
```

이 코드를 실행해 보면, 스크롤 시 텍스트가 두 번 그려지는 것을 알 수 있습니다. 이는 새로운 텍스트를 그리기 전에 기존 텍스트를 지우지 않았기 때문입니다. `canvas.delete`를 호출하여 이전 텍스트를 삭제하세요:

``` {.python}
def draw(self):
    self.canvas.delete("all")
    # ...
```

이제 스크롤이 정상적으로 작동해야 합니다!

::: {.further}
디스플레이 리스트를 저장하면 스크롤 속도가 빨라집니다. 브라우저가 스크롤할 때마다 `layout`을 다시 수행하지 않기 때문입니다. 현대 브라우저는 이를 [훨씬 더 발전시켰습니다][webrender]. 디스플레이 리스트의 대부분을 JavaScript나 사용자 상호작용으로 웹 페이지가 변경될 때도 유지하도록 설계되었습니다.

일반적으로 스크롤은 웹 페이지에서 가장 일반적인 사용자 상호작용입니다. 그래서 실제 브라우저는 스크롤을 빠르게 만들기 위해 **엄청난** 노력을 기울였습니다. 이 책의 뒷부분에서 이 방법들 중 일부를 더 다룰 예정입니다.
:::

[webrender]: https://hacks.mozilla.org/2017/10/the-whole-web-at-maximum-fps-how-webrender-gets-rid-of-jank/

더 빠른 렌더링
================

<a name="framebudget"></a> 애플리케이션은 상호작용을 부드럽게 느끼게 하기 위해 페이지 내용을 빠르게 다시 그려야 하며,[^compositing] 클릭이나 키 입력에도 신속히 반응해야 사용자가 불만을 느끼지 않습니다. "부드럽게 느낀다"는 개념은 더 구체적으로 정의될 수 있습니다. 브라우저와 같은 그래픽 애플리케이션은 일반적으로 화면의 새로 고침 속도 또는 *프레임 속도*에 맞춰 다시 그리기를 목표로 하며, 보통 고정된 60Hz로 작동합니다.[^sixty-hertz] 이는 브라우저가 모든 작업을 1/60초(즉, 16ms) 이내에 완료해야 한다는 것을 의미하며, 이 제한을 *애니메이션 프레임 예산*(animation frame budget)이라고 합니다.

[^sixty-hertz]: 대부분의 현대 화면은 60Hz 새로 고침 속도를 가지고 있으며, 이는 보통 부드럽게 보이기에 충분하다고 간주됩니다. 하지만 점점 더 많은 새로운 하드웨어가 120Hz와 같은 더 높은 새로 고침 속도로 등장하고 있습니다. 브라우저가 이런 속도에 맞춰질 수 있을지는 아직 불분명합니다. 특히 렌더링 속도가 따라가지 못할 경우, 일부 렌더링 엔진(특히 게임)은 의도적으로 더 낮은 속도로 새로 고침을 진행합니다.

[^compositing]: 이전 시스템에서는 애플리케이션이 화면에 직접 그리고, 업데이트하지 않으면 마지막에 있던 내용이 그대로 남아 있었습니다. 이는 오류 발생 시 한 창이 다른 창 위에 "잔상(trails)"을 남기기도 했던 이유입니다. 현대 시스템은 [합성(compositing)](https://en.wikipedia.org/wiki/Compositing_window_manager)을 사용하여 이러한 잔상을 방지하고 성능과 화면 요소 간 격리를 개선합니다. 그러나 애플리케이션은 여전히 창 내용을 새로 그려야 화면에 표시되는 것을 변경할 수 있습니다. 합성에 대한 자세한 설명은 [13장](animations.md)에서 다룹니다.

하지만 현재 우리 브라우저에서 스크롤 속도는 꽤 느립니다.[^slow-scroll] 왜일까요? 이는 `create_text` 내에서 문자 자체의 모양을 로드하는 작업이 시간이 많이 걸리기 때문입니다. 스크롤 속도를 높이기 위해서는 이러한 작업을 꼭 필요할 때만 수행해야 합니다(반면, 화면에 표시되는 픽셀은 항상 정확해야 합니다).

[^slow-scroll]: 정확히 얼마나 느린지는 운영 체제와 기본 글꼴에 의해 크게 좌우됩니다.

실제 브라우저는 이 문제를 해결하기 위해 여러 복잡한 최적화를 수행합니다. 하지만 여기에서는 간단히 다음의 개선만 도입하겠습니다: 화면 밖에 있는 문자는 그리지 않습니다.

``` {.python}
for x, y, c in self.display_list:
    if y > self.scroll + HEIGHT: continue
    if y + VSTEP < self.scroll: continue
    # ...
```

첫 번째 `if` 문은 현재 화면 아래쪽에 있는 문자를 생략하고, 두 번째 `if` 문은 화면 위쪽에 있는 문자를 생략합니다. 여기서 `y + VSTEP`은 문자의 하단 경계를 나타냅니다. 이는 문자가 화면에 절반만 들어와 있는 경우에도 여전히 그려야 하기 때문입니다.

이제 스크롤 속도가 상당히 빨라졌으며, 이상적으로는 16ms 애니메이션 프레임 예산에 가까워졌을 것입니다.^[제 컴퓨터에서는 여전히 이 예산의 약 두 배 정도 시간이 걸렸지만, 이는 이후의 장에서 더 다룰 작업이 남아있음을 의미합니다.] 또한 `layout`과 `draw`를 별도로 분리했기 때문에 이번 최적화를 구현하기 위해 `layout`에는 전혀 변경이 필요하지 않았습니다.

::: {.further}
모든 웹 페이지 상호작용이 애니메이션은 아닙니다—마우스 클릭과 같은 **이산적(discrete)** 동작도 있습니다. 연구에 따르면, 이산적 동작에 대해 [100ms][100ms] 이내에 응답하는 것으로 충분하다고 합니다. 이 한계를 초과하지 않으면 대부분의 사람은 속도의 차이를 느끼지 않습니다. 이는 스크롤처럼 60Hz 이하의 속도가 눈에 띄는 상호작용과는 매우 다릅니다. 이러한 차이는 인간의 뇌가 움직임(애니메이션)과 이산적 동작을 처리하는 방식, 그리고 그러한 동작을 결정하고 실행하며 결과를 이해하는 데 걸리는 시간과 관련이 있습니다.
:::

[100ms]: https://www.nngroup.com/articles/response-times-3-important-limits/

요약
=======

이번 장에서는 초기 명령줄 브라우저에서 시작하여 스크롤 가능한 텍스트가 있는 그래픽 사용자 인터페이스로 발전시켰습니다. 이제 브라우저는 다음과 같은 기능을 수행합니다:

- 운영 체제와 통신하여 창을 생성합니다.
- 텍스트를 배치하고 해당 창에 그립니다.
- 키보드 명령을 수신합니다.
- 창을 스크롤하여 사용자 입력에 반응합니다.

::: {.web-only}

여기서는 이 웹 페이지를 렌더링한 우리의 브라우저를 선보입니다(완전히 상호작용 가능하며, 클릭하여 초점을 맞춘 뒤 아래쪽 화살표 키로 스크롤할 수 있습니다):^[이것은 전체 브라우저 소스 코드로, JavaScript로 크로스 컴파일되어 iframe에서 실행되고 있습니다. "restart"를 클릭하면 렌더링할 새로운 웹 페이지를 선택할 수 있으며, "start"를 클릭하면 렌더링이 시작됩니다. 이후 장에서는 각 장 끝에서 이 브라우저가 어떻게 개선되는지 확인할 수 있는 이와 같은 데모를 포함할 것입니다.]

::: {.widget height=400}
    lab2-browser.html
:::

:::

다음으로, 브라우저가 영어 텍스트에서 작동할 수 있도록 할 것입니다. 여기에는 가변 너비 문자, 줄 배치, 서식과 같은 복잡성을 처리하는 작업이 포함됩니다.

::: {.signup}
:::

개요
=======

우리의 브라우저에 포함된 함수, 클래스, 메서드의 전체 목록은 다음과 비슷할 것입니다:

::: {.web-only .cmd .python .outline html=True}
    python3 infra/outlines.py --html src/lab2.py --template book/outline.txt
:::

::: {.print-only .cmd .python .outline}
    python3 infra/outlines.py src/lab2.py --template book/outline.txt
:::

연습 문제
=========

## 2-1 *줄바꿈*  
`layout` 함수에서 줄바꿈 문자를 만나면 현재 줄을 끝내고 새 줄을 시작하도록 수정하세요. 새 줄을 만들 때 *y* 값을 `VSTEP`보다 크게 증가시켜 단락 구분 효과를 만드세요. *서유기*에는 시가 포함되어 있으니 이제 그것들을 더욱 명확히 구분할 수 있을 것입니다.

## 2-2 *마우스 휠*  
위쪽 화살표 키를 눌렀을 때 위로 스크롤하는 기능을 추가하세요. 페이지 상단을 넘어서 스크롤할 수 없도록 하세요. 그런 다음 마우스 휠로 스크롤하면 트리거되는 `<MouseWheel>` 이벤트를 바인딩하세요.[^laptop-mousewheel] 해당 이벤트 객체에는 스크롤 거리와 방향을 나타내는 `event.delta` 값이 포함되어 있습니다. 불행히도 macOS와 Windows는 `event.delta`의 부호와 스케일링이 다르고, Linux에서는 스크롤이 대신 `<Button-4>` 및 `<Button-5>` 이벤트를 사용합니다.[^more-mousewheel]

[^laptop-mousewheel]: 마우스가 없어도 터치패드 제스처로 트리거됩니다.
[^more-mousewheel]: [Tk 매뉴얼][tk-mousewheel]에서 이에 대한 추가 정보를 확인할 수 있습니다. 크로스 플랫폼 애플리케이션은 크로스 브라우저 애플리케이션보다 훨씬 작성하기 어렵습니다!

[tk-mousewheel]: https://wiki.tcl-lang.org/page/mousewheel

## 2-3 *크기 조정 가능하게 만들기*  
브라우저 창을 크기 조정 가능하게 만드세요. 이를 위해 `canvas.pack`에 [`fill` 및 `expand` 인자][fill-expand]를 전달하고, 창 크기가 조정될 때 발생하는 `<Configure>` 이벤트를 호출 및 바인딩하세요. 새 창의 너비와 높이는 이벤트 객체의 `width` 및 `height` 필드에 있습니다. 창 크기가 조정되면 줄바꿈도 변경되어야 하므로, `layout`을 다시 호출해야 합니다.

[fill-expand]: https://web.archive.org/web/20201111222645id_/http://effbot.org/tkinterbook/pack.htm

## 2-4 *스크롤바*  
브라우저가 마지막 디스플레이 리스트 항목을 넘어 스크롤하지 않도록 만드세요.[^not-quite-right] 화면 오른쪽 가장자리에 파란색 직사각형 스크롤바를 그리세요. 스크롤바의 크기와 위치는 브라우저가 전체 문서에서 어떤 부분을 볼 수 있는지를 반영해야 합니다(그림 5 참조). 전체 문서가 화면에 맞는 경우 스크롤바를 숨기세요.

[^not-quite-right]: 실제 브라우저에서는 정확히 이것만으로는 부족합니다. 브라우저는 화면 하단의 추가 공백 또는 화면 밖에 의도적으로 그려진 객체를 고려해야 합니다. 이를 [5장](layout.md)에서 올바르게 구현할 것입니다.

## 2-5 *이모지*  
브라우저에 이모지 `😀`{=html}`\smiley`{=latex}를 지원하도록 추가하세요. 이모지는 문자이므로 `create_text`를 호출하여 그릴 수 있지만, 결과가 좋지 않을 수 있습니다. 대신 [OpenMoji 프로젝트](https://openmoji.org)로 가서 ["활짝 웃는 얼굴"](https://openmoji.org/library/#emoji=1F600)의 이모지를 PNG 파일로 다운로드하고, 크기를 16 × 16 픽셀로 조정한 다음 브라우저와 동일한 폴더에 저장하세요. Tk의 `PhotoImage` 클래스를 사용해 이미지를 로드한 후 `create_image` 메서드를 사용해 캔버스에 그리세요. 사실 OpenMoji 전체 라이브러리를 다운로드하여("Get OpenMojis" 버튼을 상단 오른쪽에서 찾으세요) 페이지에서 사용하는 모든 이모지를 브라우저가 검색해 표시할 수 있도록 하세요.

## 2-6 *`about:blank`*  
잘못된 URL은 현재 브라우저를 충돌시킵니다. 대신 오류 복구를 추가하여 빈 페이지를 표시해 사용자가 오류를 수정할 수 있도록 만드는 것이 더 좋습니다. 이를 위해 `about:blank`라는 특별한 URL을 지원하도록 추가하세요. 이는 단순히 빈 페이지를 렌더링해야 하며, 잘못된 URL은 `about:blank`처럼 자동으로 렌더링되도록 처리하세요.

## 2-7 *대체 텍스트 방향*  
모든 언어가 좌측에서 우측으로 읽거나 배치되는 것은 아닙니다. 아랍어, 페르시아어, 히브리어는 우측에서 좌측으로 읽는 언어의 좋은 예입니다. 명령줄 플래그를 사용해 이를 위한 기본 지원을 브라우저에 구현하세요.^[이후 [4장](html.md)에 도달하면 `<body>` 요소에 [`dir`][dir-attr] 속성을 대신 사용할 수 있습니다.] 영어 문장은 여전히 좌측에서 우측으로 배치되지만, 화면 오른쪽에서부터 성장해야 합니다([이 예제][rtl-example]를 좋아하는 브라우저에서 열어보세요).^[실제 우측에서 좌측으로 읽는 언어에서는 반대로 해야 합니다. 또한 중국어나 일본어와 같은 일부 동아시아 언어에서는 세로 쓰기 모드도 있습니다.]

[dir-attr]: https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/dir

[rtl-example]: examples/example2-rtl.html