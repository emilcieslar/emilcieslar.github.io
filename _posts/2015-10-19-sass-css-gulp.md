---
layout: post
title:  "SCSS + Gulp"
subtitle: "Jak jednoduše komplikovat SASS pomocí Gulp"
date:   2015-10-19 16:57:51
tags:
- SCSS
- Gulp
---

SASS CSS preprocessor je asi nejpopulárnějším ve své kategorii. Kompilovat SCSS soubory po každé úpravě by bylo ale trochu namáhavé. Proto následující manuál jednoduše vysvětlí jakým způsobem lze automaticky kompilovat SCSS soubory pomocí Gulpu.

## Nainstalujeme si Gulp
Nejdříve budeme potřebovat Gulp nainstalovaný ve složce, kde máme náš projekt. Pokud ještě nemáte soubor package.json, ideálně si ho ještě před tímto krokem vytvořte pomocí `npm init`. Poté si nainstalujeme Gulp pomocí následujících příkazů.

{% highlight shell %}
npm install --global gulp
npm install --save-dev gulp
npm install gulp-sass --save-dev
{% endhighlight %}

Nejprve jsme nainstalovali globálně gulp a poté jsme ho přidali do našeho projektu. Nakonec jsme nainstalovali specifický plugin pro gulp a to sass plugin.

## gulpfile.js
Vytvoříme soubor s názve gulpfile.js a vložíme do něj následující kód.

{% highlight js %}
var gulp = require('gulp');
var sass = require('gulp-sass');

gulp.task('sass', function () {
  gulp.src('./sass/**/*.scss')
    .pipe(sass().on('error', sass.logError))
    .pipe(gulp.dest('./css'));
});

gulp.task('sass:watch', function () {
  gulp.watch('./sass/**/*.scss', ['sass']);
});
{% endhighlight %}

Výše uvedený kód v podstatě zajistí, že soubory s příponou `.scss` ve složce `./sass`, která se nachází v našem projektu se kompilují do složky `./css`, která se také nachází v adresáři našeho projektu. Příkaz `gulp.watch` zajistí, že po spuštění `gulp sass` v okně našeho terminalu bude `gulp` automaticky sledovat složku `scss` a při jakékoliv změně a uložení se soubory automaticky opět v mžiku kompilují aniž bychom museli hnout prstem. Níže nastiňuji základní adresářovou strukturu projektu, který by obsahoval jak css tak scss soubory.

{% highlight php %}
├── index.html
├── css/
│   ├── style.css
├── scss/
│   ├── style.scss
├── package.json
├── gulpfile.js
{% endhighlight %}

A to je vše co potřebujete ke kompilaci SCSS souborů do CSS pomocí Gulpu. Pokud máte jakékoliv dotazy, zanechte mi vzkaz v komentářích. Ať se daří při kódování.

## Reference
1. [https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md)
2. [https://www.npmjs.com/package/gulp-sass](https://www.npmjs.com/package/gulp-sass)
