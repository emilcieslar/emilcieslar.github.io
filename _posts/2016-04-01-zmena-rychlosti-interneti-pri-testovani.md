---
layout: post
title:  "Změna rychlosti internetu při testování"
subtitle: "Chrome Developer Tools - Jak změnit rychlost internetu pro testování webu aneb Network Throttling"
date:   2016-04-01 16:57:51
tags:
- Chrome Dev Tools
---

Když programujete a testujete web, většinu času jste na localhostu, kde je vše příliž rychlé. Vše se načte rychlostí blesku a proto někdy nepostřehnete nedokonalosti, ke kterým může dojít pouze když je rychlost stahování dat pomalá. Tak například načítáte na úvodní stránku nějaké příspěvky pomocí ajaxu z externí API a na localhostu (nebo wifi) jsou příspěvky už načtené okamžitě při načtení DOMu. Kdežto běžný uživatel si prohlíží váš web třeba když zrovna jede RegioJetem z Prahy do Ostravy a úplnou náhodou je v úseku někde mezi Českou Třebovou a Olomoucem, kde je nejvíce tunelů a neuvěřitelně pomalý internet. Na mobilu vidí pouze bílou stránku a žádné příspěvky a čeká a čeká – až ho přejde trpělivost a web zavře.

Nenapadlo vás totiž, že když se příspěvky načítají, na webu je polovina homepage bílá a proto by bylo fajn tam dát nějaký hezký načítač a oznámení, že se příspěvky načítají a proto by mohl uživatel chvíli počkat. Proto jsou tady skvělé Chrome Developer Tools, kde můžete nastavit simulaci pomalejšího internetu, viz. obrázek níže.

<img src="" alt="Google throttle" />

Takto jednoduchým způsobem otestujete váš web při pomalém připojení a doladíte chybičky, kterých byste si jinak nevšimli.

## Reference
https://developers.google.com/web/tools/chrome-devtools/profile/network-performance/network-conditions?hl=en

