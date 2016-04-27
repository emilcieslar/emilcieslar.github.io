---
layout: post
title:  "Nevybíravé (asynchronní) chování console.log"
subtitle: "Jak neztratit hlavu při debuggování pomocí console.log()"
date:   2016-04-26 16:57:51
tags:
- console.log
- JavaScript
---

Možná se už vám stalo následující. V pohodě si debuggujete pomocí `console.log()` a najednou příjdete na to, že `console.log()` nevyplivne to, co byste od něj v daném momentě čekali. Počítáte totiž s tím, že `console.log()` by měl odrážet přesně ten moment, ve kterém jste ho použili v kódu. Nemusí tomu tak vždycky být, pojďme se na takovou situaci podívat.

Zvažte tento kód:

{% highlight js %}
Person = {
	name: 'Emil Cieslar'
};

console.log(Person);

Person.age = 26;

console.log(Person);
{% endhighlight %}

Co byste čekali, že vytiskne první `console.log()` a co ten druhý? Možná hádáte, že ten první vytiskne objekt `Person` obsahující pouze `name`. Bohužel tomu ale tak není. První, stejně tak jako druhý vytiskne objekt `Person` obsahující jak `name`, tak `age`. Proč je tomu tak?

Při tisknutí tohoto objektu do konzole může proběhnou určité zpoždění, během kterého se již do objektu `Person` přiřadil i `age`. Přesněji řečeno, konzole může fungovat *asynchronně*.

### Tisíc prohlížečů, tisíc názorů
Tento nečekaný efekt se nemusí stát vždy. Vzhledem k tomu, že `console.log()` není součástí specifikace `JavaScriptu`, naopak je definován takzvaným *hosting environment*, což je v podstatě prohlížeč. Každý prohlížeč si ho může definovat jak chce. A tak například v Chrome je `console.log()` asynchronní, tzn. že neproběhne okamžitě když ho zavoláme, ale je přiřazen do smyčky procesů (`event loop`) a čeká až na něho příjde řada (důvodem je I/O, které je pomalé a mohlo by blokovat prohlížeč). Mezi tím se ale může `Person` objekt změnit a tím pádem se jeho změny projeví i v `consoli`. Zajímavé, že? Je tady ale řešení?

### Řešení

Pokud nám opravdu záleží na tom, co v daném okamžiku objekt `Person` obsahuje, musíme ho vytisknout pomocí `JSON.stringify()` funkce, která prakticky převede objekt v daném momentě do JSON řetězce a tím zajistí, že uvidíme jeho obsah v daném momentu. Zvažte následující kód.

{% highlight js %}
Person = {
	name: 'Emil Cieslar'
};

console.log( JSON.stringify(Person) );

Person.age = 26;

console.log(Person);
{% endhighlight %}

Nevýhodou `JSON.stringify()` metody je, že vytiskne objekt vcelku škaredě, přesněji takto:

{% highlight js %}
{"name":"Emil Cieslar"}
{% endhighlight %}

Představa, že debugujete objekt, který má mnohem více údajů, tak se v tom člověk rychle ztratí.

### Řešení no. 2

Použijte JavaScript debugger a breakpointy. O tom ale více příště.
