+++
title = "Markdown"
date = "2020-09-15"
draft = true
author = "Matheus Almeida"
description = "Artigo para lembrar da estrutura do markdown"
tags = [
    "diversos",
]
+++

use hugo server -D para aparecer essa pagina <!--more--> Redit teque digerit hominumque toris verebor lumina non cervice
subde tollit usus habet Arctonque, furores quas nec ferunt. Quoque montibus nunc
caluere tempus inhospita parcite confusaque translucet patri vestro qui optatis
lumine cognoscere flos nubis! Fronde ipsamque patulos Dryopen deorum.

1. Exierant elisi ambit vivere dedere
2. Duce pollice
3. Eris modo
4. Spargitque ferrea quos palude

Rursus nulli murmur; hastile inridet ut ab gravi sententia! Nomine potitus
silentia flumen, sustinet placuit petis in dilapsa erat sunt. Atria
tractus malis.

[Built-in Shortcodes](https://gohugo.io/) for rich content 

## Mane refeci capiebant unda mulcebat

Victa caducifer, malo vulnere contra
dicere aurato, ludit regale, voca! Retorsit colit est profanae esse virescere
furit nec; iaculi matertera et visa est, viribus. Divesque creatis, tecta novat collumque vulnus est, parvas. **Faces illo pepulere** tempus adest. Tendit flamma, ab opes virum sustinet, sidus sequendo urbis.

<!--comentario html-->

## titulos

`<h1>`—`<h6>`

# H1
## H2
### H3
#### H4
##### H5
###### H6

## Paragraph

Mussum Ipsum, cacilds vidis litro abertis. Praesent malesuada urna nisi, quis volutpat erat hendrerit non. Nam vulputate dapibus. Nec orci ornare consequat. Praesent lacinia ultrices consectetur. Sed non ipsum felis. Vehicula non. Ut sed ex eros. Vivamus sit amet nibh non tellus tristique interdum. Tá deprimidis, eu conheço uma cachacis que pode alegrar sua vidis.

Itatur? Quiatae cullecum rem ent aut odis in re eossequodi nonsequ idebis ne sapicia is sinveli squiatum, core et que aut hariosam ex eat.

## Blockquotes

#### Blockquote without attribution

> Tiam, ad mint andaepu dandae nostion secatur sequo quae.
> **Note** that you can use *Markdown syntax* within a blockquote.

#### Blockquote with attribution

> Don't communicate by sharing memory, share memory by communicating.</p>
> — <cite>Rob Pike[^1]</cite>


[^1]: citacao.

## Tables

Tables aren't part of the core Markdown spec, but Hugo supports supports them out-of-the-box.

   Name | Age
--------|------
    Bob | 27
  Alice | 23

#### Inline Markdown within tables

| Inline&nbsp;&nbsp;&nbsp;     | Markdown&nbsp;&nbsp;&nbsp;  | In&nbsp;&nbsp;&nbsp;                | Table      |
| ---------- | --------- | ----------------- | ---------- |
| *italics*  | **bold**  | ~~strikethrough~~&nbsp;&nbsp;&nbsp; | `code`     |

## Code Blocks

#### Code block with backticks

```
html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Example HTML5 Document</title>
</head>
<body>
  <p>Test</p>
</body>
</html>
```

#### Code block with Hugo's internal highlight shortcode
{{< highlight html >}}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Example HTML5 Document</title>
</head>
<body>
  <p>Test</p>
</body>
</html>
{{< /highlight >}}

## List Types

#### Ordered List

1. First item
2. Second item
3. Third item

#### Unordered List

* List item
* Another item
* And another item

#### Nested list

* Item
1. First Sub-item
2. Second Sub-item

---

## Instagram Simple Shortcode

{{< instagram_simple BGvuInzyFAe hidecaption >}}

<br>

---

## YouTube, url: https://youtu.be/ZJthWmvUzzc

{{< youtube ZJthWmvUzzc >}}

<br>

---

## Twitter Simple Shortcode

{{< twitter_simple 1085870671291310081 >}}

<br>

---

## Vimeo, url: https://vimeo.com/4.8912912e+07

{{< vimeo_simple 48912912 >}}