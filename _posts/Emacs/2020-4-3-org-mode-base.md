---
title: org-mode
tags: [ Emacs ]
---

<div id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#sec-1">1. code</a></li>
<li><a href="#sec-2">2. table</a>
<ul>
<li><a href="#sec-2-1">2.1. C-c C-c</a></li>
<li><a href="#sec-2-2">2.2. C-c |</a></li>
<li><a href="#sec-2-3">2.3. |- TAB</a></li>
<li><a href="#sec-2-4">2.4. | TAB</a></li>
<li><a href="#sec-2-5">2.5. TAB</a></li>
<li><a href="#sec-2-6">2.6. Shift + TAB</a></li>
<li><a href="#sec-2-7">2.7. C-c space</a></li>
</ul>
</li>
</ul>
</div>
</div>

# code<a id="sec-1" name="sec-1"></a>

    int main(){
      printf("hello org mode\n");
      return 0;
    }

# table<a id="sec-2" name="sec-2"></a>

## C-c C-c<a id="sec-2-1" name="sec-2-1"></a>

-   表格对齐

## C-c |<a id="sec-2-2" name="sec-2-2"></a>

-   生成表格

## |- TAB<a id="sec-2-3" name="sec-2-3"></a>

-   生成 |-&#x2014;+-&#x2014;|结构

## | TAB<a id="sec-2-4" name="sec-2-4"></a>

-   生成 |     |结构

## TAB<a id="sec-2-5" name="sec-2-5"></a>

-   从前到后下一个表项

## Shift + TAB<a id="sec-2-6" name="sec-2-6"></a>

-   从后到前下一个表项

## C-c space<a id="sec-2-7" name="sec-2-7"></a>

-   清除当前格

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="left" />

<col  class="right" />

<col  class="left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="left">name</th>
<th scope="col" class="right">age</th>
<th scope="col" class="left">sex</th>
</tr>
</thead>

<tbody>
<tr>
<td class="left">&#xa0;</td>
<td class="right">58</td>
<td class="left">female</td>
</tr>


<tr>
<td class="left">ghivern</td>
<td class="right">100</td>
<td class="left">male</td>
</tr>
</tbody>
</table>
