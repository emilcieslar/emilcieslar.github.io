---
layout: post
title:  "PHP Multiupload"
subtitle: "Nahrávání více souborů/obrázků najednou pomocí jednoho formuláře"
date:   2015-11-02 16:57:51
tags:
- PHP
---

## Nahrávání několika obrázků najednou pomocí jednoho formuláře
Často se stává, že chcete nahrát několik obrázků najednou pomocí jednoho inputu. Nahrávat více obrázků najednou lze několika způsoby.

1. Mít několik inputů, každý pro jeden obrázek,
2. použít staženou třídu nebo plugin, který má většinou složitou dokumentaci a stovky nastavení, protože se snaží řešit dalších tisíc věcí navíc,
3. zkusit moji jednoduchou PHP třídu :)
4. Určitě tady budou další cesty, ale to by pro ilustraci určitě stačilo.

Dnes vám ukáži, jak lze jednoduše použít moji PHP třídu k tomu, abyste si již nikdy nemuseli trhat vlasy když nahráváte více souborů/obrázků pomocí jednoho formuláře. Nejprve si vytvoříme naprosto jednoduchý formulář.

{% highlight html %}
<form action="test.php" method="post" enctype="multipart/form-data">
  <fieldset>
    <legend>PHP File Multiupload</legend>
    <input type="file" name="files[]" multiple>
    <input type="submit" name="uploadFiles" value="Upload files">
  </fieldset>
</form>
{% endhighlight %}

Zde je důležité si povšimnout, že input pro nahrávání souborů má na konci atribut `enctype`, který obsahuje `multipart/form-data` a v podstatě tím formuláři říkáme, že budeme posílat něco většího (soubory). Dále také jeho atribut `name` má hodnotu `files[]`, čímž dáváme najevo, že odeslaná data budou pole s názvem `files`. Formulář směřuje do souboru `test.php`, který má (nebo může mít, záleží na vás) následující kód.

{% highlight php %}
<?php

require_once('Upload.php');
// Initializujeme si třídu Upload a do jejího konstruktoru předáme pole files
$upload = new Upload($_FILES['files']);
// Nastavíme si adresář, do kterého budeme nahrávat soubory
$upload->setDir('github/PHP-File-Multiupload/upload');
// Nahrajeme soubory
$upload->upload();
{% endhighlight %}

No není to naprosto jednoduché? A to je vše. V případě dotazů se podívejte do dokumentace (je velmi krátká) nebo zanechte komentář. Ať se daří při kódování.

## Upload.php třída
Abych nezapomněl, tak zde je odkaz ke stažení Upload.php třídy.

[Upload.php na githubu](https://github.com/emilcieslar/PHP-File-Multiupload/blob/master/Upload.php)
