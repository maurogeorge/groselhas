---
layout: post
title: Somando valores de um array em ruby de uma forma mais elegante utilizando inject
meta_description: Entenda como utilizar o inject para somar valores de um array em ruby sem a necessidade de iterar sobre todos os valores
meta_keywords: Ruby, Dica, Inject, Enumerable, Array
category: Ruby
tags: [Ruby, Dica]
---

Imagine que tenhamos que somar os valores de um `array`, provavelmente seguiríamos para uma abordagem neste sentido:

{% highlight ruby %}
values = [1, 2, 3, 5, 8]

total = 0
values.each do |value|
  total += value
end

puts total # => 19
{% endhighlight %}

No entanto o ruby possui o [Enumerable#inject](http://ruby-doc.org/core-1.9.3/Enumerable.html#method-i-inject). Poderiamos trocar o nosso exemplo para algo como:

{% highlight ruby %}
values = [1, 2, 3, 5, 8]

puts values.inject(:+) # => 19
{% endhighlight %}

uma versão bem mais simples e elegante do que o primeiro exemplo. O `inject` ainda nos dá a possibilidade de definirmos um valor para iniciarmos o somatório.

{% highlight ruby %}
values = [1, 2, 3, 5, 8]

puts values.inject(10, :+) # => 29
{% endhighlight %}

Perceba que agora iniciamos a contagem em 10 e adicionamos os demais valores.

Por padrão o valor que é iniciado no `inject` é `nil`, sendo assim se passarmos um array vázio o nosso retorno será `nil` e não 0, que pode ser o esperado:

{% highlight ruby %}
values = []

puts values.inject(:+) # => nil
{% endhighlight %}

Como vimos que podemos definir o valor inicial, podemos definir como 0 e assim conseguir o resultado que esperamos.

{% highlight ruby %}
values = []

puts values.inject(0, :+) # => 0
{% endhighlight %}

Para quem estiver utilizando o rails, o `ActiveSupport` adiciona no `Enumerable` o método [`sum`](http://api.rubyonrails.org/classes/Enumerable.html#method-i-sum) que pode ser utilizado assim:

{% highlight ruby %}
values = [1, 2, 3, 5, 8]

puts values.sum # => 19
{% endhighlight %}

No nosso exemplo utilizamos o `array` values criado manualmente, mas poderia muito bem ser um `array` criado a partir do uso do [pluck do ActiveRecord](/conheca-o-pluck-do-activerecord.html) por exemplo.
