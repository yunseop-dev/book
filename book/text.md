---
title: 텍스트 서식 지정
chapter: 3
prev: 그래픽
next: HTML
---

이전 장에서 우리의 브라우저는 그래픽 창을 생성하고 그 안에 문자 그리드를 그렸습니다. 이는 중국어에는 적합하지만, 영어 텍스트는 너비가 다른 문자들로 구성되어 있으며, 단어는 줄 바꿈 중에 분리될 수 없습니다.[^lotsoflang] 이번 장에서는 이러한 기능을 추가할 것입니다. 심지어 [이 장](https://browser.engineering/text.html)을 여러분의 브라우저에서 읽을 수도 있을 것입니다!

[^lotsoflang]: 세상에는 아랍어부터 줄루어까지 다양한 언어와 타이포그래피 규칙이 존재합니다. 실제 웹 브라우저는 모든 언어를 지원하지만, 이 책은 영어에 집중합니다. 텍스트는 거의 무한히 복잡하지만, 이 책은 무한히 길 수 없습니다!

# 폰트란 무엇인가?

지금까지 우리는 `create_text`를 문자와 두 좌표와 함께 호출하여 화면에 텍스트를 작성했습니다. 하지만 폰트\index{font}, 크기 또는 스타일을 지정하지 않았습니다. 이러한 요소들에 대해 이야기하려면, 폰트 객체를 생성하고 사용해야 합니다.

폰트란 정확히 무엇일까요? 과거에는 인쇄업자들이 금속 활자를 레일에 배열하고 잉크를 묻힌 뒤 종이에 눌러 인쇄된 페이지를 만들었습니다 (그림 1 참조). 금속 활자는 글자마다 상자에 보관되었으며, 예를 들어 'e'는 큰 상자에, 'x'는 작은 상자에 보관되었습니다. 이러한 상자들은 대문자와 소문자를 구분하는 케이스(상자)로 나뉘었으며 (그림 2 참조), 이 케이스들의 집합을 폰트라고 불렀습니다.[^fontname] 당연히 더 큰 텍스트를 인쇄하려면 더 큰 금속 활자가 필요했으며, 이는 다른 폰트로 간주되었습니다. 폰트들의 집합은 *타입(type)*이라고 불렸으며, 이는 우리가 타이핑(typing)이라는 단어를 사용하는 이유입니다. 굵게(bold) 또는 기울임(italic) 같은 변형은 해당 타입의 "페이스(face)"로 불렸습니다.

[^fontname]: '폰트'라는 단어는 작은 금속 활자를 제작하던 *주조소(foundry)*와 관련이 있습니다.

::: {.center}
![그림 1: 인쇄소 작업자의 그림. (By [Daniel Nikolaus Chodowiecki][chodowiecki]. [Wikipedia][wiki-tafel], public domain.)](im/text-old.jpeg)
:::

[chodowiecki]: https://en.wikipedia.org/wiki/Daniel_Chodowiecki
[wiki-tafel]: https://commons.wikimedia.org/wiki/File:Chodowiecki_Basedow_Tafel_21_c.jpg

::: {.center}
![그림 2: 금속 활자와 조합 스틱. (By Willi Heidelbach. [Wikipedia][wiki-type], [CC BY 2.5][cc-by-25].)](im/text-metal.png)
:::

[wiki-type]: https://en.wikipedia.org/wiki/File:Metal_movable_type.jpg
[cc-by-25]: https://creativecommons.org/licenses/by/2.5/deed.en

이 명명법은 금속 활자가 상자 안에 보관되고, 주조소에서 제작되던 인쇄소의 세계를 반영합니다. 현대에서는 드롭다운 메뉴가 등장하면서 이러한 단어들이 더 이상 정확히 맞지 않습니다. 이제 "폰트"는 폰트(font), 서체(typeface), 또는 타입(type)을 의미할 수 있습니다.[^family] 현대의 폰트는 여러 가지 **두께**(예: "bold", "normal"),[^manyweight] 여러 가지 **스타일**(예: "italic", "roman"—italic이 아닌 스타일을 지칭),[^options] 그리고 임의의 **크기**[^sizes]를 포함합니다. 마치 마법 잉크의 세계에 온 것 같습니다.[^magic-ink]

[^family]: "폰트 패밀리(font family)"는 더 크거나 작은 타입의 집합을 나타낼 수도 있습니다.
[^manyweight]: 때로는 "light", "semibold", "black", "condensed" 같은 다른 두께도 포함됩니다. 좋은 폰트는 다양한 두께 옵션을 제공합니다.
[^options]: 때로는 소문자 대체(small-caps) 버전 같은 추가 옵션도 있습니다. 자동 기울임과 수동 기울임의 차이에 대해서도 논의할 수 있습니다.
[^sizes]: 특정 크기에서 폰트를 픽셀 그리드에 잘 맞추도록 하는 *힌트(hint)*가 있어야 특히 보기 좋습니다.
[^magic-ink]: 이 용어는 컴퓨터 그래픽 가능성을 활용해 더 나은 애플리케이션을 만들 수 있다고 논의한 [Bret Victor의 에세이][magic-ink-essay]에서 유래했습니다.

[magic-ink-essay]: http://worrydream.com/MagicInk/

Tk의 *폰트 객체*는 고정된 크기, 스타일 및 두께를 가진 과거의 폰트를 나타냅니다. 예를 들어:[^after-tk]

[^after-tk]: `Font` 객체나 다른 종류의 Tk 객체를 생성하려면 반드시 `tkinter.Tk()` 호출 후에 가능하며, `tkinter.font`를 별도로 가져와야 합니다.

```python
import tkinter.font

window = tkinter.Tk()

bi_times = tkinter.font.Font(
    family="Times",
    size=16,
    weight="bold",
    slant="italic",
)
```

::: {.quirk}
컴퓨터에 "Times" 폰트가 설치되어 있지 않을 수도 있습니다. 사용 가능한 폰트 목록은 `tkinter.font.families()`로 확인하고 다른 것을 선택할 수 있습니다.
:::

폰트 객체는 `create_text` 함수의 `font` 매개변수로 전달될 수 있습니다:

```python
canvas.create_text(200, 100, text="Hi!", font=bi_times)
```

::: {.further}
과거 미국 조판공들은 [캘리포니아 작업 케이스][california]라는 방식으로 금속 활자를 정리했습니다. 이 방식은 대문자와 소문자를 나란히 배치하여 조판 작업을 더 쉽게 만들었습니다. 대문자/소문자라는 명칭은 몇 세기 전부터 사용되었습니다.
:::

[california]: http://www.alembicpress.co.uk/Typecases/CJCCASE.HTM

# 텍스트 측정

텍스트는 수직 및 수평 공간을 차지하며, 폰트 객체의 `metrics`와 `measure` 메서드는 이 공간을 측정합니다:[^spacing]

```{.python .output}
>>> bi_times.metrics()
{'ascent': 15, 'descent': 4, 'linespace': 19, 'fixed': 0}
>>> bi_times.measure("Hi!")
24
```

[^spacing]:
    On your computer, you might get different numbers. That's
    right---text rendering is OS-dependent, because it is complex
    enough that everyone uses one of a few libraries to do it, usually
    libraries that ship with the OS. That's why macOS fonts tend to be
    "blurrier" than the same font on Windows: different libraries make
    different trade-offs.

`metrics` 호출은 텍스트의 수직 치수에 대한 정보를 제공합니다 (그림 3 참조):  
`linespace`는 텍스트의 전체 높이를 나타내며, 여기에는 "line" 위로 뻗는 `ascent`와 "line" 아래로 내려가는 `descent`가 포함됩니다.[^fixed]  
서로 다른 크기의 단어들이 같은 줄에 위치할 때는 `ascent`와 `descent`가 중요합니다. 이들은 텍스트의 상단이나 하단이 아니라 "줄을 따라" 정렬되어야 합니다.

[^fixed]:
    The `fixed` parameter is actually a boolean and tells you whether
    all letters are the same _width_, so it doesn't really fit here.

::: {.center}
![그림 3: 폰트의 다양한 수직 메트릭. 폰트의 모든 글리프는 동일한 어센트(ascent), *x-높이*, 디센트(descent)를 공유하며 공통 기준선에 맞춰 배치됩니다. 하지만 글리프의 너비(measure 또는 advance)는 서로 다를 수 있습니다.](im/text-metrics.png)
:::

좀 더 자세히 살펴봅시다. 기억하세요, `bi_times`는 사이즈 16의 Times 폰트입니다. 그런데 왜 `font.metrics`는 실제로 19 픽셀의 높이를 보고할까요?  
우선, 여기서 사이즈 16은 16 *points*를 의미하는데, 이는 1인치의 72분의 1로 정의되며, 16 *pixels*가 아닙니다,[^french-pts] 아마도 모니터는 1인치당 약 100픽셀 정도를 가지고 있을 것입니다.[^pt-for-fonts]  
이 16 포인트는 개별 글자를 측정하는 것이 아니라, 한때 글자를 새겼던 금속 블록의 크기를 나타내므로 실제 글자는 _16 포인트보다 작아야_ 합니다.  
실제로, 사이즈 16의 폰트들은 서로 다른 높이의 글자를 가지고 있습니다:[^varying-times]

[^french-pts]:
    Actually, the definition of a "point" is a total mess,
    with many different length units all called "point" around the
    world. The [Wikipedia page][wiki-point] has the details, but a
    traditional American/British point is actually slightly less than
    1/72 of an inch. The 1/72 standard comes from PostScript, but
    some systems predate it; `TeX`{=html} `\TeX`{=latex}, for example, hews closer to the
    traditional point, approximating it as 1/72.27 of an inch.

[wiki-point]: https://en.wikipedia.org/wiki/Point_(typography)

[^pt-for-fonts]:
    Tk doesn't use points anywhere else in its API. It's
    supposed to use pixels if you pass it a negative number, but that
    doesn't appear to work.

[^varying-times]:
    You might even notice that Times has different
    metrics in this code block than in the earlier one where we
    specified a bold, italic Times font. The bold, italic Times font
    is taller, at least on my current macOS system!

```{.python .output}
>>> tkinter.font.Font(family="Courier", size=16).metrics()
{'fixed': 1, 'ascent': 13, 'descent': 4, 'linespace': 17}
>>> tkinter.font.Font(family="Times", size=16).metrics()
{'fixed': 0, 'ascent': 14, 'descent': 4, 'linespace': 18}
>>> tkinter.font.Font(family="Helvetica", size=16).metrics()
{'fixed': 0, 'ascent': 15, 'descent': 4, 'linespace': 19}
```

`measure()` 메서드는 보다 직접적으로 텍스트가 수평으로 얼마만큼의 공간을 차지하는지를 픽셀 단위로 알려줍니다.  
이는 텍스트에 따라 달라지는데, 서로 다른 글자들이 서로 다른 너비를 가지기 때문입니다:[^widths]

```{.python .output}
>>> bi_times.measure("Hi!")
24
>>> bi_times.measure("H")
13
>>> bi_times.measure("i")
5
>>> bi_times.measure("!")
7
>>> 13 + 5 + 7
25
```

[^widths]:
    Note that the sum of the individual letters' lengths is not
    the length of the word. Tk uses fractional pixels internally, but
    rounds up to return whole pixels in the `measure` call. Plus, some
    fonts use something called _kerning_ to shift letters a little bit
    when particular pairs of letters are next to one another, or even
    _shaping_ to make two letters look one glyph.

이 정보를 사용하여 페이지에 텍스트를 배치할 수 있습니다.  
예를 들어, "Hello, world!"라는 텍스트를 두 부분으로 나누어 "world!"만 이탤릭체로 표시하고 싶다고 가정해 봅시다.  
두 개의 폰트를 사용해 보겠습니다:

```{.python .example}
font1 = tkinter.font.Font(family="Times", size=16)
font2 = tkinter.font.Font(family="Times", size=16, slant='italic')
```

이제 `(200, 200)`에서 텍스트를 배치할 수 있습니다:

```{.python .example}
x, y = 200, 200
canvas.create_text(x, y, text="Hello, ", font=font1)
x += font1.measure("Hello, ")
canvas.create_text(x, y, text="world!", font=font2)
```

"Hello,"와 "world!"가 올바르게 정렬되고 두 번째 단어가 이탤릭체로 표시되는 것을 볼 수 있을 것입니다.

불행하게도, 이 코드에는 예제 텍스트의 선택으로 인해 드러나지 않는 버그가 있습니다.  
예를 들어 "world!"를 "overlapping!"으로 바꾸면 두 단어가 겹치게 됩니다.  
이는 `create_text`에 전달하는 좌표 `x`와 `y`가 텍스트의 *중심*을 기준으로 위치를 지정하기 때문입니다.  
"Hello, world!"에서는 "Hello,"와 "world!"의 길이가 동일했기 때문에 문제가 없었던 것입니다!

다행히도, 전달하는 좌표의 의미는 설정할 수 있습니다.  
`anchor` 인자를 `"nw"`로 설정하면, 전달한 좌표를 텍스트의 좌상단(즉, "northwest" 코너)으로 취급하도록 Tk에 지시할 수 있습니다:

```{.python .example}
x, y = 200, 225
canvas.create_text(x, y, text="Hello, ", font=font1, anchor='nw')
x += font1.measure("Hello, ")
canvas.create_text(
    x, y, text="overlapping!", font=font2, anchor='nw')
```

`draw` 함수를 수정하여 `anchor`를 `"nw"`로 설정하세요.  
이전 장에서는 모든 중국어 문자가 동일한 폭을 가졌기 때문에 그럴 필요가 없었습니다.

::: {.further}
폰트 메트릭이 혼란스럽다면, 당신만 그런 것이 아닙니다! 2012년에 Michigan 대법원은 [Stand Up for Democracy v. Secretary of State][case] 사건을 심리했습니다. 이 사건은 최종적으로 투표 국민투표의 유효성을 둘러싼 사건으로, 폰트 크기의 정의에 초점을 맞추고 있었습니다. 법원은 (정확하게도) 폰트 크기는 글자가 새겨졌던 금속 블록의 크기이지, 글자 자체의 크기가 아니라고 결정했습니다.
:::

[case]: https://publicdocs.courts.mi.gov/opinions/final/sct/20120803_s145387_157_standup-op.pdf

# 단어별

제2장([graphics.md](graphics.md))에서, `layout` 함수는 텍스트를 문자 단위로 순회하며 공간이 부족할 때마다 다음 줄로 이동했습니다. 이는 각 문자가 거의 *단어*인 중국어에는 적절하지만, 영어에서는 단어 중간에서 줄 바꿈을 할 수 없습니다. 대신, 텍스트를 한 번에 한 단어씩 배치할 필요가 있습니다:[^whitespace]

[^whitespace]: 이 코드는 공백을 기준으로 단어들을 분리합니다. 따라서 단어 사이에 공백이 없을 중국어에서는 문제가 발생할 것입니다. 실제 브라우저는 단어 경계를 식별하는 것을 포함하여, 텍스트 배치에 언어별 규칙을 사용합니다.

```{.python expected=False}
def layout(text):
    # ...
    for word in text.split():
        # ...
    return display_list
```

중국어 문자와는 달리, 영어 단어들은 각각 다른 크기를 가지므로 각 단어의 너비를 측정해야 합니다:

```{.python expected=False}
import tkinter.font

def layout(text):
    font = tkinter.font.Font()
    # ...
    for word in text.split():
        w = font.measure(word)
    # ...
```

여기서는 Tk의 기본 폰트를 사용하도록 선택했습니다. 이제 텍스트를 `cursor_x` 위치에서 그린다면 오른쪽 끝은 `cursor_x + w`에 위치하게 될 것입니다. 이 값이 페이지의 오른쪽 가장자리 너머로 나갈 수 있으므로, 이 경우 다음 줄로 감싸서 공간을 만들어야 합니다:

```{.python expected=False}
def layout(text):
    for word in text.split():
        # ...
        if cursor_x + w > WIDTH - HSTEP:
            cursor_y += font.metrics("linespace") * 1.25
            cursor_x = HSTEP
```

이 코드 블록은 `for` 루프 내부만을 보여줍니다. `layout`의 나머지 부분은 그대로 두어야 합니다. 또한, 나는 인자를 주어 `metrics`를 호출했는데, 이는 단지 해당 이름의 메트릭을 직접 반환합니다. 마지막으로, `y`를 증가시킬 때 linespace에 1.25를 곱한 것을 확인하세요. 만약 이 곱셈 인수를 제거하면, 줄 사이가 너무 촘촘해져 텍스트를 읽기 어려워짐을 볼 수 있습니다.[^tight] 대신, 줄 사이에 "줄 간격(line spacing)" 또는 "리딩(leading)"[^leading]을 추가하는 것이 일반적입니다. 25%의 줄 간격은 일반적인 비율입니다.

[^tight]: 디자이너들은 텍스트가 너무 "타이트하다"고 말합니다.
[^leading]: 이 용어는 금속 활자 시대에 글자 사이에 얇은 납 조각을 끼워 넣어 줄 간격을 벌렸던 것에서 유래합니다. 납은 실제 글자들이 만들어진 금속보다 부드러워서, 다른 조각들에 일정한 압력을 유지하기 위해 약간 압축될 수 있었습니다. 발음은 "led-ing"이지 "leed-ing"가 아닙니다.

이제 `cursor_x`와 `cursor_y`는 단어의 _시작_ 위치를 가리키게 되었으므로, 이를 표시할 목록에 추가하고 `cursor_x`를 단어의 끝으로 업데이트합니다:

```{.python expected=False}
def layout(text):
    for word in text.split():
        # ...
        display_list.append((cursor_x, cursor_y, word))
        cursor_x += w + font.measure(" ")
```

나는 `cursor_x`에 `w` 대신 `w + font.measure(" ")`를 더합니다. 이는 단어들 사이에 공백을 두기 위함입니다. `split()` 호출은 모든 공백을 제거하므로 이를 다시 추가하는 것입니다. 단, `if` 조건에서는 마지막 단어 뒤에 공백이 필요 없으므로 공백을 더하지 않습니다.

::: {.further}
단어 중간에서 줄을 나누는 것을 하이픈화(hyphenation)라고 하며, 이는 [`hyphens` CSS property][hyphens]를 통해 활성화할 수 있습니다. 최신 방식은 단어 조각의 사전을 사용하여 가능한 하이픈 지점을 우선순위로 선정하는 [Knuth–Liang 하이픈화 알고리즘][liang]입니다. 처음에는 CSS 명세가 이 알고리즘과 [호환되지 않았다][css-hyphen]가, 최근의 [`text-wrap-style` property][css4-text]가 이를 해결했습니다.
:::

[liang]: http://www.tug.org/docs/liang/liang-thesis.pdf
[hyphens]: https://drafts.csswg.org/css-text-3/#hyphens-property
[css-hyphen]: https://news.ycombinator.com/item?id=19472922
[css4-text]: https://drafts.csswg.org/css-text-4/#propdef-text-wrap-style

# 텍스트 스타일링

현재 페이지에 있는 모든 텍스트는 하나의 폰트로 그려지고 있습니다. 그러나 웹 페이지는 때때로 `<b>`와 `<i>` 태그를 사용하여 텍스트가 **볼드** 혹은 *이탤릭*으로 표시되어야 한다고 지정합니다. 이를 지원하면 좋겠지만, 지금의 코드는 그렇지 않습니다. 왜냐하면 `layout` 함수는 페이지의 텍스트만을 입력으로 받기 때문에 볼드와 이탤릭 태그가 어디에 있는지 알지 못하기 때문입니다.

이제 `lex` 함수를 수정하여 토큰(token)들의 리스트를 반환하도록 합시다. 여기서 토큰은 태그 밖의 문자들이 연속된 부분을 나타내는 `Text` 객체이거나, 태그 내용을 담은 `Tag` 객체입니다.  
여기서 `Text`와 `Tag` 클래스를 작성해야 합니다:[^dataclass]

[^dataclass]: 만약 파이썬에 익숙하다면, 이러한 유틸리티 클래스를 보다 쉽게 정의할 수 있게 해주는 `dataclass` 라이브러리를 사용할 수도 있습니다.

```{.python}
class Text:
    def __init__(self, text):
        self.text = text

class Tag:
    def __init__(self, tag):
        self.tag = tag
```

이제 `lex` 함수는 텍스트를 `Text`와 `Tag` 객체로 수집해야 합니다:[^exercises]

[^exercises]: 이전 장의 일부 또는 전체 연습 문제를 이미 수행했다면, 여러분의 코드는 다르게 보일 수 있습니다. 이 책의 코드 스니펫은 연습 문제를 수행하지 않은 상태를 가정하고 있으므로, 여러분의 수정 사항을 이식해야 할 수도 있습니다.

```{.python}
def lex(body):
    out = []
    buffer = ""
    in_tag = False
    for c in body:
        if c == "<":
            in_tag = True
            if buffer: out.append(Text(buffer))
            buffer = ""
        elif c == ">":
            in_tag = False
            out.append(Tag(buffer))
            buffer = ""
        else:
            buffer += c
    if not in_tag and buffer:
        out.append(Text(buffer))
    return out
```

여기서는 이제 사용 전에 텍스트나 태그 내용을 저장하므로 `text` 변수를 `buffer`로 이름을 변경했습니다. 이 이름은 또한 루프가 끝난 후 버퍼에 저장된 텍스트가 있는지, 그리고 그것을 어떻게 처리할지를 확인해야 함을 상기시켜 줍니다. 즉, `lex` 는 축적된 텍스트를 `Text` 객체로 내보냅니다. 만약 앵글 브래킷을 한 번도 보지 못했다면, 빈 토큰 리스트를 반환하게 됩니다. 하지만 `Hi!<hr`과 같이 미완성 태그는 버려집니다.[^errortag]

[^errortag]: 이 결정이 다소 이상하게 느껴질 수도 있습니다: 왜 저자는 태그를 완성하지 않고 버리기로 했을까요? 저는 잘 모르겠지만, 브라우저들이 그렇게 동작하기 때문입니다.

텍스트와 태그는 비대칭적입니다: `lex` 는 빈 `Text` 객체는 피하지만 빈 `Tag` 객체는 그대로 둡니다. 이는 빈 `Tag` 객체가 HTML 코드 `<>`를 나타내는 반면, 빈 `Text` 객체는 아무런 내용도 나타내지 않기 때문입니다.

이제 `lex` 함수를 수정했으므로, 페이지의 텍스트뿐만 아니라 태그들도 함께 `layout` 함수에 전달됩니다. 따라서 `layout` 함수는 텍스트가 아니라 토큰들을 순회해야 합니다:

```{.python expected=False}
def layout(tokens):
    # ...
    for tok in tokens:
        if isinstance(tok, Text):
            for word in tok.text.split():
                # ...
    # ...
```

또한, `layout` 함수는 페이지의 지시에 따라 폰트를 변경하기 위해 태그 토큰을 검사할 수 있습니다. 우선, 굵기(weight)와 스타일(style)을 지원하기 위해 두 개의 변수를 준비합시다:

```{.python replace=weight/self.weight,style/self.style}
weight = "normal"
style = "roman"
```

이 변수들은 볼드와 이탤릭의 시작 및 종료 태그가 보일 때 변경되어야 합니다:

```{.python replace=weight/self.weight,style/self.style indent=8}
if isinstance(tok, Text):
    # ...
elif tok.tag == "i":
    style = "italic"
elif tok.tag == "/i":
    style = "roman"
elif tok.tag == "b":
    weight = "bold"
elif tok.tag == "/b":
    weight = "normal"
```

이 코드는 `<b>bold</b>`와 `<i>italic</i>` 텍스트뿐만 아니라 `<b><i>bold italic</i></b>` 텍스트도 올바르게 처리합니다.[^even-misnested]

[^even-misnested]: 심지어 `<b>b<i>bi</b>i</i>`처럼 올바르지 않게 중첩된 태그도 처리하나, `<b><b>twice</b>bolded</b>`와 같은 텍스트는 처리하지 못합니다. 이에 대해서는 [Chapter 6](styles.md)에서 다시 다루겠습니다.

`style`과 `weight` 변수는 폰트를 선택하는 데 사용됩니다:

```{.python expected=False}
if isinstance(tok, Text):
    for word in tok.text.split():
        font = tkinter.font.Font(
            size=16,
            weight=weight,
            slant=style,
        )
        # ...
```

폰트는 `layout` 함수에서 계산되지만 `draw` 함수에서 사용되므로, 표시 리스트의 각 항목에 사용된 폰트를 추가해야 합니다:

```{.python expected=False}
if isinstance(tok, Text):
    for word in tok.text.split():
        # ...
        display_list.append((cursor_x, cursor_y, word, font))
```

`draw` 함수도 이 추가된 폰트 필드를 예상하고 사용하도록 업데이트해 주세요.

::: {.further}
_이탤릭_ 폰트는 이름 그대로 이탈리아에서 개발되어 "[chancery hand][chancery]"라는 필기체 스타일을 모방합니다. 반면, 이탤릭이 아닌 폰트는 로마 기념비에 새겨진 텍스트를 모방하기 때문에 *roman*이라 불립니다. 또한, 덜 알려진 세 번째 옵션인 [`<span style="font-style:oblique">oblique</span>`{=html} `\textsl{oblique}`{=latex} fonts][oblique]가 있는데, 이는 로만 폰트를 닮았지만 기울어져 있습니다.
:::

[chancery]: https://en.wikipedia.org/wiki/Chancery_hand
[oblique]: https://en.wikipedia.org/wiki/Oblique_type

# 레이아웃 객체

이러한 태그들이 모두 추가되면서, `layout` 함수는 많은 지역 변수와 복잡한 제어 흐름을 가진 꽤 큰 함수가 되어버렸습니다. 이것은 함수가 아닌 클래스여야 한다는 한 가지 신호입니다:

```{.python}
class Layout:
    def __init__(self, tokens):
        self.display_list = []
```

이제 `layout` 함수에 있던 모든 지역 변수를 `Layout` 클래스의 필드로 옮깁니다:

```{.python}
self.cursor_x = HSTEP
self.cursor_y = VSTEP
self.weight = "normal"
self.style = "roman"
```

기존 `layout` 함수의 핵심은 토큰들을 순회하는 루프였으며, 그 루프의 본문을 `Layout` 클래스의 메서드로 옮길 수 있습니다:

```{.python}
def __init__(self, tokens):
    # ...
    for tok in tokens:
        self.token(tok)

def token(self, tok):
    if isinstance(tok, Text):
        for word in tok.text.split():
            # ...
    elif tok.tag == "i":
        self.style = "italic"
    # ...
```

사실, `if isinstance(tok, Text)` 분기의 본문은 별도의 메서드로 옮길 수 있습니다:

```{.python expected=False}
def word(self, word):
    font = tkinter.font.Font(
        size=16,
        weight=self.weight,
        slant=self.style,
    )
    w = font.measure(word)
    # ...
```

이제 Browser의 기존 layout 함수에서 모든 코드가 분리되었으므로,  
이를 Layout 호출로 대체할 수 있습니다.

```{.python}
class Browser:
    def load(self, url):
        body = url.request()
        tokens = lex(body)
        self.display_list = Layout(tokens).display_list
        self.draw()
```

이와 같이 대규모 리팩토링을 진행할 때는 점진적으로 작업하는 것이 중요합니다.  
한 번에 모든 것을 변경하는 것이 더 효율적으로 보일 수 있지만,  
그 효율성은 너무 많은 것을 한꺼번에 시도하여 혼란에 빠져 리팩토링 전체를 포기하게 만드는 위험을 동반합니다.  
그러므로 계속 진행하기 전에 브라우저가 여전히 제대로 동작하는지 잠시 테스트해 보세요.

어쨌든, 이번 리팩토링을 통해 모든 텍스트 처리 코드가 독립된 메서드로 분리되었으며,  
주요 token 함수는 단지 태그 이름에 따라 분기하기만 합니다.  
이제 새롭고 깔끔해진 구조를 활용하여 더 많은 태그를 추가해 봅시다.  
폰트의 두께와 스타일은 이미 동작하고 있으므로, 다음으로 타이포그래피의 최첨단은 글자 크기입니다.  
글자 크기를 변경하는 간단한 방법 중 하나는 `<small>` 태그와 더 이상 사용되지 않는 `<big>` 태그를 사용하는 것입니다.[^why-obsolete]

[^why-obsolete]:
    In your web design projects, use the CSS `font-size`
    property to change text size instead of `<big>` and `<small>`. But
    since we haven't yet implemented CSS for our browser (see [Chapter
    6](styles.md)), we're stuck using tags here.

우리의 폰트 스타일과 두께에 관한 경험은 Layout의 size 필드를 단순하게  
커스터마이징하는 접근 방식을 제안합니다. 우선 다음과 같이 시작합니다:

```{.python}
self.size = 12
```

위 변수는 폰트 객체를 생성하는 데 사용됩니다:

```{.python expected=False}
font = tkinter.font.Font(
    size=self.size,
    weight=self.weight,
    slant=self.style,
)
```

또한, 이 변수를 업데이트하여 `<big>` 및 `<small>` 태그 내에서 글자 크기를 변경할 수 있습니다:

```{.python indent=4}
def token(self, tok):
    # ...
    elif tok.tag == "small":
        self.size -= 2
    elif tok.tag == "/small":
        self.size += 2
    elif tok.tag == "big":
        self.size += 4
    elif tok.tag == "/big":
        self.size -= 4
```

전체 문단을 마치 작은 글씨체(미세 인쇄문)처럼 `<small>` 태그로 감싸 보세요.  
그러면 새로 얻은 타이포그래피적 자유를 만끽할 수 있을 것입니다.

::: {.further}
All of `<b>`, `<i>`, `<big>`, and `<small>` date from an earlier,
pre-CSS era of the web. Nowadays, CSS can change how an element
appears, so visual tag names like `<b>` and `<small>` are out of
favor. That said, `<b>`, `<i>`, and `<small>` still have some
[appearance-independent meanings][html5-text].
:::

[html5-text]: https://html.spec.whatwg.org/multipage/text-level-semantics.html#the-small-element

# 다양한 크기의 텍스트

이제 폰트 크기를 섞어 사용해 보세요—예를 들어 `<small>a</small><big>A</big>`와 같이요—그러면 글꼴 크기 코드에 문제가 있다는 것을 금방 알게 될 것입니다: 텍스트가 마치 빨래줄에 매달린 것처럼 상단에 정렬되어 있습니다. 하지만 여러분은 영어 텍스트가 일반적으로 모든 글자가 보이지 않는 _기준선_ 위에 맞춰 써진다는 사실을 알고 있습니다.

문제를 해결하는 방법을 같이 고민해 봅시다. 만약 큰 텍스트를 위로 이동시키면 이전 줄과 겹치게 되므로, 작은 텍스트는 아래로 이동해야 합니다. 이는 작은 텍스트의 수직 위치를 큰 텍스트가 `token`을 거친 *후*에 계산해야 함을 의미합니다. 그런데 작은 텍스트는 루프를 통해 먼저 처리되므로, 텍스트 줄을 처리하기 위한 *2단계 알고리즘*이 필요합니다. 첫 번째 단계에서는 줄에 들어갈 단어들을 식별하고 그들의 _x_ 좌표를 계산하며, 두 번째 단계에서는 단어들을 수직으로 정렬하여 _y_ 좌표를 계산합니다 (그림 4 참조).

::: {.center}
![Figure 4: How lines are laid out when multiple fonts are involved.
All words are drawn using a shared baseline. The ascent and descent
of the whole line is then determined by the maximum ascent and descent
of all words in the line, and leading is added before and after the
line.](im/text-line.png)
:::

첫 번째 단계부터 시작해 봅시다. 한 줄에는 여러 태그의 텍스트가 포함되므로, 처리될 줄을 저장할 `Layout` 클래스의 필드가 필요합니다. 이 필드인 `line`은 리스트가 되며, `text`는 단어들을 표시 리스트 대신 이 버퍼에 추가합니다. `line`의 항목들은 _x_ 좌표는 포함하지만 _y_ 좌표는 포함하지 않습니다. 왜냐하면 첫 번째 단계에서는 _y_ 좌표가 계산되지 않기 때문입니다:

```{.python}
class Layout:
    def __init__(self, tokens):
        # ...
        self.line = []
        # ...

    def word(self, word):
        # ...
        self.line.append((self.cursor_x, word, font))
```

새로운 `line` 필드는 기본적으로 단어들이 배치되기 전에 임시로 보관되는 버퍼 역할을 합니다. 두 번째 단계는 한 줄의 처리가 끝나면 이 버퍼를 비우는(flush) 단계입니다:

```{.python}
class Layout:
    def word(self, word):
        if self.cursor_x + w > WIDTH - HSTEP:
            self.flush()
```

버퍼를 사용할 때 흔히 그렇듯, 모든 토큰 처리가 끝난 후 버퍼가 반드시 비워지도록 해야 합니다:

```{.python}
class Layout:
    def __init__(self, tokens):
        # ...
        self.flush()
```

새로운 `flush` 함수는 세 가지 역할을 수행해야 합니다:

1. 단어들을 기준선에 맞춰 정렬해야 합니다 (그림 5 참조);
2. 그 단어들을 모두 표시 리스트에 추가해야 합니다; 그리고
3. `cursor_x`와 `cursor_y` 필드를 업데이트해야 합니다.

::: {.web-only}
Here's what it looks like, step by step:

::: {.widget height=204}
lab3-baselines.html
:::

:::

::: {.center}
![그림 5: 단어들을 줄 위에 정렬합니다.](examples/example3-words-align.png)
:::

단어들을 “줄 위에” 정렬하고자 하므로, 우선 그 줄이 어디에 위치해야 하는지를 계산해 봅시다. 이는 해당 줄에서 가장 높은 단어에 따라 결정됩니다:

```{.python indent=4}
def flush(self):
    if not self.line: return
    metrics = [font.metrics() for x, word, font in self.line]
    max_ascent = max([metric["ascent"] for metric in metrics])
```

그런 다음 기준선(baseline)은 `self.y`로부터 `max_ascent`만큼 아래에 위치하게 됩니다—혹은 실제로는 리딩(leading)을 고려하여 조금 더 내려갑니다:[^leading-half]

[^leading-half]:
    Actually, 25% leading doesn't add 25% of the ascent
    above the ascender and 25% of the descent below the descender.
    Instead, it adds [12.5% of the line height in both
    places][line-height-def], which is subtly different when fonts are
    mixed. But let's skip that subtlety here.

[line-height-def]: https://www.w3.org/TR/CSS2/visudet.html#leading

```{.python}
baseline = self.cursor_y + 1.25 * max_ascent
```

이제 줄의 위치를 알았으니, 각 단어를 그 기준선에 맞춰 배치하고 표시 리스트에 추가할 수 있습니다:

```{.python}
for x, word, font in self.line:
    y = baseline - font.metrics("ascent")
    self.display_list.append((x, y, word, font))
```

여기서 `y`는 기준선에서 시작해, 해당 단어의 상승(ascent)을 수용할 만큼만 위로 이동합니다. 이제 `cursor_y`는 가장 깊은 하강(descender)을 고려하여 기준선 아래로 충분히 이동해야 합니다:

```{.python}
max_descent = max([metric["descent"] for metric in metrics])
self.cursor_y = baseline + 1.25 * max_descent
```

마지막으로, `flush` 함수는 `Layout`의 `cursor_x`와 `line` 필드를 업데이트해야 합니다:

```{.python}
self.cursor_x = HSTEP
self.line = []
```

이제 텍스트 크기가 혼합되어 있더라도 모든 텍스트가 기준선에 맞춰 정렬됩니다. 게다가 이 새로운 `flush` 함수는 다른 줄 바꿈 작업에도 유용합니다. 예를 들어, HTML에서는 `<br>` 태그[^self-closing]가 현재 줄을 종료하고 새 줄을 시작합니다:

[^self-closing]:
    Which is a self-closing tag, so there's no `</br>`.
    Many tags that _are_ content, instead of annotating it, are like
    this. Some people like adding a final slash to self-closing tags,
    as in `<br/>`, but this is not required in HTML.

```{.python}
def token(self, tok):
    # ...
    elif tok.tag == "br":
        self.flush()
```

마찬가지로, 단락은 `<p>` 및 `</p>` 태그로 정의되므로, `</p>` 태그도 현재 줄을 종료합니다:

```{.python}
def token(self, tok):
    # ...
    elif tok.tag == "/p":
        self.flush()
        self.cursor_y += VSTEP
```

여기서는 단락 사이에 약간의 간격을 만들기 위해 `cursor_y`에 추가로 값을 더해줍니다.

이 시점에서 여러분은 브라우저를 실행하여 [예제 페이지](examples/example3-sizes.html)를 표시할 수 있을 것이며, 그 모습은 그림 6과 비슷할 것입니다.

::: {.center}
Figure 6: Screenshot of a web page demonstrating different text sizes.
:::

::: {.further}
실제로, 브라우저는 _수평_ 뿐만 아니라 [_수직_ 쓰기 시스템][vertical]도 지원합니다. 이는 일부 전통적인 동아시아 문체와 같이 사용되며, 특히 위에서 아래로, 왼쪽에서 오른쪽으로 진행되는 [몽골 문자][mongolian]가 도전 과제로 꼽힙니다. 많은 몽골의 [정부 웹사이트][president-mn]에서 이 문자를 사용하고 있습니다.
:::

[vertical]: https://www.smashingmagazine.com/2019/08/writing-modes-layout/
[mongolian]: https://www.w3.org/TR/mlreq/
[president-mn]: https://president.mn/mng/

# 폰트 캐싱

스타일이 적용된 텍스트를 구현한 후, 아마도 macOS[^macos-cache]를 사용하지 않는 한, [이 장](http://browser.engineering/text.html)과 같은 대형 웹 페이지에서 우리의 브라우저가 [이전 장](graphics.md)보다 상당히 느려졌음을 눈치챘을 것입니다. 이는 텍스트 레이아웃, 특히 각 단어를 측정하는 부분이 상당히 느리기 때문입니다.[^profile]

[^macos-cache]: 문서로 확인할 수는 없지만, macOS의 "Core Text" API는 리눅스나 윈도우보다 폰트를 더 적극적으로 캐싱하는 것으로 보입니다. 이 섹션에서 설명하는 최적화는 macOS에서는 문제를 일으키지 않지만, 윈도우와 리눅스에서만큼 속도를 크게 향상시키지는 않을 것입니다.
[^profile]: `python3` 명령 대신 `python3 -m cProfile`을 사용하여 Python 프로그램을 프로파일링할 수 있습니다. 텍스트 측정에 소요되는 시간을 확인하려면 `measure`와 `metrics` 호출에 해당하는 줄을 찾아보세요.

불행히도, 텍스트 측정 속도를 크게 빠르게 만드는 것은 어렵습니다. 비례 폰트와 힌팅, 케르닝과 같은 복잡한 폰트 기능 때문에, 텍스트 측정에는 상당히 복잡한 계산이 필요할 수 있습니다. 그러나 대형 웹 페이지에서는 어떤 단어들은 정말 많이 등장합니다—예를 들어, 이 장에서는 "the"라는 단어가 200번 넘게 등장합니다. 이런 단어들을 매번 측정하는 대신 한 번만 측정하고 결과를 캐싱하면, 보통 일반 영어 텍스트에서 상당한 속도 향상을 가져올 수 있습니다.

캐싱은 너무나도 좋은 아이디어이기 때문에 대부분의 텍스트 라이브러리는 이미 각 `Font` 객체 내에 텍스트 측정 결과를 캐싱하는 기능을 구현하고 있습니다. 그러나 우리의 `text` 메서드는 단어마다 새로운 `Font` 객체를 생성하기 때문에, 캐싱 효과가 제대로 나타나지 않습니다. 캐싱이 제대로 작동하게 하려면 새로운 객체를 생성하는 대신 가능한 한 기존의 `Font` 객체를 재사용해야 합니다.

우리는 전역 `FONTS` 딕셔너리에 캐시를 저장할 것입니다:

```python
FONTS = {}
```

이 딕셔너리의 키는 size/weight/style의 3요소 튜플이 되고, 값은 `Font` 객체가 될 것입니다.[^get_font-hack] 캐싱 로직 자체는 새로운 `get_font` 함수에 넣을 수 있습니다:

```python
def get_font(size, weight, style):
    key = (size, weight, style)
    if key not in FONTS:
        font = tkinter.font.Font(size=size, weight=weight,
            slant=style)
        label = tkinter.Label(font=font)
        FONTS[key] = (font, label)
    return FONTS[key][0]
```

[^get_font-hack]: 실제로 값은 폰트 객체와 `tkinter.Label` 객체로 이루어져 있습니다. 이 방식은 어떤 이유에서인지 `metrics`의 성능을 크게 향상시키며, [Python 문서][metrics-doc]에서 추천되고 있습니다.

[metrics-doc]: https://github.com/python/cpython/blob/main/Lib/tkinter/font.py#L163

그런 다음, `word` 메서드는 직접 `Font` 객체를 생성하는 대신 `get_font`를 호출할 수 있습니다:

```python
class Layout:
    def word(self, word):
        font = get_font(self.size, self.weight, self.style)
        # ...
```

이제 동일한 단어들은 동일한 폰트를 사용하게 되어, 텍스트 측정 결과가 캐시에 적중하게 됩니다.

::: {.further}
중국어와 같은 스크립트의 폰트는 용량이 수 메가바이트에 달할 수 있으므로, 보통 디스크에 저장되고 필요할 때만 메모리에 로드됩니다. 이로 인해 폰트 로딩이 느려지며 캐싱이 더욱 중요해집니다. 브라우저는 텍스트 측정, 셰이핑, 렌더링을 위한 방대한 캐시를 갖추고 있습니다. 웹 페이지에 텍스트가 많기 때문에, 이러한 캐시는 렌더링 속도를 크게 향상시키는 가장 중요한 요소 중 하나가 됩니다.
:::

# 요약

이전 장에서는 문자를 그리드에 배치하는 브라우저를 소개했습니다. 이제 브라우저는 표준 영어 텍스트 레이아웃을 처리할 수 있게 되어 다음과 같은 기능을 제공합니다:

- 텍스트가 단어 단위로 배치됩니다.
- 줄은 단어 경계에서 나뉩니다.
- 텍스트는 볼드 또는 이탤릭 스타일로 표시될 수 있습니다.
- 다양한 크기의 텍스트를 혼합하여 사용할 수 있습니다.

이제 우리의 브라우저를 사용하여 에세이, 블로그 글, 심지어 책도 읽을 수 있습니다!

::: {.web-only .widget height=400}
lab3-browser.html
:::

::: {.signup}
:::

# 개요

우리 브라우저에 포함된 모든 함수, 클래스, 메서드의 전체 목록은 다음과 같아야 합니다:

::: {.web-only .cmd .python .outline html=True}

```bash
python3 infra/outlines.py --html src/lab3.py --template book/outline.txt
```

:::

::: {.print-only .cmd .python .outline}

```bash
python3 infra/outlines.py src/lab3.py --template book/outline.txt
```

:::

# 연습 문제

### 3-1 _가운데 정렬 텍스트_

이 [책의 웹사이트](https://browser.engineering/text.html)에서 페이지 제목은 가운데 정렬되어 있습니다. `<h1 class="title">`와 `</h1>` 사이의 텍스트를 브라우저에서 동일하게 가운데 정렬하도록 만드세요. 각 줄은 개별적으로 가운데 정렬되어야 합니다.[^center-tag]

[^center-tag]: 초기 HTML에서는 이를 수행하는 `<center>` 태그가 있었지만, 오늘날에는 보통 CSS의 `text-align` 속성을 사용하여 텍스트를 가운데 정렬합니다. 이 연습 문제에서 사용하는 접근 방식은 비표준이며 학습 목적으로만 사용됩니다.

---

### 3-2 _위첨자(Superscripts)_

`<sup>` 태그를 지원하도록 추가하세요. 이 태그 안의 텍스트는 더 작아야 하며(예: 일반 텍스트 크기의 절반 정도), 위첨자의 상단이 일반 글자의 상단과 맞춰지도록 배치되어야 합니다.

---

### 3-3 _소프트 하이픈(Soft hyphens)_

소프트 하이픈 문자(`\N{soft hyphen}`로 작성)는 텍스트 렌더러가 단어를 줄 바꾸기와 함께 하이픈을 삽입할 수 있는 위치를 나타냅니다. 이를 지원하도록 추가하세요.[^entity] 단어가 줄 끝에 맞지 않을 경우, 소프트 하이픈을 확인하고 단어를 줄 바꾸기와 함께 나누세요. 단어에 여러 개의 소프트 하이픈이 있을 수 있음을 기억하고, 단어를 나눌 때 하이픈을 그려야 합니다. "super­cali­fragi­listic­expi­ali­docious"는 좋은 테스트 사례입니다.

[^entity]: [Exercise 1-4](http.md#exercises)에서 HTML 엔터티를 구현했다면, 소프트 하이픈으로 확장되는 `&shy;` 엔터티도 지원하도록 추가할 수 있습니다.

---

### 3-4 _스몰 캡(Small caps)_

`<abbr>` 요소가 스몰 캡으로 텍스트를 렌더링하도록 만드세요. 예: `<abbr>like this</abbr>`{=html} → `\textsc{like this}`{=latex}. `<abbr>` 태그 안에서는 소문자가 작고 대문자로 변환되며 굵게 표시되어야 하고, 다른 모든 문자(대문자, 숫자 등)는 일반 폰트로 그려져야 합니다.

---

### 3-5 _서식 있는 텍스트(Preformatted text)_

`<pre>` 태그를 지원하도록 추가하세요. 일반적인 문단과는 달리 `<pre>` 태그 안의 텍스트는 자동으로 줄 바꿈되지 않으며, 공백(스페이스 및 개행 문자)이 그대로 유지됩니다. 또한 고정 폭 폰트(예: `Courier New` 또는 `SFMono`)를 사용하세요. `<pre>` 태그 안에서도 태그가 정상적으로 작동해야 합니다. 예를 들어, `<pre>` 안에서 일부 텍스트를 굵게 표시할 수 있어야 합니다. [Exercise 1-4](http.md#exercises)를 완료했다면 결과가 더욱 보기 좋게 나올 것입니다.
