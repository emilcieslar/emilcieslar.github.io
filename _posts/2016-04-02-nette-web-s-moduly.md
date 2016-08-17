---
layout: post
title:  "Nette web s moduly"
subtitle: "Jak nastavit nette aplikaci, aby měla moduly pro běžného uživatele a pro admina"
date:   2016-04-02 16:57:51
tags:
- Nette
- PHP
---

Při svých začátcích v Nette jsem nemohl najít článek o tom, jak postavit nette aplikaci na modulech pro běžného uživatele a pro administrátora, který by nebyl tzv. 'out of date', jednoduše řečeno starý. Proto jsem se rozhodl napsat *jednoduchý* návod, jak začít.

## Nette moduly
Hezký, ale až moc zjednodušený přehled o modulech nabízí přímo [dokumentace nette](https://doc.nette.org/cs/2.3/presenters#toc-moduly), ale jednak mi tento úsek příjde trochu neaktualizovaný (např. složka `templates` už v nette 2.3 je standardně součástí `presenters`), druhak je až moc zjednodušený a všeobecný pro začátečníky, kteří nerozumí ještě úplně struktuře nette. Proto blíže nastíním, jak tyto moduly zprovoznit.

### Struktura
Důležitá je adresářová struktura ve složce app:

{% highlight php %}
├── AdminModule/
│   ├── presenters/
│   │   ├── templates/
│   │   ├── @layout.latte
├── FrontModule/
│   ├── presenters/
│   │   ├── templates/
│   │   ├── @layout.latte
...
{% endhighlight %}

### Jednotlivé presentery v modulech
Na co je třeba dát velký pozor jsou namespacy jednotlivých presenterů v modulech. Co tím myslím? Tak kupříkladu takto by mohl vypadat ukázkový BasePresenter.php ve FrontModule:

{% highlight php linenos %}
<?php

namespace FrontModule; // FrontModule !!!!!

use Nette;

abstract class BasePresenter extends Nette\Application\UI\Presenter
{

  public function __construct()
  {

  }

}
{% endhighlight %}

Nejdůležitější je řádek 3, `namespace FrontModule;`. Tento řádek musí být v __KAŽDÉM__ presenteru ve FrontModulu. Možná už tušíte, že naopak v `AdminModule` tento řádek bude namespace `AdminModule;`. Na tyto detaily je třeba dát pozor, protože jinak vás bude bolet hlava z chyb, se kterými se setkáte.

### Config
Dále nesmíme opomenout na application mapping v config souboru. Defaultně máme nastaveno následovně.

{% highlight neon %}
application:
	errorPresenter: Error
	mapping:
		*: App\*Module\Presenters\*Presenter
{% endhighlight %}

Config ale změníme takto:

{% highlight neon %}
application:
	errorPresenter: Error
	mapping:
		*: *Module\*Presenter
{% endhighlight %}

Touto malou, ale důležitou úpravou zajistíme, že se moduly budou správně načítat. Dejte pozor také na konfiguraci `errorPresenter`, který momentálně směřuje do slepé uličky vzhledem k tomu, že máme nakonfigurované moduly. Pokud chceme mít společný `errorPresenter` v jednom modulu (např. ve `FrontModule`), změníme konfiguraci následovně.

{% highlight neon %}
application:
	errorPresenter: Front:Error
	mapping:
		*: *Module\*Presenter
{% endhighlight %}

Na [nette foru](https://forum.nette.org/cs/) můžete nalézt i řešení `errorPresenteru` pro každý modul zvlášť, to už ale nechám na vás.

### RouterFactory
Poslední věcí je routování. Abychom byli schopni nasměrovat uživatele na dané moduly (a jejich presentery), potřebujeme několik úprav v našich routách. Následující kód ukazuje, jak by naše routy mohly vypadat.

{% highlight php linenos %}
<?php

namespace App;

use Nette;
use Nette\Application\Routers\RouteList; // Nezapomeňte přidat RouteList!!!
use Nette\Application\Routers\Route;


class RouterFactory
{

	/**
	 * @return Nette\Application\IRouter
	 */
	public static function createRouter()
	{
		$router = new RouteList;

		$admin = new RouteList('Admin');
		$admin[] = new Route('admin/<presenter>/<action>[/<id>]', 'Homepage:default');

		$front = new RouteList('Front');
		$front[] = new Route('<presenter>/<action>[/<id>]', 'Homepage:default');

		$router[] = $admin;
		$router[] = $front;

		$router[] = new Route('<presenter>/<action>[/<id>]', 'Homepage:default');

		return $router;
	}

}
{% endhighlight %}

V podstatě si vytvoříme dvě různé `RouteListy`, jednu pro Admina a druhou pro Front (zákazníka) a pak je přidáme do routeru. Nakonec ještě přidáme tzv. *fallback route*, jinými slovy, kdyby nic neklaplo, tak to půjde do Homepage:default. Malý detail na dovysvětlenou, pomocí new RouteList('Admin') specifikujeme název modulu. Takže module se musí jmenovat `AdminModule`, pokud ho chceme v RouterListu specifikovat jako Admin. Nemůžeme ho pojmenovat například `ModuleAdmin` nebo `AdminM`, prostě `AdminModule`. V začátcích jsem si totiž hrál s myšlenkou pojmenovávat moduly např. `ModuleAdmin` a `ModuleFront`, aby to hezky šlo v abecedě za sebou v adresářové struktuře, to byla ale zásadní chyba.

## Bonus
Abych vám ušetřil čas, tak jsem připravil boilerplate ([který naleznete na mém githubu](https://github.com/emilcieslar/nette-and-foundation-for-sites)) pro aplikaci s moduly. Je to ale spíše pro mé potřeby, takže to neušetří čas každému. Obsahuje totiž nejen nette, ale přidal jsem [Foundation for Sites](http://foundation.zurb.com/sites/docs/) a AngularJS. Foundation mám rád, protože šetří hodně času. Na rozdíl od Bootstrapu je čistější a je i menší (a taky přístupěnější). Neobsahuje tolik stylů, takže se hodí spíše pro projekty, kdy nechcete přepisovat tisíc předdefinovaných stylů od bootstrapu, ale potřebujete nadesignovat web app podle svých představ od začátku. Stačí vytvořit nový projekt pomocí příkazu `git clone`:

{% highlight bash %}
git clone https://github.com/emilcieslar/nette-and-foundation-for-sites.git
{% endhighlight %}


## Reference

1. [Nette dokumentace](https://doc.nette.org/cs/2.3/presenters#toc-moduly)
2. [Jak na nette s funkčními moduly](http://www.egoblog.cz/jak-na-nette-s-funkcnimi-moduly/)
3. [Nette forum](https://forum.nette.org/cs/25221-nechce-nacist-homepresenter-v-adminmodule)
