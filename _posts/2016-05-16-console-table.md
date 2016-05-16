---
layout: post
title: 'Užitečný console.table()'
subtitle: 'Jak přehledně zobrazit data v consoli'
date: 2016-05-16 14:00
tags:
- console.table()
- Chrome Dev Tools
---

Věřím, že jste se mnohokrát setkali s tím, že jste chtěli zobrazit data v konzoli. Např. pro kontrolu, co máte uvnitř objektu. Klasickou cestou byste napsali asi následující kód.

{% highlight js %}
var User = function User(name, age) {
    this.name = name;
    this.age = age;
};

var emil = new User('Emil', 26);
var jakub = new User('Jakub', 25);
var karel = new User('Karel', 42);

var users = {
    emil: emil,
    jakub: jakub,
    karel: karel
};

console.log(users);
{% endhighlight %}

<img style="max-width: 352px" src="{{ site.github.baseurl }}/assets/img/posts/2016-05-16-console-table-1.png" alt="Console table" />

Tímto způsobem ale musíte každou část objektu otevírat, abyste si data jednotlivých objektů zobrazili. Proč to ale neudělat jednodušeji a přehledněji? Stačí malá úprava. Změna `console.log()` na `console.table()` a všechna data se nám krásně zobrazí v tabulce.

{% highlight js %}
var User = function User(name, age) {
    this.name = name;
    this.age = age;
};

var emil = new User('Emil', 26);
var jakub = new User('Jakub', 25);
var karel = new User('Karel', 42);

var users = {
    emil: emil,
    jakub: jakub,
    karel: karel
};

console.table(users);
{% endhighlight %}

Pokud tedy vložíte předchozí kód do vašeho .js souboru a podívate se do konzole, budete velmi překvapení přehledností.

<img style="max-width: 522px" src="{{ site.github.baseurl }}/assets/img/posts/2016-05-16-console-table-2.png" alt="Console table" />

## Reference
1. [https://egghead.io/lessons/javascript-logging-pretty-printing-tabular-data-to-the-console](https://egghead.io/lessons/javascript-logging-pretty-printing-tabular-data-to-the-console)
2. [https://developers.google.com/web/tools/chrome-devtools/debug/console/structured-data?hl=en](https://developers.google.com/web/tools/chrome-devtools/debug/console/structured-data?hl=en)
