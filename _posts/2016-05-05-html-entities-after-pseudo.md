---
layout: post
title:  "HTML entity v ::after pseudo-elementu"
subtitle: "Jak vložit HTML entitu do contentu od pseudo-elementu"
date:   2016-05-05 16:57:51
tags:
- Pseudo-element
- HTML
- CSS3
---

## Pseudo-element a pseudo-třídy
Pokud jste ještě neslyšeli o *pseudo-elementech*, jsou něco podobného jako *pseudo-třídy*. *Pseudo-třídy* přidávají styl na základě určitého stavu elementu. _Pseudo-elementy_ naopak spíše umožňují přidat styl určitým částem elementu, na kterého ukazují. Teorie je sice hezká, ale často hodně matoucí pokud programátor nevidí konkrétní příklad. Pojďme se na něj podívat

<iframe width="100%" height="300" src="//jsfiddle.net/emilcieslar/k0t1y357/embedded/html,css,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Na předchozím příkladu můžete vidět, že _pseudo-třída_ `:hover` mění barvu elementu `a`, kdežto _pseudo-element_ `::after` přidává styl za určitou část elementu (nezávisle na stavu tohoto elementu), konkrétně v tomto případě na konec elementu `a`. Nakonec jsem ještě přidal příklad _pseudo-elementu_ v kombinaci s _pseudo-třídou_ (protože tyto dvě *pseudo* můžeme také kombinovat).

## Pseudo-element a vložení HTML entity
Abych se konečně dostal k jádru věci, co kdybychom chtěli do předchozího příkladu přidat _HTML entitu_, např. __&rarr;__ (opravdovou šipku) místo -> (falešné šipky), kterou tam momentálně máme? První věc, co by programátora mohla napadnout je vložit `&rarr;` takto `content: '&rarr;'`. Zde ale narazíte, protože CSS nepoužívá kódování entit jako HTML. Proto, abychom mohli takovou šipku ukázat, potřebujeme dvě věci:

1. Nejprve potřebujeme zjistit desetinnou hodnotu naší _entity_, [můžeme ji nalézt například zde](https://dev.w3.org/html5/html-author/charref). Desetinná hodnota je to poslední číslo když přejedete myškou přes _html entitu_. Pro `&rarr;` je to například `8594`.
2. Toto desetinné číslo vložíme do políčka `Numeric Value` na [této stránce](http://www.evotech.net/articles/testjsentities.html) a v políčku `CSS Value` dostaneme, co potřebujeme. V našem případě šipky je to hodnota `\2192`.

Hodnotu `\2192` použijeme uvnitř `content` následovně: `content: '\2192';`. Pojďme se podívat na náš první příklad upravený, aby obsahoval _html entitu_.

<iframe width="100%" height="300" src="//jsfiddle.net/emilcieslar/de23vcvc/embedded/html,css,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## Závěr
Tak to bychom měli. Doufám, že používání _html entit_ uvnitř _pseudo elementů_ je již jasnější. Kdybyste měli jakékoliv dotazy, zanechte komentář.
