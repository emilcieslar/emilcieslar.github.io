---
layout: post
title:  "Nevybíravé chování console.log "
subtitle: "Jak neztratit hlavu při debuggování pomocí console.log()"
date:   2016-04-26 16:57:51
tags:
- console.log
- JavaScript
---

Možná se už vám stalo následující. V pohodě si debuggujete pomocí `console.log()` a najednou příjdete na to, že `console.log()` nevyplivne to, co byste od něj čekali. Pojďme se na takovou situaci podívat.

Zvažte tento kód:

{% highlight js %}
Person = {
	name: 'Emil Cieslar'
};

console.log(Person);

Person.age = 26;

console.log(Person);
{% endhighlight %}

Co byste čekali, že vytiskne první `console.log()` a co ten druhý? Možná hádáte, že ten první vytiskne objekt Person s jednou property a to `name`. Bohužel tomu ale tak není. První, stejně tak jako druhý vytiskne objekt `Person` obsahující jak `name`, tak `age`. Proč je tomu tak?

Při tisknutí tohoto objektu do konzole proběhne určité zpoždění, během kterého se již do objektu `Person` přiřadil i věk. Dalo by se tedy říci, že konzole vytiskne v podstatě živou referenci na objekt `Person` a tudiž změny po tisku na tomto objektu se objeví i v konzoli.

### Řešení?

Pokud nám opravdu záleží na tom, co v daném okamžiku objekt `Person` obsahuje, musíme ho vytisknout pomocí následujícího kódu.

{% highlight js %}
Person = {
	name: 'Emil Cieslar'
};

JSON.stringify(Person);

Person.age = 26;

console.log(Person);
{% endhighlight %}
