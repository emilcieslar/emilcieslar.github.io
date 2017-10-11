---
layout: post
title:  "Ember-CoffeeScript-React"
subtitle: "Jak jsem poznal Ember, EmberData, CoffeScript, Emblem a React (a trochu Redux)"
date:   2017-10-11 15:00:00
tags:
- Ember
- EmberData
- CoffeeScript
- Emblem
- React
- Redux
---

Tento článek se zaměřuje na projekt, na kterém v poslední době pracuji a na technologie, které při tom používám.

Zkratky použité v článku:
* JS = JavaScript
* CS = CoffeeScript
* FE = Frontend
* BE = Backend

__TL;DR__

Už tomu bude rok, co jsem zde nepřidal žádný nový příspěvek. Rok 2017 přinesl mnoho změn a proto mi nezbývalo moc času psát nové články. Jedna z nejdůležitějších změn, která výrazně omezila mé časové možnosti je práce na novém projektu. Dostal jsem nabídku spolupracovat s jedním nejmenovaným izraelským startupem na jejich již běžícím projektu. Startup už je na celosvětlovém trhu již několik let a tak projekt od svého prvopočátku prošel mnoha změnami a refaktoringy. Přišel jsem tedy k hotové věci, která velmi často obsahovala legacy kód. Trvalo mi několik týdnů než jsem se zorientoval a naučil potřebné frameworky a tooly, které startup používá. Tento článek bych chtěl zaměřit právě na ně.

## Ember + Ember Data
Začalo to s Emberem. Ember je jeden z mnoha JS frameworků, který řeší stejný problém: jak na interakci s uživatelem bez obnovení stránky a zároveň udržet projekt čitelný. Někdo by mohl namítnout, že se to dá napsat s jQuery. Samozřejmě, že dá, ale věřte mi, čím je projekt větší, tím rychleji se začnete v kódu ztrácet a začnete hledat cesty jak mít kód více modulárnější. Ember je ale specifický v tom, že je velmi "opiniated". Jinými slovy jeho autoři rozhodli hodně věcí za vás. Např. je předem stanovená struktura projektu. Ve chvíli kdy musíte pracovat s více projekty najednou budete toto považovat za opravdovou výhodu, protože se okamžitě zorientujete. Dále nastavuje dopředu určitý přístup k testování. Hezky rozděluje unit, integration a acceptance testování a vysvětluje jak k jednotlivým vrstvám testování přistupovat. Pro vývojáře, kteří si nechtějí lámat hlavu s tím, jak nastvit webpack nebo gulp, aby jim vůbec běžel kompilační process a mohli na projektu začít pracovat. Nebo pro vývojáře, kteří si nemohou vybrat v té směsi dostupných FE řešení je tento framework vysvobozením. Má vše přednastavené a nabízí celkem slušnou rozšířitelnost ve formě pluginů. Tato rozšířitelnost není ale nekonečná. V momentě, kdy chcete svůj Ember projekt hodně upravit podle svých specifických potřeb tak narazíte. Snaží se vás držet v určitých kolejích což u rostoucích projektů může být negativem a často je. Shrnuto a podtrženo, s týmem často narážíme na sturkturu a build process Emberu když chceme jít svou cestou. Projekt ale dosáhl velkých rozměrů a je těžké se vracet zpět nebo předělat projekt na úplně nový framework (a který je nakonec ten správný?). Takže pro začátek je to skvělý framework, ale pro hodně custom projekty to nemusí být správná volba.

Jenom malá odbočka k Ember Data. Je to skvělá záležitost pokud máte skvělý tým BE vývojářů, kteří dokážou hezky zpracovat API. Budete pak schopní namapovat Ember na toto API a Ember Data se postará o práci s daty. Je to krásná vrstva, která udělá hodně práce za vás, jako např. one-to-many mappingy.


## Emblem
Emblem prostě ve svém projektu nechcete. Je to templatovací framework. Představte si následující HTML kód.
{% highlight html %}
<div>
  <p>
    <span>Ano</span>
  </p>
</div>
{% endhighlight %}
A teď v Emblemu.
{% highlight html %}
div
  p
    span Ano
{% endhighlight %}
Vypadá to sice fajn, taky jeho hlavní myšlenkou je zjednododušit a zkrášlit HTML templaty, ale upřímně řečeno raději budu dále psát HTML než se neustále topit ve směsi chyb, na které narazíte často a dokumentace je bohužel velmi omezená. Zvláště když kombinujete Emblem s Emberem, který v základu používá handlebars jako templaty. Už se mi stalo, že jsem hledal ve zdrojovém kódu Emblemu, abych našel jak něco funguje. Na stackoverflow rovnou zapomeňte. Rozhodně nedoporučuji.

¨¨
Vypadá to sice fajn, taky jeho hlavní myšlenkou je zjednododušit a zkrášlit HTML templaty, ale upřímně řečeno raději budu dále psát HTML než se neustále topit ve směsi chyb, na které narazíte často a dokumentace je bohužel velmi omezená. Zvláště když kombinujete Emblem s Emberem, který v základu používá handlebars jako templaty. Už se mi stalo, že jsem hledal ve zdrojovém kódu Emblemu, abych našel jak něco funguje. Na stackoverflow rovnou zapomeňte. Rozhodně nedoporučuji.

## CoffeeScript
Poslední věcí, která pro mě byla na začátku novinkou byl CS. Vzhledem k tomu, že BE projektu je napsán v Pythonu a velká část týmu jsou full stack vývojáři, oblíbili si tuto podivuhodnou formu JSka, která se kompiluje do čistého JSka. Nevím ale jestli podivuhodné není moc silné slovo, protože jestli jste někdy zažili [LiveScript](http://livescript.net/), tak víte o čem mluvím když používám slovo podivuhodné. Každopádně zpět k CS. podle autorů CS kompiluje kód do ještě rychlejšího kódu než by ho napsal člověk. Do jisté míry je to pravda, ale jsou to většinou takové mikro optimalizace, které nemají v běžné aplikaci praktický žádný efekt. Mikro optimalizace např. ve formě inicializace proměnné ve for loopu `var i = 0;` na začátek scope. No prostě detaily, na které se někteří programátoři velmi rádi zaměřují. Zapomínají ale na ty základní věci a to je architektura projektu. Bez kvalitní architektury jsou tyto mikro optimalizace k ničemu. Zase zpět k CSku. CSko se dá napsat, pokud se hodně snažíte (a věřte mi, někteří programátoři se hodně snaží), v naprosto nečitelné formě. Programátor, který to po vás čte už rozbíjí druhý monitor než pochopí, co jeden řádek vlastně provádí. CSko jde cestou hodně zhuštěného kódu do jednoho řádku. Já zastávám ale názor, že lépe napsat 10 řádků čitelného kódu s komentáři než jeden řádek nečitelného žvástu, který chápe jenom člověk, který ho napsal (a chápe ho jenom po dobu jednoho měsíce, pak už také ne). Další stránkou CSka jsou sourcemapy. Někdy se v devtools v Chrome načítají i několik sekund na čtyřjádrovém macu (po celodenní zátěži). Není nic lepšího než čekat na sourcemap až obnovíte stránku. Dále, všechny funkce v CSku jsou anonymní, takže debugging je někdy opravdu bolestivý. Upřímně si nemyslím, že výhody CSka převažují ty starosti, které s ním jsou. Já osobně v něm tolik výhod nevidím, takže nedoporučuji.


## Nová éra
Hodně headhunterů láká programátory na greenfield projekty, ve kterých se můžete vyhrát. Asi záleží na úrovni a zkušenostech, ale myslím si, že pokud nejste naprosto dokonalý profesionál s 20 letou praxí, tak na greenfieldu se nenaučíte tolik, co na existujícím projektu. Je až k neuvěření kolik nových věcí se právě naučíte na projektech, které už nějakou dobu běží. Vidíte na nich jaké architektonické chyby a rozhodnutí programátoři udělali a můžete se z nich poučit. Pokud máte dost energie a sílu protlačit své nápady, můžete tyto chyby napravit a na tom se člověk naučí nejvíce.

Nevím jestli jsem měl jenom štěstí, přiel jsem ve správnou dobu nebo jsem byl ve svých názorech neoblomný a dostatečně přesvědčivý, v každém případě jsem si prosadil několik věcí, které posunuly projekt kupředu (alespoň doposud jsem o tom stále přesvědčený).

## SASS
Jedna z věcí byl sass. Projekt byl doslova zasypán nepřehledným bordelem napsaným v sassu. Nepoužval se sass linter, proměnné byly velmi náhodně definovány a jeden selektor přepisoval druhý. I když tým následoval BEM nebyli v něm moc striktní (bohužel nejsou doposud, ale když je někdo tvrdohlavý...). To samozřejmě vede ke spoustě problémům se specificitou. Takže celý systém BEM nakonec spadne a je prakticky k ničemu.

Naštěstí se mi podařilo prosadit zásadní refaktoring proměnných, nasadit sass lint (a opravdu ho dodržovat) a nakonec jsem stanovil jasnou strukturu souborů a složek, která se musí dodržovat. Výsledkem byl mnohem přehlednější sass, ve kterém píšete nový kód s větší radostí. Na druhou stranu toto se mi podařilo zatím pouze v jednom z projektů. Ještě mám před sebou dlouhou cestu k lepšímu sassu.

## Testy
Člověk by si řekl, testy na FE? :) K čemu? V dobách, kdy se psal FE v jQuery bych si řekl asi to samé.
