# 쌓임 맥락에 대해 설명해주세요.

## `쌓임 맥락(Stacking context)`

`쌓임 맥락(Stacking context)`이란, 가상의 z축을 상정하여 HTML 요소들을 사용자가 화면을 바라보는 기준으로 가상의 z축을 상정하여 HTML 요소를 3차원 개념화한 것을 말한다. 쌓임 맥락은 요소들이 쌓이는 순서에 영향을 미친다.

기본적으로 HTML 요소는 HTML 상에서 위치에 위치할수록 아래쪽에 쌓이는 방식으로 DOM 순서에 따라 쌓인다. 그런데 특정 조건이 충족되면 새로운 쌓임 맥락이 형성되며 독립적인 3차원 공간을 만든다. 이 쌓임 맥락은 부모 쌓임 맥락의 영향을 받지 않고 해당 쌓임 맥락 내에서만 비교가 이뤄져 쌓임 순서가 결정된다.

대표적으로 쌓임 맥락이 생성되는 조건은 다음과 같다.

- `position` 속성이 `relative` 또는 `absolute`이고 `z-index `값이 `auto`가 아닌 경우
- `postion` 속성이 `fixed` 또는 `sticky`인 경우
- `display`가 `flex` 또는 `grid`이고, `z-index`가 설정된 경우
- `opacity` 값이 `1` 미만인 경우
- `transform`, `filter`, `backdrop-filter` 등의 속성이 적용되는 경우

> `z-index` 값이 낮을수록 아래쪽으로, 높을수록 위쪽으로 쌓이게 된다.

## 예시

```html
<div style="position: relative; z-index: 1;">
    A 요소 (z-index: 1)
    <div style="position: absolute; z-index: 999;">
        A-1 요소 (z-index: 999)
    </div>
    <div style="position: absolute; z-index: 1;">
        A-2 요소 (z-index: 999)
    </div>
</div>

<div style="position: relative; z-index: 2;">
    B 요소 (z-index: 2)
</div>
```

위 예시에서는 가장 위쪽부터 B, A-1, A 요소 순으로 쌓인다.
최상위 쌓임 맥락 내의 요소들인 A 요소와 B요소의 `z-index` 값을 비교했을 때, B 요소가 더 크기 때문에 가장 위쪽에 쌓인다. 
그리고 A 요소가 형성한 쌓임 맥락 내에서 `z-index` 값을 비교했을 때, A-2 요소보다 `z-index`가 큰 A-1 요소가 더 위쪽에 쌓이게 된다. 또한 A-1 요소의 `z-index`는 A가 생성한 쌓임 맥락 내부에서만 의미가 있기 때문에, 최상위 쌓임 맥락에서는 여전히 B보다 아래에 표시가 된다.

## 공식문서 참고
> Stacking context is a **three-dimensional conceptualization of HTML elements** along an imaginary z-axis relative to the user, who is assumed to be facing the viewport or the webpage. The stacking context determines how elements are layered on top of one another along the z-axis (think of it as the "depth" dimension on your screen). Stacking context determines the visual order of how overlapping content is rendered.

`Stacking context`는 사용자가 웹페이지를 바라보고 있다고 가정했을 때, HTML 요소들을 가상의 z축(깊이) 상에서 3차원적으로 개념화한 것입니다. `Stacking context`는 요소들이 화면에서 겹칠 때 어떤 순서로 쌓여 보이는지를 결정합니다.

> Elements within a stacking context are stacked independently from elements outside of that stacking context, ensuring elements in one stacking context don't interfere with the stacking order of elements in another. Each stacking context is completely independent of its siblings: only descendant elements are considered when stacking is processed.

하나의 `Stacking context` 안에 있는 요소들은 해당 컨텍스트 외부의 요소들과 독립적으로 쌓입니다. **즉, 한 `Stacking context`의 요소가 다른 컨텍스트의 쌓임 순서에 영향을 주지 않습니다.** 각 `Stacking context`는 형제 요소들과 독립적이며, 쌓임 처리 시 해당 요소 내부의 자식 요소들만 고려됩니다.

> Each stacking context is self-contained. After an element's contents are stacked, the entire element is considered as a single unit in the stacking order of its parent stacking context.

`Stacking context`는 자체적으로 완결적입니다. 요소 내부의 자식들이 쌓인 후, 그 요소 전체는 부모 `Stacking context`에서 하나의 단위로 취급됩니다.

> Within a stacking context, child elements are stacked according to the z-index values of all the siblings. The stacking contexts of these nested elements only have meaning in this parent. Stacking contexts are treated atomically as a single unit in the parent stacking context. Stacking contexts can be contained in other stacking contexts, and together create a hierarchy of stacking contexts.


`Stacking context` 내부에서는 자식 요소들이 형제들의 z-index 값에 따라 쌓입니다. 중첩된 `Stacking context`는 오직 부모 컨텍스트 안에서만 의미가 있습니다. `Stacking context`는 부모 컨텍스트에서 하나의 단위로 취급되며, 다른 `Stacking context` 안에 포함될 수 있어 계층 구조를 형성합니다.

> The hierarchy of stacking contexts is a subset of the hierarchy of HTML elements because only certain elements create stacking contexts. Elements that don't create their own stacking contexts are assimilated by the parent stacking context.

`Stacking context`의 계층 구조는 HTML 요소 계층 구조의 부분 집합입니다. 왜냐하면 모든 요소가 `Stacking context`를 생성하는 것은 아니기 때문입니다. `Stacking context`를 생성하지 않는 요소는 부모 `Stacking context`에 포함됩니다.

=> 특정 요소의 `z-index`는 **자신이 속한 가장 가까운 상위 스태킹 컨텍스트** 안에서만 의미가 있다. 또한 상위 요소 중에 stacking context를 만드는 부모가 없으면, 결국 최상위(html/body) stacking context가 기준이 된다.



### 참고자료 
- [쌓임 맥락에 대해 설명해주세요](https://www.maeil-mail.kr/question/158)  
- [쑤쑤의 z-index의 동작 방식](https://www.youtube.com/watch?v=ln9vfw-JAr8)  
- [쌓임 맥락](https://developer.mozilla.org/ko/docs/Web/CSS/Guides/Positioned_layout/Stacking_context)