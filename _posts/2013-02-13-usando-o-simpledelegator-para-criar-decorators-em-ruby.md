---
layout: post
title: Usando o SimpleDelegator para criar decorators em Ruby
meta_description: Saiba como utilizar o SimpleDelegator para criar seus decorators em Ruby
meta_keywords: Ruby, PORO, OOP, SimpleDelegator, decorator, rails, active record
category: Ruby
tags: [Ruby, Ruby on Rails, OOP]
---

Recentemente postei no [blog da HE:labs](http://helabs.com.br/blog) sobre [como extrair responsabilidade de models no rails utilizando-se de decorators](http://helabs.com.br/blog/2013/01/28/extraindo-a-responsabilidade-de-fat-models-com-o-uso-de-decorators/) para quem curtiu e já está utilizando decorators em suas apps apresento agora o [SimpleDelegator](http://www.ruby-doc.org/stdlib-1.9.3/libdoc/delegate/rdoc/SimpleDelegator.html) que nos ajuda a criar nossos decorators.

Imagine que tenhamos a seguinte classe:

{% highlight ruby %}
class Saiyajin
  attr_accessor :name

  def initialize(name)
    self.name  = name
  end
end
{% endhighlight %}

uma simples classe ruby, mas que poderia ser um de seus models active record por exemplo.

A nossa classe é utilizada assim:

{% highlight ruby %}
goku = Saiyajin.new("Goku")
puts goku.name # => "Goku"
{% endhighlight %}

Agora iremos criar o Super Sayajin, vamos a nossa classe:

{% highlight ruby %}
require 'delegate'

class SuperSaiyajin < SimpleDelegator

  def name
    "#{super} super saiyajin"
  end

  def power
    "It's Over 9000!"
  end
end
{% endhighlight %}

a nossa classe Super Sayajin poderia ser utilizada assim:

{% highlight ruby %}
goku = SuperSaiyajin.new(goku)
puts goku.name # => "Goku super saiyajin"
puts goku.power # => "It's Over 9000!"
{% endhighlight %}

Criando o nosso decorator herdando de `SimpleDelegator` não precisamos criar um `initialize` e agora o nosso decorator responde a todos os métodos da classe "decorada". Perceba que podemos utilizar o `super` para nos referenciarmos a métodos da classe "decorada", como usamos em `SuperSaiyajin#name`.

Fica a dica do `SimpleDelegator` para quando formos criar nossos decorators. Se você não conhecia o `SimpleDelegator` ou usa outra abordagem comenta aí.