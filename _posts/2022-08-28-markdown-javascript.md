---
title: "GitHub/Jekyll 블로그에 자바스크립트 추가하기 1"
categories:
    - 글쓰기
tags:
    - 글쓰기
    - 블로그
    - 수식
math: true
toc: true
toc_sticky: true
---

## 수식, 다이어그램,차트 등이 포함된 글을 써보자

글을 쓰다 보면 수식, 다이어그램, 차트 등을 포함시켜야 하는 경우가 많다.

`GitHub/Jekyll` 조합을 통한 블로그는 마크다운 형식으로 글을 쓰는데, 자바스크립트 라이브러리를 활용하면 수식과 다이어그램, 차트 등 다양한 것들을 추가할 수있다.

항목 | 라이브러리 | 웹주소
--- | --- | ---
수식 | `MathJax` | <https://www.mathjax.org/>
다이어그램 | `Mermaid` | <https://mermaid-js.github.io/>
차트 | `Chart.js` | <https://www.chartjs.org/>

### 자바스크립트 추가

> `_includes` 폴더에 `xxx-support.html` 생성 후 스크립트 추가한다.

관련 라이브러리가 있는 웹에 방문하면 스크립트 사용 방법을 알 수 있다. 예를 들어 `MathJax`는 다음과 같은 내용을 추가하면 된다.

`config`부분 때문에 좀 복잡하긴 한데, 다음 두 부분이 추가되었기 때문이다.

* 수식에 넘버링 추가
* 인라인 수식과 독립적인 수식의 시작과 끝 문자 지정

```html
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    TeX: {
      equationNumbers: { autoNumber: "AMS" },
      tagSide: "right"
    },
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
      processEscapes: true
    }
  });
  MathJax.Hub.Register.StartupHook("TeX AMSmath Ready", function () {
    MathJax.InputJax.TeX.Stack.Item.AMSarray.Augment({
      clearTag() {
        if (!this.global.notags) {
          this.super(arguments).clearTag.call(this);
        }
      }
    });
  });
</script>
<script type="text/javascript" charset="utf-8"
  src="https://cdn.jsdelivr.net/npm/mathjax@2/MathJax.js?config=TeX-AMS_CHTML">
</script>
```

따라서 `mathjax-support.html` 을 만들어 위 스크립트를 추가하자.

### 레이아웃 헤더 수정

> `_layout/default.html` 파일의 `<head>` 부분 수정

지킬 블로그의 기본 레이아웃을 설정하는 부분이 `_layout/default.html` 이다.
이 파일의 헤드 부분에 다음과 같은 내용을 추가하여 블로그 페이지에서 작성한 글에서 온오프할 수 있게 한다.

아래서 중요한 단어는 다음과 같다.

* `page.math`
* `mathjax-support.html` : `_includes` 폴더에 스크립트를 포함한 html 문서 이름임

```ruby
{ % if page.math % }
    { % include mathjax-support.html % }
{ % endif % }
```

참고로 위의 내용에 `{`, `}` 과 `%` 사이의 빈칸은 없애야 함.

### 작성 글의 YMF 수정

> 작성할 글의 YMF(Yaml front matter) 수정

위에서 `if` 다음에 `page.math`를 추가하였으므로, 아래와 같이 YMF에서 `math: true`를 해주면 `MathJax` 자바스크립트를 사용할 수 있게 된다.

```yaml
---
title: "GitHub/Jekyll 블로그에 자바스크립트 추가하기"
categories:
    - 글쓰기
math: true
---
```

### 글의 본문 작성

아래와 같은 형식으로 마크다운 문서를 추가하면 웹페이지에서 매우 잘 렌더링 됨을 알 수 있다.

* 입력 예

```tex
인라인 수식 : \\(f(x) = e^x\\), $f(x)=e^x$

독립적인 수식

$$f(x) = \frac{e^x}{(1+x)}$$

$$
\begin{equation}
   E = mc^2
\end{equation}
$$

\begin{equation}
\label{eq:square}
\begin{split}
   (x+1)^2 &= (x+1)(x+1)\\
           &= x^2 + 2x + 1
\end{split}
\end{equation}

Link to equation \eqref{eq:square}

```

* 출력 예

인라인 수식 : \\(f(x) = e^x\\), $f(x)=e^x$

수식

$$f(x) = \frac{e^x}{(1+x)}$$

$$
\begin{equation}
   E = mc^2
\end{equation}
$$

\begin{equation}
\label{eq:square}
\begin{split}
   (x+1)^2 &= (x+1)(x+1)\\
           &= x^2 + 2x + 1
\end{split}
\end{equation}

Link to equation \eqref{eq:square}

### 참고 사이트

* [Jekyll Github 블로그에 MathJax로 수학식 표시하기](https://mkkim85.github.io/blog-apply-mathjax-to-jekyll-and-github-pages/)
* [Stack Overflow: MathJax equation numbers do not show using Jekyll on GitHub Pages](https://stackoverflow.com/questions/59141529/mathjax-equation-numbers-do-not-show-using-jekyll-on-github-pages)