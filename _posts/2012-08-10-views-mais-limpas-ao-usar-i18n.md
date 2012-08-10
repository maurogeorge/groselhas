---
layout: post
title: Views mais limpas ao usar I18n
meta_description: Dicas para ter suas views mais limpas no uso do I18n do ruby on rails
meta_keywords: ruby on rails, I18n, .html_safe, views, ruby
category: Ruby on Rails
tags: [Ruby on Rails, I18n, Views, Ruby]
---

## Melhorando as views com o I18n

Quando estamos trabalhando com I18n no rails as vezes podemos nos deparar com algo como o código a seguir em nossas views:

{% highlight ruby %}
t 'posts.index.title'
{% endhighlight %}

veja que temos que passar por várias sessões do yml para chegarmos ao título que queremos exibir.

Agora imagine que tenhamos em nossa pasta de views a seguinte estrutura:

    posts
      - index.html.haml

e em nosso yml

    pt-BR:
      posts:
        index:
          title: Meu título

podemos usar em nossa view (**views/posts/index.html.haml**) apenas:

{% highlight ruby %}
t '.title'
{% endhighlight %}

isso funciona desde que mantenhamos a convenção no yml, de manter as sessões do yml a mesma da estrutura de pastas das views.

## Removendo o .html\_safe das views

Caso algum dos seus arquivos de tradução possua alguma tag html, você já deve ter encontrado algo assim nas views:

{% highlight ruby %}
t('.title').html_safe
{% endhighlight %}

este último para dar escape das tags html.

No entanto isto poderia ser feito no yml como a seguir.

    pt-BR:
      posts:
        index:
          title_html: Meu <strong>título</strong>


definindo o sufixo `_html` no yml você não precisa mais usar o `html_safe` podendo fazer apenas.

{% highlight ruby %}
t '.title'
{% endhighlight %}