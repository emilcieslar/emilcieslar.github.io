---
layout: post
title:  "Instalace Forever.js"
subtitle: "Jak nainstalovat Forever.js, který zanechá běžet NodeJS aplikace v pozadí"
date:   2015-10-14 16:57:51
tags:
- ExpressJS
- NodeJS
- ForeverJS
---

Pokud jste si přečetli článek [Instalace NodeJS na vlastní VPS]({% post_url 2010-07-21-name-of-post %}) určitě už víte, že abychom mohli mít několik aplikací spuštěných najednou, potřebujeme forever.js. Bez forever.js bychom museli každou aplikaci spustit pomocí příkazu node jmenoaplikace.js a pro každou mít zvlášť okno terminálu. Pokud by vám tato nepohodlnost nevadila, tak je tady další. Když aplikace spadne v důsledku nějaké chyby, příkaz node ji nerestartuje. Pojďme se tedy podívat jak nainstalovat forever.js, aby aplikace opravdu běžely neustále.

Vždy když používáte `{% raw %}{{#each promenna}}...{{/each}}{% endraw %}` nesmíte zapomenout na to, že uvnitř tohoto bloku se dostáváte do jiné **úrovně** proměnné. V praxi to znamená, že když například projíždím pomocí *each* pole `users`, uvnitř *each* už jsem uprostřed pole `users` a adresuji už nižší úroveň. Kdybych chtěl adresovat proměnnou ve vyšší úrovni, musím se na tu vyšší úroveň nejdříve dostat za použití `../`. Pojďme se podívat na příklad.

Tak například máme pole uživatelů `users`, které má více úrovní a vypadá v json následovně. Jako bonus k tomu máme ještě jednu proměnnou s dnešním datem `date`, která je o úroveň **výš** než např. `user` vlastnost uvnitř `user` objektu.

{% highlight js %}
users = [
	user: {
		name: 'Emil Cieslar',
		age: 26
	},
	user: {
		name: 'Karel Veselý',
		age: 30
	}
]

date = '15/10/2015'
{% endhighlight %}

Takové pole budeme iterovat pomocí handlebars *each*.

{% highlight handlebars %}
{% raw %}{{#each users}}
  Jméno: {{user.name}}
{{/each}}{% endraw %}
{% endhighlight %}

Pokud bychom chtěli uvnitř *each* vypsat ještě jinou proměnnou, která je o úroveň výše vůči této proměnné, museli bychom použít `../` jako v následujícím příkladu.

{% highlight handlebars %}
{% raw %}{{#each users}}
  Jméno: {{user.name}}

  Dnešní datum: {{../date}}
{{/each}}{% endraw %}
{% endhighlight %}

Tato maličkost je lehce přehlédnutelná a potom budete kroutit hlavou proč je ta proměnná `date` vlastně prázdná, když ji máte definovanou. Ať se daří při kódování.
