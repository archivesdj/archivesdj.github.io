---
title: "GitHub/Jekyll 블로그에 수식과 다이어그램 표시하기"
excerpt: "MathJax/Mermaid를 블로그 세팅에 추가하여 수식과 다이어그램을 그려보자."
categories:
    - 글쓰기
tags:
    - 글쓰기
    - 블로그
math: true
mermaid: true
toc: true
toc_sticky: true
---

## 수식과 다이어그램

글을 쓰다 보면 수식과 다이어그램을 써야 하는 경우가 많다.

`GitHub/Jekyll` 조합을 통한 블로그는 마크다운 형식으로 글을 쓰는데, 수식과 다이어그램은 각각 다음의 조합을 통해 해결 가능하다.

내용 | 툴
--- | ---
수식 | `MathJax`
다이어그램 | `Mermaid`

### 셋업

### 테스트

#### 수식

* 입력

```markdown
인라인 수식 : \\(f(x) = e^x\\)

수식

$$f(x) = \frac{e^x}{(1+x)}$$
```

* 출력

인라인 수식 : \\(f(x) = e^x\\) $f(x) = e^x$

수식

$$f(x) = \frac{e^x}{(1+x)}$$

$$
\begin{equation}
   E = mc^2
\end{equation}
$$

#### 다이어그램

다이어그램은 그림을 그려서 추가해도 되지만 Mermaid 문법을 배워서 추가하면 훨씬 깔끔하게 할 수 있다.

* 입력

```
<div class="mermaid">
graph TD;
  A-->B;
  A-->C;
  B-->D;
  C-->D;
</div>
```

* 출력

<div class="mermaid">
graph TD;
  A-->B;
  A-->C;
  B-->D;
  C-->D;
</div>
