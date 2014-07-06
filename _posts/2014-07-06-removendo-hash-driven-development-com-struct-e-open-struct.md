---
layout: post
title: Removendo hash-driven development com Struct e OpenStruct
meta_description: Remova o hash-driven development utilizando de Struct e OpenStruct
meta_keywords: Ruby, hash, Struct, OpenStruct
category: Ruby
tags: [Ruby, OOP]
---

Temos que criar uma classe que exiba as informações de um Pokémon baseado no seu [id nacional](http://bulbapedia.bulbagarden.net/wiki/List_of_Pok%C3%A9mon_by_National_Pok%C3%A9dex_number) consumindo estes valores de uma API. Para isso simplesmente acessamos a API e parseamos a resposta do JSON a retornando por completo.

{% highlight ruby %}
class FetchPokemon

  def initialize(national_id)
    @national_id = national_id
    build_info
  end

  def call
    info
  end

  private

    attr_reader :national_id, :info

    def endpoint
      URI("http://pokeapi.co/api/v1/pokemon/#{national_id}/")
    end

    def build_info
      response = Net::HTTP.get(endpoint)
      @info = JSON.parse(response)
    end
end
{% endhighlight %}

Este código atende a nossa necessidade mais podemos melhorar o seu retorno, pois hoje precisamos apenas do nome, altura e peso. Então vamos alterar o nosso método `call` para retornar apenas estes atributos do `info`.

{% highlight ruby %}
class FetchPokemon
  # ...

  def call
    { name: info['name'], height: info['height'], weight: info['weight'] }
  end

  # ...
end
{% endhighlight %}

Agora temos apenas as informações que necessitamos. No entanto seria melhor se tivessemos um objeto `Pokemon` e não um hash, com um objeto sabemos exatamente com que estamos lidando, qual a entidade. E assim evitamos o *hash-driven development* e utilizamos uma abordagem orientada a objetos.


Para isso criamos uma [*inner class*](http://en.wikipedia.org/wiki/Inner_class) `Pokemon` que define exatamente os atributos que precisamos e os expõe através de um `attr_accessor`.

{% highlight ruby %}
class FetchPokemon
  # ...

  def call
    Pokemon.new(info['name'], info['height'], info['weight'])
  end

  private
    # ...

    class Pokemon

      attr_accessor :name, :height, :weight

      def initialize(name, height, weight)
        @name, @height, @weight = name, height, weight
      end
    end
end
{% endhighlight %}

Não se esqueça que uma [inner class possui o comportamento diferente de quando estamos utilizando módulos como *namespace*](http://blog.elpassion.com/ruby-gotchas/). No nosso caso não é um problema dado que estamos utilizando a classe `Pokemon` apenas como um container, não nos importamos com nada declarado na classe `FetchPokemon`.

Agora nossa classe `FetchPokemon` retorna um `FetchPokemon::Pokemon` no método `call`, bem legal. No entanto estamos repetindo 3 vezes cada atributo na declaração da classe.

Para remover esta repetição podemos criar um [`Struct`](http://www.ruby-doc.org/core-2.1.2/Struct.html) que faz exatamente o que precisamos: cria *accessors* para cada um dos atributos passados no `initialize`.

{% highlight ruby %}
class FetchPokemon
  # ...

  def call
    Pokemon.new(info['name'], info['height'], info['weight'])
  end

  private
    # ...

    Pokemon = Struct.new(:name, :height, :weight)
end
{% endhighlight %}

Continuamos com o `FetchPokemon::Pokemon` como classe no entanto removemos a repetição ao se declarar a classe `Pokemon`.

## OpenStruct

Para casos em que desejamos transformar um hash completo em um objeto temos o [OpenStruct](http://www.ruby-doc.org/stdlib-2.1.2/libdoc/ostruct/rdoc/OpenStruct.html). Voltando lá no inicio quando tínhamos um hash reduzido com apenas as informações que precisamos podemos criar uma classe `Pokemon` a partir dele utilizando do `OpenStruct`.

Para isso criamos uma classe `Pokemon` que herda de `OpenStruct`.

{% highlight ruby %}
class FetchPokemon
  # ...

  def call
    Pokemon.new(name: info['name'], height: info['height'], weight: info['weight'])
  end

  private
    # ...

    class Pokemon < OpenStruct
    end
end
{% endhighlight %}

Seja utilizando o `Struct` ou o `OpenStruct` teremos um objeto `FetchPokemon::Pokemon` a diferença é que um será herdado de `Struct` e outro de `OpenStruct`.

{% highlight ruby %}
fetch_pokemon = FetchPokemon.new(6)
pokemon = fetch_pokemon.call # => #<FetchPokemon::Pokemon name="Charizard", height="17", weight="905">
pokemon.name                 # => "Charizard"
{% endhighlight %}

Com este refactoring saimos de um hash para uma entidade Pokémon, que deixa claro o que é aquele objeto. Afinal um hash que tem informações de um pokémon é um pokémon, então deixe isso explícito.
