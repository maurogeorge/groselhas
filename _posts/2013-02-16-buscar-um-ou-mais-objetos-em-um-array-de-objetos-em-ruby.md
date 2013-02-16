---
layout: post
title: Buscar um ou mais objetos em um array de objetos em Ruby
meta_description: Saiba como buscar por objetos em um array de objetos em Ruby utilizando os métodos select e detect do Enumerable
meta_keywords: Ruby, Dica, select, detect, Enumerable, Array, Object
category: Ruby
tags: [Ruby, Dica]
---

Como fariamos para conseguir pegar um objeto de um `array` de objetos baseado em uma propriedade do objeto?

Vamos ao seguinte cenário, dado que tenhamos o seguinte `array` de `OpenStruct`s, utilizamos `OpenStruct` pela facilidade mas poderia ser qualquer objeto ou mesmo um `Hash`.

{% highlight ruby %}
require 'ostruct'

games = [
  OpenStruct.new(
    id: 53,
    name: "Gears of War",
    gender: "action"
  ),
  OpenStruct.new(
    id: 54,
    name: "Uncharted",
    gender: "action"
  )
]
{% endhighlight %}

Se quisessemos apenas o objeto com o id 53, poderiamos usar o [Enumerable#select](http://ruby-doc.org/core-1.9.3/Enumerable.html#method-i-select) que nos retornaria:

{% highlight ruby %}
puts games.select { |g| g.id == 53 } # => [#<OpenStruct id=53, name="Gears of War", gender="action">]
{% endhighlight %}

como pode ver o `select` nos retorna uma coleção, sendo assim ele retorna todos os objetos que satisfazerem a condição passada ao bloco, veja o exemplo a seguir:

{% highlight ruby %}
puts games.select { |g| g.gender == 'action' } # => [#<OpenStruct id=53, name="Gears of War", gender="action">, #<OpenStruct id=54, name="Uncharted", gender="action">]
{% endhighlight %}

Como temos 2 objetos com o gender igual a action ele retornou os 2. Caso queiramos apenas o primeiro objeto que satisfazer a condição, o que faz sentido no caso de uma busca por id por exemplo, podemos utilizar o [Enumerable#detect](http://ruby-doc.org/core-1.9.3/Enumerable.html#method-i-detect) veja:

{% highlight ruby %}
puts games.detect { |g| g.id == 53 } # => #<OpenStruct id=53, name="Gears of War", gender="action">
{% endhighlight %}

Então fica a dica precisando buscar em uma coleção, uma coleção de objetos que satisfaçam uma certa condição utilizar o `select`, precisando retornar um único valor que satisfaça a condição utilizar o `detect`.