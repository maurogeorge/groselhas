---
layout: post
title: Dúvidas comuns do RVM
meta_description: Algumas dúvidas no uso do RVM respondidas
meta_keywords: ruby, rvm, current, gemset
category: Dica
tags: [Ruby, Dica, RVM]
---

Dúvidas que algum dia você já teve, ou terá enquanto utiliza o [RVM](https://rvm.io/), respondidas aqui.

## Qual o ruby e o gemset que estou atualmente?

Utilizamos o current quando queremos saber qual o ruby e versão estamos utilizando atualmente.

{% highlight bash %}
$ rvm current
{% endhighlight %}

## Como defino um gemset como default?

Para definirmos um gemset como default usamos o seguinte comando:

{% highlight bash %}
$ rvm  --default use 2.0.0@global
{% endhighlight %}

é uma boa prática definir o `@global` como gemset padrão.

## Como faço para criar e usar um gemset?

Parar criar e já sair usando um gemset utilize o seguinte comando:

{% highlight bash %}
$ rvm use 2.0.0@rails --create
{% endhighlight %}

este eu utilizo muito para começar a trabalhar em projetos open source =)

## Como faço para utilizar um gemset?

Parar usar um gemset utilize o seguinte comando:

{% highlight bash %}
$ rvm use 2.0.0@rails
{% endhighlight %}

também muito utilizado para trabalhar com open source, dado que muito dos projetos não colocam em seus repos o `.ruby-version` e `.ruby-gemset`.


## Como faço para ver os gemsets criados?

Para listar os gemsets utilizamos o seguinte comando:

{% highlight bash %}
$ rvm gemset list
{% endhighlight %}

## Como faço para remover todas as gems de um gemset?

Para remover todas as gems de um gemset rode:

{% highlight bash %}
$ rvm gemset empty my_gemset
{% endhighlight %}

