---
layout: post
title: 'Moduly s Webpackem'
subtitle: 'Jak "sbalit" moduly pomocí Webpacku'
date: 2016-09-14 14:00
tags:
- Webpack
- JavaScript
- Moduly
---

V dnešní době se front-endová část webu stává více komplikovanější než tomu bylo kdysi. Píše se mnohem více JavaScriptu na straně klienta. Proto je zapotřebí mít hezkou strukturu v kódu, abychom se neztratili a náš kód mohl růst. Pro takové situace se ideálně hodí moduly. Kód si můžeme rozdělit do jednotlivých modulů a tak separovat jednotlivé funkce naší aplikace bez toho aniž by docházelo mezi jednotlivými části kódu ke konfliktům.

Webpack je, mimo jiné skvělé vlastnosti, skvělým nástrojem pro **sbalování** takových modulů dohromady. Dokáže sbalit jednak námi vytvořené moduly, tak moduly cizí, jako např. *babel-polyfill* nebo *angular*. Pojďme se rovnou vrhnout na příklad velmi jednoduché aplikace, která nebude v podstatě nic vypisovat v prohlížeči, pouze v konzoli. Nejprve si vytvoříme vlastní *modul*, který budeme chtít použít v naší aplikaci a ukážeme si, jak bychom pracovali s tímto modulem bez **Webpacku**. Poté se podíváme na to, jak nám *Webpack* ulehčí práci při používání našeho modulu.

## Nejprve bez Webpacku
Všechno to odstartujeme prázdným souborem `index.html`, který bude mít pouze script tagy.

{% highlight html %}
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
    </head>
    <body>

        <script src="helpers.js"></script>
        <script src="app.js"></script>

    </body>
</html>
{% endhighlight %}

V HTML souboru odkazujeme na dva JavaScriptové soubory, které si ukážeme následovně. První bude náš modul `helpers.js` ve starém ES5 JavaScriptu zapouzdřený v [IIFE](https://en.wikipedia.org/wiki/Immediately-invoked_function_expression).

{% highlight js %}
var helpers = (function () {

    return {

        preloadImg: function preloadImg(url, successCallback, errorCallback) {

            var imgToBePreloaded = new Image();

            imgToBePreloaded.addEventListener('load', function onImgLoad() {
                successCallback.call(this);
            });

            imgToBePreloaded.addEventListener('error', function onImgError() {
                errorCallback();
            })

            imgToBePreloaded.src = url;

        },

        anotherHelper: function anotherHelper() {

            return 'Helping...';

        }

    }

})();
{% endhighlight %}

A následuje náš `app.js` soubor, který bude modul *helpers* používat.

{% highlight js %}

// Načte obrázek
helpers.preloadImg('image.jpg', function () {

    // Obrázek se načetl...
    console.log('preloaded...', this);

});

{% endhighlight %}

Jak můžete vidět, je naprosto jednoduchý, pouze volá metodu, kterou nabízí náš module *helpers*. Dosud jsme mohli vidět už minimálně dva negativa tohoto postupu:

1. Modul *helpers* tzv. znečišťuje globální prostředí tím, že ho přiřazujeme proměnné `var helpers = ...`. Může se tedy lehce stát, že někde uvnitř našeho kódu v `app.js` nebo kdekoliv jinde tuto proměnnou přepíšeme něčím jiným a celá aplikace se zboří.

2. Kdybychom měli 20 takových modulů, tak každý bychom museli přidávat jako `<script>` tag do HTML souboru. Docela nepříjemná věc na udržitelnost rozsáhlejších projektů.

## Pojďme na Webpack
V následující části si přepíšeme výše uvedený kód tak, aby se nám sbalil pomocí *Webpacku* a ještě přidáme oblíbený *Babel*, který nám umožní používat ES6 syntaxi aniž by byla ještě podporována ve všech prohlížečích.

Abychom se v tom nezamotali, nastíním adresářovou strukturu, kterou v následujícím příkladu použijeme.

{% highlight javascript %}

├── dist/
├── node_modules/
├── app.js
├── helpers.js
├── image.jpg (neni povinne, muze byt jakykoliv obrazek)
├── index.html
├── package.json
├── webpack.config.js

{% endhighlight %}

Nejprve si přepíšeme `index.html`, v podstatě pouze odstraníme *script* s našim helpers modulem a změníme název `app.js` na `dist/app.bundle.js`. Změna názvu na *.bundle* není nutná, ale je to v podstatě takový balíček, takže název více napovídá, co je uvnitř.

{% highlight html %}
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
    </head>
    <body>

        <script src="dist/app.bundle.js"></script>

    </body>
</html>
{% endhighlight %}

Následovně přepíšeme *helpers* modul ve stylu ES6. V příštím článku ještě ukážu jak přepsat funkci pro přednačítání obrázků do stylu ES6 pomocí tzv. *Promises*. Nechtěl jsem to sem míchat, protože by to mohlo mást.

{% highlight js %}
function preloadImg(url, successCallback, errorCallback) {

    var imgToBePreloaded = new Image();

    imgToBePreloaded.addEventListener('load', function onImgLoad() {
        successCallback.call(this);
    });

    imgToBePreloaded.addEventListener('error', function onImgError() {
        errorCallback();
    })

    imgToBePreloaded.src = url;

}

function anotherHelper() {

    return 'Helping...';

}

export { preloadImg, anotherHelper };
{% endhighlight %}

V podstatě jsme vytáhli funkce z *objektu*, který původně IIFE vracela a o zapouzdření se nyní stará `export { preloadImg, ... };` Tímto způsobem pouze zveřejňujeme metody, které chceme. Pokud se chcete více dozvědět o tom, jak se dají exportovat nebo importovat moduly, mrkněte na [tento článek](http://www.2ality.com/2014/09/es6-modules-final.html). Není sice česky, ale tak to bohužel bývá s většinou článků o této tématice. Pokud byste měli zájem, mohl bych se této tématice věnovat více, stačí napsta komentář.

Další následuje úprava `app.js`, do kterého importujeme pomocné metody pomocí `import` ES6 syntaxe.

{% highlight js %}

import * as helpers from './helpers';

// Načte obrázek
helpers.preloadImg('image.jpg', function () {

    // Obrázek se načetl...
    console.log('preloaded...', this);

});

// Vypíše "Helping..."
console.log( helpers.anotherHelper() );

{% endhighlight %}

Jako poslední věc se postaráme o samotný webpack. Nejprve nainstalujeme potřebné závislosti pomocí `npm`.

{% highlight bash %}

npm install webpack -g # Nainstaluje webpack globálně
npm init # Vytvoříme prázdný package.json, který bude obsahovat naše závislosti
npm install webpack --save-dev # Nainstalujeme webpack do projektu
npm install babel-loader babel-core babel-preset-es2015 --save-dev # Nainstalujeme závislosti, které potřebujeme pro babel

{% endhighlight %}

Poté vytvoříme soubor `webpack.config.js`, kterým nakonfigurujeme náš sbalovací proces. Tento soubor bude obsahovat následující.

{% highlight js %}

var path = require('path');
var webpack = require('webpack');

module.exports = {

    context: __dirname, // Absolutni cesta, ktera se pouzije pro vychozi bod, tzv. `entry`

    entry: './app.js', // Vychozi bod aplikace

    output: {
        path: path.join(__dirname, 'dist'), // Do jake slozky pujde nase sbalena aplikace
        filename: 'app.bundle.js' // Jak se bude nase sbalena aplikace jmenovat
    },

    module: {
        loaders: [
            {
                test: /\.js$/, // Transpilujeme vsechny soubory s priponou .js
                exclude: [/node_modules/], // Vynechame slozku /node_modules
                loader: 'babel-loader', // Pouzijeme k transpilingu babel
                query: {
                    presets: ['es2015'] // Prednastaveni babelu bude es2015, jinymi slovy ES6
                }
            }
        ]
    }

}

{% endhighlight %}

A jako bonus, spustíme webpack, aby to všechno dohromady sbalil.

{% highlight bash %}

webpack -w # Spustíme webpack a necháme ho naslouchat změnám v souborech

{% endhighlight %}


V následujícím článku si ještě ukážeme, jak oddělit velké (neměnící se) moduly od těch našich malých. Např. takový angular je celkem rozsáhlý modul, který by mohl velmi zpomalit balící proces webpacku a proto je lepší ho oddělit do separátního balíčku. Teď již ale víte, jak sbalit jednoduché moduly a tím si zpřehlednit celou aplikaci.



## Reference
1. [https://github.com/babel/babel-loader](https://github.com/babel/babel-loader)
2. [http://webpack.github.io/docs/](http://webpack.github.io/docs/)
