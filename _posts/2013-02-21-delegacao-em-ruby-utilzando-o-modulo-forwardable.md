---
layout: post
title: Delegação em Ruby utilzando o módulo Forwardable
meta_description: Saiba como utilizar o módulo Forwardable para usar delegação de uma forma mais inteligente em Ruby
meta_keywords: Ruby, Delegação, Enumerable, Forwardable, delegator
category: Ruby
tags: [Ruby, OOP]
---

Em Ruby temos o poder de fazer overloading de operadores, ou seja definirmos nossa própria implementação para os operadores.

Quando temos uma classe que é uma coleção é uma boa idéia, se fizer sentido, definirmos os operadores `<<`, `[]` e `[]=` pois assim teremos uma interface igual a de um [Enumerable](http://ruby-doc.org/core-1.9.3/Enumerable.html).
Tecnicamente os operadores `[]` e `[]=` não são operadores e sim métodos, em que o Ruby aplica um [açucar sintático](http://en.wikipedia.org/wiki/Syntactic_sugar) assim quando fazemos `foo[2]` estamos chamando o método `[]` em `foo`, passando o parâmetro 2 e quando fazemos `foo[2] = 4` estamos na realidade chamando o método `[]=` em `foo` passando os parâmetros 2 e 4, por isso conseguimos fazer overloading destes métodos também.

Vamos a um exemplo:

{% highlight ruby %}
class Basket

  def initialize
    @itens = []
  end

  def total_of_itens
    @itens.size
  end

  def <<(item)
    @itens << item
  end

  def [](index)
    @itens[index]
  end

  def []=(index, item)
    @itens[index] = item
  end
end
{% endhighlight %}

criamos uma classe `Basket` que é nossa cesta, seu papel é ser uma coleção de itens, vamos vê-la em ação:

{% highlight ruby %}
basket = Basket.new
basket << "Morango"
basket << "Manga"
basket << "Banana"
basket[3] = "Pera"
p basket[0] # => "Morango"
p basket[3] # => "Pera"
p basket.total_of_itens # => 4
{% endhighlight %}

Como pode ver agora nossa classe se comporta como um `Enumerable` no entanto se observar bem os métodos que implementamos, nada mais são do que delegar para os métodos do `Enumerable`, vamos a uma implementação mais inteligente.

## Utilizando o Forwardable

O Ruby possui o módulo [Forwardable](http://ruby-doc.org/stdlib-1.9.2/libdoc/forwardable/rdoc/Forwardable.html) que nos permite delegar métodos específicos para um objeto.
Ou seja poderemos delegar os métodos de array, para o nosso próprio array `@itens`. Vamos ao código:

{% highlight ruby %}
require 'forwardable'

class Basket
  extend Forwardable

  def initialize
    @itens = []
  end

  def_delegator :@itens, :size, :total_of_itens
  def_delegators :@itens, :<<, :[], :[]=
end
{% endhighlight %}

Ao utilizarmos a nossa nova classe `Basket` obtemos o mesmo resultado da classe anterior:

{% highlight ruby %}
basket = Basket.new
basket << "Morango"
basket << "Manga"
basket << "Banana"
basket[3] = "Pera"
p basket[0] # => "Morango"
p basket[3] # => "Pera"
p basket.total_of_itens # => 4
{% endhighlight %}

Vamos agora entender o que aconteceu.

### def_delegator

Primeiro utilizamos o [`def_delegator`](http://ruby-doc.org/stdlib-1.9.2/libdoc/forwardable/rdoc/Forwardable.html#method-i-def_delegator) que nos permite definir um delegator, para um único método do objeto e opcionalmente definir um nome diferente ao método. No nosso exemplo usamos o `Basket#total_of_itens` pois para a nossa interface faz mais sentido este nome do que apenas `Basket#size`.

### def_delegators

Como você já deve imaginar o [`def_delegators`](http://ruby-doc.org/stdlib-1.9.2/libdoc/forwardable/rdoc/Forwardable.html#method-i-def_delegators) nos permite definir diversos delegators, para diversos métodos do objeto, no entanto não é possível definir um nome diferente ao método. Utilizamos ele para criar os demais métodos `<<`, `[]` e `[]=`.

Como pode ver o uso do `Forwardable` fez nossa classe mais **enxuta** e não precisamos redefinir os métodos já existentes em nosso objeto. Lembrando que pode-se definir qualquer método de um dado objeto, cabe a você fazer as escolhas certas para manter uma interface clara.