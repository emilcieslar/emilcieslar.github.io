---
layout: post
title: 'Angular directive scope'
subtitle: 'Scope proměnných v AngularJS directive'
date: 2016-04-30 20:00
tags:
- AngularJS
- Directives
- JavaScript
---

Nevěděl jsem jak toto téma pojmout v češtině, protože pro slovíčka jako `scope` nebo `directive` se horko těžko hledají české výrazy. Proto jsem to raději v češtině nepojal a `directive` budu prostě říkat *directive* a `scope` budu říkat *scope*. Jinak bychom se z toho slovíčkaření asi zbláznili.

## Scope
Co je vlastně *scope* všeobecně v programování? Abychom tento termín pochopili, pojďme se podívat na následující kód.

{% highlight js %}

var myVarOutside = 'hello from outside';

var myFunc = function myFunc() {

    var myVarInside = 'hello from inside';

    console.log(myVarOutside, myVarInside); // Toto vytiskne hello from outside, hello from inside

}

console.log(myVarOutside, myVarInside); // Toto vytiskne hello from outside, undefined (nebo ReferenceError, záleží jestli jedete ve strict módu)
{% endhighlight %}

V předchozím kód můžete vidět, že `myVarOutside` má *scope* mimo funkci `myFunc` a tudíž se vytiskne jak uvnitř funkce, tak i mimo ní. Naopak proměnná `myVarInside` má *scope* uvnitř funkce `myFunc` a tudíž při pokusu vytisknout tuto proměnnou mimo *scope* naší funkce dostaneme chybu na oplátku. `myFunc` má totiž uzavřenou *scope* a proměnná definovaná uvnitř nemůže být použita mimo tuto funkci. Mimo znamená ve *vyšší* *scope*. Kdybychom definovali další funkci uvnitř funkce `myFunc`, tak uvnitř té funkce tato proměnná bude fungovat, protože se dostaváme *hlouběji*.

## Directive Scope
U každé *directive* v angularu můžeme definovat její *scope*. *Scope* u *directive* je ale trochu složitější než jsme před chvíli nastínili. Přesněji jsou zde tři druhy *scope*, které můžeme u *directive* definovat. Abychom mohli tyto tři příklady hezky nastínit, vytvoříme si velmi jednoduchou aplikaci, na které si všechno ukážeme. Aplikace bude zaměřená na zobrazování počasí, takže ji nazveme WeatherApp. Začneme rovnou s jednoduchým controllorem a šablonou, na které si zobrazíme počasí.

<iframe width="100%" height="300" src="//jsfiddle.net/emilcieslar/zy6jnqxm/embedded/js,html,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

V předchozím kódu jsme si definovali nový module `WeatherApp` a také vytvořili `mainController`, který má objekt `currentWeather` obsahující data o počasí. Dále také obsahuje proměnnou typu *string* `windSpeed`, která má doprovodnou informaci o větru. Také jsme si vytvořili šablonu, ve které se zobrazuje aktuální počasí. Jako bonus je pod počasím formulář, pomocí kterého můžeme měnit teplotu a rychlost větru. Zatím ale nemáme zapojené do práce žádné *directives*. Pojďme se tedy nyní podívat na tři druhy *scope*, které můžeme na *directive* nastavit a jak by náš příklad upravily.

### 1. Scope: false

První nastavení je defaultní (pokud nenastavíme nic jiného) v podstatě říká, že vše z venkovní *scope* bude přístupné uvnitř *directive* a můžeme s tím manipulovat. Zde si musíme dát velký pozor, protože pokud změníme uvnitř *directive* cokoliv z venkovní *scope*, tak změna se v angularu projeví i venku. Pojďme se podívat na příklad rozvíjející se z naší malé aplikace.

<iframe width="100%" height="300" src="//jsfiddle.net/emilcieslar/whh9kaxm/embedded/js,html,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Jak můžete vidět, přesunuli jsme formulář dovnitř naší *directive* a stále můžeme pomocí něho manipulovat s proměnnými venku, které jsou vyznačené zeleným okrajem. Takže například pokud změníme hodnotu větru na 30, tak se změní i v popisu, který je úplně mimo naši *directive*. Tento přístup je nejméně doporučovaný co se týče programování *directive*, protože, jakákoliv úprava zděděné *scope* se projeví i v rodiči. Kdybychom např. v *directive* vytvořili novou proměnnou `windDirection` v rámci stejné *scope*, objevil by se i v *rodičovské scope*. To by mohlo mít nečekaný efekt, protože bychom mohli přepsat již jiné existující proměnnou. Za druhé, pokud se změní název objektu např. z `currentWeather` na `weather`, musíme změnit tento název i v *directive*, abychom s proměnnou mohli dále pracovat. Při představě velké aplikace s několika desítkami *directives* se naše aplikace může lehce stát neudržitelnou (člověk by se zbláznil, aby si všechno hlídal).


### 2. Scope: true
Následně je tady druhá varianta, která zjemňuje první variantu tím, že vytváří novou *scope* pro naši *directive*, ale stále zároveň dědí celou *scope* od rodiče, tzn. v naší applikaci od `mainControlleru`. Problém s neudržitelností názvů proměnných zůstává. Jeden bonus tato *scope* ale má a to ten, že pokud do *scope* cokoliv přidáme nebo změníme (proměnnou, funkci, ...), tento dodatek se neobjeví ve *scope* od *rodiče*, v tomto případě `mainControlleru`. To prakticky znamená, že to nebude mít nežádoucí efekt někde jinde v aplikaci. Pojďme se podívat na prakticky úplně stejný příklad, pouze s novým nastavením `scope: true`.

<iframe width="100%" height="300" src="//jsfiddle.net/emilcieslar/sa7dmf3u/embedded/js,html,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Pokud nyní zkusíte upravit *rychlost větru*, tak se změna ve venkovní *scope* neprojeví. Je tady ale jeden chyták. V případě, že zkusíte upravit *teplotu*, tak se změna projeví, jak to? To nechám na zvídavosti čtenáře (takový malý kvíz). Když si nebudete vědět rady, stačí napsat komentář a dovysvětlím. Každopádně nyní by mělo být trochu zřetelnější jaký je rozdíl mezi *false* a *true*.

### 3. Scope: {}
Nakonec je zde doporučovaná varianta, která prakticky izoluje *scope* od naší directive. Jinými slovy vytváří izolovanou *scope*, což znamená, že nic implicitně nedědí z `mainControlleru`. Dědí pouze pokud explicitně řekneme, že chceme nějaké hodnoty předat (jak, to si za chvíli ukážeme). Tím zamezuje (z velké části – za chvíli se k tomu vrátím) jak problému s názvy proměnných, tak změnám, které se projeví na venek (což už prakticky zamezilo i `scope: true`). Pojďme se podívat na kód.

<iframe width="100%" height="300" src="//jsfiddle.net/emilcieslar/852o3ckg/embedded/js,html,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Proč jsem zmínil, že zamezuje *z velké části* a ne úplně? Je sice hezké, že máme znovu definovaný název našeho objektu pouze pro vnitřní potřeby naší *directive*, pokud ale změníme název vlastnosti uvnitř objektu, např. `description` na `desc`, dostaneme se do úplně stejné šlamastiky jako předtím. Tento problém můžeme omezit tím, že pro každou proměnnou vytvoříme atribut. Jinými slovy bychom neposílali celý objekt `currentWeather` přes atribut `weatherData`, ale měli bychom jednotlivé atributy, např. `weatherDesc`, `weatherTemp`, atd., přes které bychom poslali jenom vybrané data a tím vytvořili naprosto izolovanou `directive`, u které bychom v případě venkovní změny názvů nemuseli vůbec nic měnit. Poté nastává otázka zda-li je to udržitelné při velkém objektu. Tady se už dostáváme ale do problematiky pevných/slabých vazeb při posílání parametrů do funkcí (tight/loose stamp/data coupling), kterou si už každý musí vyřešit sám. Hlavní výhodou tohoto druhu *scope* ale je, že nedědí žádné jiné proměnné nebo funkce z *rodičovské* *scope*, máme přístup pouze k těm, které přeneseme pomocí *scope* objektu {}. Jakákoliv následná úprava těchto proměnných se neprojeví v *mainControlleru*. Samozřejmě pokud se nebavíme o modifikaci vnitřních vlastností objektu jako tomu bylo u příkladu se `scope: true`, který jsem nechal zvídavému čtenáři jako kvíz.

V každém případě u tohoto příkladu nekončíme. Tento druh *scope* se jmenuje *Object scope* (protože posíláme objekt {}) a obsah toho, co posíláme přes objekt se ještě dělí na další tři typy, které vysvětluji následovně.

#### a. Objekt (=)
Na příkladě výše jsme si ukázali tento typ. V podstatě jsme vnitřní proměnnou naší *directive* definovali jako `weatherData: '='`. Znaménko `=` naznačuje, že cokoliv co pošleme přes atribut `weatherData` zůstane tím, čím je, pouze to bude uložené v proměnné s novým názvem, jejž definujeme. V našem případě je to například objekt `currentWeather`, se kterým můžeme dále manipulovat v naší *directive* (stejně jako s každým jiným JavaScriptovým objektem).

To mě vede k myšlence, že název *Objekt* není úplně přesný. Možná jste už slyšeli, že v JavaScripu je objekt úplně všechno, není to pravda (ale to je na další rozsáhlejší článek). Každopádně to znamená, že pokud do atributu definovaného jako typ `Objekt (=)` vložíte např. řetězec nebo číslo, dostanete tento *typ* proměnné. Jak řetězec, tak číslo není objektem (pouze může mít kolem sebe objektovou obálku, která jim dává další metody, ale v základu tyto typy nejsou objektem). Pojďme se podívat na upravený příklad, abych nestínil, co tím vlastně myslím.

<iframe width="100%" height="300" src="//jsfiddle.net/emilcieslar/fLqcxppk/embedded/js,html,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Na předchozím příkladu sčítám hodnoty `temperature` a `windSpeed`. Kdyby tyto dvě hodnoty byly brány jako objekt, tak by součet nebyl číslo, ale pouze spojení dvou objektů. V tomto případě by byly automaticky JavaScripem převedeny na řetězce a výsledek by nebyl `44` ale `2420`. Takže pozor na JavaScript, má **typy**, ne pouze __objekty__.

#### b. Text (@)
Druhý typ je *Text*, který jak už název napovídá pouze přenese hodnotu, která je vložená do atributu. Jinými slovy pokud vložíme do atributu třeba proměnnou, která obsahuje *String* `"Ahoj"` (nebo cokoliv), tak angular bude ignorovat, co je uvnitř proměnné, ale vrátí nám název proměnné. Takže tento typ se spíše hodí na přenášení staticky definovaných konfiguračních hodnot. Podívejme se na malou úpravu naší aplikace, která nám přidává konfiguraci naší *directive*

<iframe width="100%" height="300" src="//jsfiddle.net/emilcieslar/3s6b6g34/embedded/js,html,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Určitě jste si všimli `ng-show="displaySum != 0"`. Proč nenapíši pouze `ng-show="displaySum"`? `0` je přece automaticky *false*. Problém je totiž v tom, že tento typ předání proměnné do *scope* všechno převádí na řetězec, tudíž `0` je zde v podstatě `"0"`, což podmínka vyhodnotí jako *true*, protože to není prázdný řetězec.

Kdybych chtěl šťourat ještě hlouběji a trochu to dovysvětlit, tak bych to vysvětlil asi takto:

`"0" === true` tímto způsobem v podstatě vyhodnocuje `ng-show` hodnotu `displaySum`, což znamená, že výsledek bude `true`.

My chceme, ale aby byl výsledek `false`, protože `0` je přece `false`. Proto musíme vyhodnotit naši podmínku explicitně takto:
`("0" != 0) === true`, což v podstatě znamená `false === true`, takže se náš blok nezobrazí pokud hodnota `displaySum` bude rovna `0`. V případě, že hodnota bude ale rovná `1`, tak podmínka bude taková: `("1" != 0) === true`, což už vyjde jako `true`.

Jestli jsem vám právě zamotal hlavu ještě více, což je možné, tak se připomeňte v komentářích. Pokusil bych se to dovysvětlit srozumitelněji.


#### c. Funkce (&)
Posledním typem předání proměnné do *scope* jsou *funkce*. Jak už název napovídá, budeme předávat celé funkce. Pojďme se rovnou podívat na kód.

<iframe width="100%" height="300" src="//jsfiddle.net/emilcieslar/nuo7tz55/embedded/js,html,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

V příkladu je hned několik změn. Nejprve jsme definovali novou metodu `countSum()` v našem *controlleru*, kterou jsme následně předali přes atribut do naší *directive*. V *directive* jsme poté tuto metodu zavolali, abychom spočítali součet temploty a rychlosti větru. Všimněte si, že metoda počítá součet z proměnných, které jsou definované v *controlleru*, ale metodu zavoláme uvnitř *directive* (nebo přesněji v *template* od *directive*). Tato metoda totiž funguje jako *closure*, jinými slovy uzavírá tyto proměnné do sebe a tím pádem k nim stále má přístup i když se pohybuje úplně mimo *controller*. Tématika týkající se *closures* je ale složitější a opět by zasloužila samostatný článek.

Pozn.: Pozor při předávání metody do atributu – nezapomeňte ji zavolat při předání (např. `countSum()`). Kdybychom zapomněli na `()`, tak v *directive* bychom museli zavolat funkci takto: `countSum()()`.

## Závěr
V tomto článku jsme si definovali různé druhy *scope*, které lze nastavit na *directive*. Byly to `false`, `true` a `Objekt`. `false` dědilo úplně všechno z rodičů a mohli jsme manipulovat a měnit proměnné s efektem v celé aplikaci. `true` bylo o něco lepší v tom, že přidávání nových proměnných nebo funkcí do *scope* nemělo efekt nikde jinde než v *directive* samotné. Nakonec `Objekt` vytvořil izolovanou *scope* pro *directive*. Do `Objektu` jsme mohli pak přenášet proměnné opět třemi různými způsoby. Pomocí přenesení celého *objektu*, pouze *textu* a nakonec i *funkce*. Doufám, že tento článek pomohl objasnit začínajícím AngularJS vývojářům nejasnosti týkající se definování *directive* *scope*. Pokud máte jakékoliv další dotazy nebo připomínky k této problematice, nezapomeňte zanechat komentář a můžeme to prodiskutovat. Ať se daří při kódování.
