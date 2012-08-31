---
layout: post
title: JavaScript - string multilinha
meta_description: Saiba como usar strings multilinhas no JavaScript
meta_keywords: JavaScript, Java Script, strings, multilinha, multi-linha
category: JavaScript
tags: [JavaScript]
---

Ao tentarmos escrever uma string multilinha em JavaScript do seguinte modo:

{% highlight javascript %}
string = "MY
awesome
string";
{% endhighlight %}

recebemos o seguinte erro:

    SyntaxError: unterminated string literal



Para "resolver" podemos concatenar a string fazendo assim:

{% highlight javascript %}
string = "My "
       + "awesome "
       + "string";
{% endhighlight %}

que até funciona, mais não seria a melhor abordagem.

No entanto podemos usar a seguinte sintaxe, para enfim escrevermos strings multilinhas no JavaScript:

{% highlight javascript %}
string = "MY \
awesome \
string";
{% endhighlight %}

Está última abordagem funciona tanto com aspas simples como duplas.