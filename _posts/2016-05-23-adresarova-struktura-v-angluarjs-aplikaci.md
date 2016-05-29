---
layout: post
title:  "Adresářová struktura v AngularJS aplikaci"
subtitle: "Jak rozdělit soubory do adresářů v AngularJS aplikaci, abyste se v nich později neztratili."
date:   2016-05-23 16:57:51
tags:
- AngularJS
---

Když začínáte vyvíjet novou aplikaci nebo spravujete již běžící, vždy nastane otázka jakým způsobem rozdělit kód do složek a souborů, abychom se později v naší struktuře neztratili.


## 1. Rozdělení podle typu souboru
Možná už jste viděli přístup, ve kterém se jednotlivé *controllery*, *directivy*, atd. rozdělují do složek podle svého typu. Takže dopadnete asi s následující adresářovou strukturou.

{% highlight javascript %}
├── js/
│   ├── app.js // Entry file
│   ├── controllers/
│   │   ├── auth-controller.js
│   │   ├── user-profile-controller.js
│   │   ├── shopping-cart-controller.js
...
│   ├── directives/
│   │   ├── shopping-cart-directive.js
...
│   ├── services/
│   │   ├── shopping-cart-service.js
│   │   ├── auth-service.js
...
{% endhighlight %}

Takto rozdělovat soubory do adresářů podle typu se hodí v malých aplikacích, které nebudou moc růst. V podstatě definujeme jeden modul (aplikaci) a všechny soubory budeme do tohoto modulu vkládat. Např. takto by vypadal náš `js/app.js` soubor a následně `js/controllers/auth-controller.js` soubor.

{% highlight javascript %}
// js/app.js
angular.module('shoppingApp', []);

// js/controllers/auth-controller.js
angular.module('shoppingApp')

	.controller('authController', [function() {

		var self = this;

		// Tady by byl authController kód...

	}]);
{% endhighlight %}

Ve chvíli kdy naše aplikace začne růst, pomalu bychom se začali ztrácet v jednotlivých složkách. Přece jenom představme si aplikaci, ve které máme 50 *controllerů* a každý z nich plní určitý účel. Kdybychom pak chtěli najít *service*, která plní podobný účel jako náš určitý *controller*, asi bychom začali chápat, že tento přístup není ideální.

## 2. Rozdělení do modulů
Určitý účel, který jednotlivé *controllery*, *servicy*, atd. plní by se dal nazvat modulem. Modul pro každou situaci. Tento přístup je doporučovaný při vývoji středně velkých až velkých aplikací. Vytvořit si složky podle modulů a do nich pak vkládat jednotlivé soubory. Pojďme se podívat na přepracovanou strukturu naší aplikace.

{% highlight javascript %}
├── js/
│   ├── app.js // Entry file
│   ├── auth/
│   │   ├── auth.js
│   │   ├── auth-controller.js
│   │   ├── auth-directive.js
│   │   ├── auth-service.js
│   │   ├── user-profile-service.js
│   ├── shopping-cart/
│   │   ├── shopping-cart.js
│   │   ├── shopping-cart-controller.js
│   │   ├── shopping-cart-directive.js
│   │   ├── shopping-cart-service.js
...
{% endhighlight %}

Nyní již můžeme hezky vidět jak by se soubory mnohem lépe hledaly. Víme, že zrovna hledáme funkcionalitu týkající se uživatele a přihlášení, tak jdeme do složky `auth`. Víme, že chceme upravovat nákupní košík, jdeme do složky `shopping-cart`, atd. Jednotlivé moduly plní svůj účel a pouze definujeme hlavní modul v souboru `js/app.js` a ostatní moduly vložíme. Pojďme se podívat, jak by to vypadalo v kódu.

{% highlight javascript %}
// js/app.js
angular.module('shoppingApp', ['authModule', 'shoppingCartModule']);

// js/auth/auth.js
angular.module('authModule', []);

// js/auth/auth-controller.js
angular.module('authModule')

	.controller('authController', [function() {

		var self = this;

		// Tady by byl authController kód...

	}]);

// js/auth/auth-directive.js
angular.module('authModule')

	.directive('auth', [function() {

		return {
			// Vas kod
		}

	}]);
{% endhighlight %}

V `js/app.js` opět definujeme naši `shoppingApp`, tentokát ale vložíme závilosti (`authModule`, `shoppingCartModule`, ...). Máme tak mnohem přehlednější aplikaci, která navíc používá (pokud možno) soběstačné moduly. Tím pádem můžeme jednoduše vzít jeden modul z naší aplikace a vložit ho do jiné aplikace.

## Závěr
Věřím, že po tomto článku je o malinko zřetelnější jak se přistipuje k adresářové struktuře v AngularJS aplikacích. Pokud by přece jenom bylo cokoliv nejasné, nebojte se zeptat v komentářích. Stejně tak v případě, že používáte jinou adresářovou strukturu a naprosto odlišný přístup, rád se něco nového naučím.
