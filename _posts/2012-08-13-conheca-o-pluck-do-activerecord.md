---
layout: post
title: Conheça o pluck do ActiveRecord
meta_description: Conheça o pluck e como ele pode ajudar nos seus objetos ActiveRecord
meta_keywords: ruby on rails, active record, ruby, pluck, map
category: Ruby on Rails
tags: [Ruby on Rails, ActiveRecord, Ruby]
---

No rails, quando precisamos selecionar apenas o valor de uma coluna especifica de uma coleção de um objeto ActiveRecord podemos fazer algo como:

{% highlight ruby %}
Post.map(&:name)
{% endhighlight %}

e obtemos a resposta algo como:

{% highlight ruby %}
[
    [0] "Foo",
    [1] "Bar",
]
{% endhighlight %}

no entanto a partir do [rails 3.2](http://guides.rubyonrails.org/3_2_release_notes.html) podemos usar o método [pluck](http://api.rubyonrails.org/classes/ActiveRecord/Calculations.html#method-i-pluck) do ActiveRecord. Ficando o nosso exemplo anterior com o pluck assim:

{% highlight ruby %}
Post.pluck(:name)
{% endhighlight %}

Aparentemente não tem diferenças, afinal ambos ocupam apenas uma linha. No entanto ao utilizarmos o primeiro método estamos instanciando N objetos ActiveRecord e com o map obtendo o valor de cada um deles e retornando um array. Já no exemplo que usa o `pluck` não precisamos instanciar N objetos ActiveRecord, obtemos o array diretamente.

Por isso prefira sempre usar o `pluck`, afinal é nativo do ActiveRecord e ainda tem uma performance melhor do que a solução com o `map`.