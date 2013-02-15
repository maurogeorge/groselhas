---
layout: post
title: Prefira composição ao invés de herança, um simples exemplo em Ruby
meta_description: Entenda o porquê de se usar composição ao invés de herança com um simples exemplo em Ruby
meta_keywords: Ruby, PORO, OOP, Herança, Composição, GoF
category: Ruby
tags: [Ruby, OOP]
---

Um dos princípios da orientação a objetos é:

> Prefira composição de objetos à herança de classes

como é descrito no [livro Design Patterns: Elements of Reusable Object-Oriented Software da GoF](http://en.wikipedia.org/wiki/Design_Patterns).
Vamos agora ver um exemplo prático em ruby de como podemos usar composição ao invés de herança.

## O problema

Nossas classes representam um carro e os componentes que podem ser adicionados ao carro, os componentes são alarme e ar-condicionado, e o que precisamos saber ao final é o preço do carro.

## Usando herança

Seguindo uma implementação baseada em herança teríamos algo assim:

{% highlight ruby %}
class Car

  def price
    1.00
  end
end

class CarWithAlarm < Car

  def price
    super + 0.20
  end
end


class CarWithAirConditioning < Car

  def price
    super + 0.30
  end
end

class CarWithAlarmAndAirConditioning < Car

  def price
    super + 0.50
  end
end
{% endhighlight %}

Nossas classes são bem simples e usadas assim:

{% highlight ruby %}
puts Car.new.price # => 1.0
puts CarWithAlarm.new.price # => 1.2
puts CarWithAirConditioning.new.price  # => 1.3
puts CarWithAlarmAndAirConditioning.new.price  # => 1.5
{% endhighlight %}

Os 2 primeiros casos (`CarWithAlarm` e `CarWithAirConditioning`) funcionam muito bem, já o terceiro caso `CarWithAlarmAndAirConditioning` temos os seguintes problemas:

* **repetição** - pois pegamos o valor adicionado de `CarWithAlarm` e `CarWithAirConditioning` e somamos para chegar ao preço de `CarWithAlarmAndAirConditioning`
* **dificil de manter** - pois se o valor de `CarWithAlarm` mudar teremos que lembrar de alterar em `CarWithAlarmAndAirConditioning`
* **dificil de estender** - imagine se tivermos mais um componente, Radio, teremos que criar:
  * `CarWithRadio`
  * `CarWithRadioAndAlarm`
  * `CarWithRadioAndAirConditioning`
  * `CarWithRadioAlarmAndAirConditioning`

deu para perceber que estamos fazendo errado né? Vamos agora ao exemplo usando composição.

## Usando composição

Primeiro criamos a nossa classe `Car`:

{% highlight ruby %}
class Car

  def initialize
    @price = 1.00
    @components = []
  end

  def add_component(component)
    @components << component
  end

  def price
    @components.map(&:price).inject(@price, :+)
  end
end
{% endhighlight %}

Vamos analisar a nossa classe.

### Initialize

Criamos 2 váriaveis de instância:

* `@price` que é o preço do nosso carro
* `@components` um array para armazenar os componentes de nosso carro

### add_component

Método que adiciona um componente ao nosso carro.

### price

Retorna o preço total do carro

Tendo nossa classe `Car` criada vamos as classes de componentes:

{% highlight ruby %}
class Alarm

  def price
    0.20
  end
end

class AirConditioning

  def price
    0.30
  end
end
{% endhighlight %}

Agora com nossas classes de componentes criadas, vamos a um exemplo de como podemos usar a nossa classe `Car`, agora utilizando de composição.

{% highlight ruby %}
car = Car.new
car.add_component(Alarm.new)
puts car.price # => 1.2
{% endhighlight %}

Como pode ver obtivemos o mesmo resultado que tinhamos com a classe `CarWithAlarm`, agora se precisarmos ter o mesmo resultado de `CarWithAlarmAndAirConditioning` fariamos:

{% highlight ruby %}
car = Car.new
car.add_component(Alarm.new)
car.add_component(AirConditioning.new)
puts car.price # => 1.5
{% endhighlight %}

como podemos ver com composição obtivemos um código bem mais fácil de se **manter**, **estender** e sem **repetições**.

Usámos o carro e seus componentes, mas poderia ser a modelagem de uma transação bancária que possui diversas taxas diferentes por exemplo.
