---
title: "Shortcodes"
date: 2022-10-22T18:53:54+02:00
draft: false
weight: 20
chapter: false
math: true
---

#### You may have noticed that some syntax can not be displayed via markdown (like equations). In the template some shortcodes are implemented to display notice boxes, mermaids (i.e. diagramms) and equations. 

## Notice 

To emphasize certain facts, it is possible to use notice. These are special shortcodes which can be implemented quite easily. 

{{% notice note %}}
This is a notice box
{{% /notice %}}

```
{{%/* notice note */%}}
This is a notice box
{{%/* /notice */%}}
```

There are also info, tip and warning boxes.

{{% notice info %}}
This is an info box
{{% /notice %}}

{{% notice tip %}}
This is a tip box
{{% /notice %}}

{{% notice warning %}}
This is a notice box
{{% /notice %}}

Just exchange "note" for info, tip or warning to achieve the desired effect.

&nbsp;

In the past we had problems displaying multible lines with these boxes. You can do that by using the backslash ("\\").
{{% notice tip %}}
This is a tip box\
\
spanning over multible lines.
{{% /notice %}}

```
{{%/* notice tip */%}}
This is a tip box\
\
spanning over multible lines.
{{%/* /notice */%}}
```

## Mermaid

If you want to display flowcharts or other diagrams you can use the build in mermaid library. This was used for example in the scengraph tutorial. 

{{<mermaid align="middle">}}
graph TD;
    root([m_RootSGN])
    groundTransform([m_GroundTransformSGN])
    groundSGN([m_GroundSGN])
    root ==> groundTransform
    groundTransform ==> groundSGN 
{{< /mermaid >}}

To display such diagrams, you need to use the short code state below: 

    {{</*mermaid align="left"*/>}}
    graph TD;
        root([m_RootSGN])
        groundTransform([m_GroundTransformSGN])
        groundSGN([m_GroundSGN])
        root ==> groundTransform
        groundTransform ==> groundSGN 
    {{</* /mermaid */>}}

[Here you can get to know more about mermaid diagrams.](https://mermaid.js.org/intro/)

## Equations

In addition \\(\KaTeX\\) is used for displaying mathematical equations. This is not normally supportet in the template itself. 
It was done with the help of this [tutorial](https://www.simonspavound.com/posts/2020/09/equations-with-katex-in-hugo/). The tutorial states to put the math partial in **single.html** file. This does not work! The **list.html** had to be changed. 

If you want to use \\(\KaTeX\\) you have to enable it in the front matter via `math: true`. 

You can display equations either inline, like the \\(\KaTeX\\) logo beforehand.

```md 
\\(\KaTeX\\) 
```

You can also display it like this: 

$$x_1,_2 = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$$

Then you need to use two dollar signs: 

```tex
$$x_1,_2 = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$$
```

It is also possible to use the math shortcode for displaying bigger equations:

```tex
{{</* math */>}}
$$
  \begin{bmatrix}
    a & b \\
    c & d \\
    e & f \\
  \end{bmatrix}
$$
{{</* /math */>}}
```


Here you can learn more about [\\(\KaTeX\\)](https://katex.org/docs/supported.html). 