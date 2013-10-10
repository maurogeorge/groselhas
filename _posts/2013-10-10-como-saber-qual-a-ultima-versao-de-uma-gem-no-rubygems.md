---
layout: post
title: Como saber qual a última versão de uma gem no Rubygems
meta_description: Dica de como saber qual a última versão de uma gem no Rubygems
meta_keywords: ruby, rubygems, gem, list, remote
category: Dica
tags: [Ruby, Dica, Gem]
---

Quando queremos instalar uma nova gem é comum procurarmos em qual versão a Gem se encontra atualmente. Podemos obter este valor das seguintes maneiras:

- acessando o Readme e procuramos a versão, isso se o autor se importou em colocar este valor lá;
- Rubygems, [exemplo](https://rubygems.org/gems/jacaranda);

nenhum dos 2 modos é muito prático, para ambos você deve sair do terminal para ir atrás destes valores. O que eu uso e recomendo é o seguinte comando do Rubygems:

{% highlight bash %}
$ gem list jacaranda -r
{% endhighlight %}

ele retorna uma lista com a gem e sua ultima versão estável.

Claro que isso não quer dizer que não tenhamos sempre que ler o Readme, e Changelog das gems para sabermos como funciona, instalamos etc. Mas ajuda em uma gem que costumamos usar, e precisamos saber apenas em qual versão ela está atualmente.

