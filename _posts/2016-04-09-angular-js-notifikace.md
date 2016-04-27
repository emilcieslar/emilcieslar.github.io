---
layout: post
title:  "AngularJS Notifikace"
subtitle: "Znovupoužitelná komponenta pro zobrazení notifikací uživateli"
date:   2016-04-09 16:57:51
tags:
- AngularJS
- JavaScript
- HTML
---

## Motivace
Velmi často se stává, že v SPAs (Single Page Applications) chcete zobrazit informaci uživateli když se nějaká akce zdaří (nebo nezdaří). Ku příkladu uživatel klikne na tlačítko uložit a ve chvíli kdy se všechny data na serveru uloží, chtěli byste uživatele informovat, že se uložení zdařilo. Např. zprávou *Úspěšně uloženo*.


## Angular EC Callout
Proto jsem se rozhodl vytvořit si znovupoužitelnou komponentu, tzv. callout, který takové zprávy bude uživateli zobrazovat a v každém projektu si ho mohu nastylovat podle libosti. Nejlepší na tom je, že ho můžete použít i vy. Nejprve se podívejte na [velmi jednoduché demo](http://emilcieslar.github.io/angular-ec-callout/).

### Dependency Injection
Jako úplně první věc, na kterou se často opomíná, je přidání závislosti angular-ec-callout ve vaší AngularJS aplikaci, jako na následujícím příkladu.

{% highlight js %}
angular.module('yourApp', ['angular-ec-callout'])
{% endhighlight %}

### Directive
Základ této komponenty je directive, která zobrazuje jednotlivé statusy (pokud jejich více) nebo pouze jeden status. Directive použijete kdekoliv ve své aplikaci, samozřejmě podle toho, kde chcete, aby se zobrazovala a také podle nastylování celé aplikace.

{% highlight html %}
<ec-callout></ec-callout>
{% endhighlight %}

### Service (služba)
Následně použijete službu ecCalloutService, pomocí které pošlete "notifikaci", aby byl nový status přidán. Toto můžete provést kdekoliv ve vašem kódu, např. ve vašem Controlleru.

{% highlight js %}
.controller('yourController', ['ecCalloutService', function(CalloutService) {
  CalloutService.notify({
    type: 'alert',
    message: 'Alert callout!',
    img: 'https://cdnjs.cloudflare.com/ajax/libs/foundicons/3.0.0/svgs/fi-alert.svg',
    timeout: 2000, // This callout will be removed in 2 seconds
    delete: false // setting this option to true would remove all displayed callouts
  });
}]);
{% endhighlight %}

## Ke stažení
Angular EC Callout se nejlépe instaluje přes bower (za pomocí příkazu `bower install angular-ec-callout --save`), můžete si samozřejmě ale stáhnout celý script a pouze ho uložit k vám do js složky. Nezapomeňte ale také stáhnout `angular-animations`, protože je tato komponenta používá pro zobrazení a schování statusů. Více informací se můžete dočíst na [github stránce](https://github.com/emilcieslar/angular-ec-callout).
