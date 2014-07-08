---
layout: post
title: Flexibilidade na criação de métodos com named parameters
meta_description: Dicas de como criar métodos utilizando named parameters
meta_keywords: Ruby, 2.1, 2.0, named, parameters, keywords, arguments
category: Ruby
tags: [Ruby, Dica]
---

Temos que calcular o frete de um produto para isso temos que saber qual a sua altura, largura, profundidade, preço e peso deve ser possivel selecionar qual a unidade de medidas que enviaremos a informação.

Para resolver este problema criaremos uma classe que recebe estas opções. Como são diversas opções utilizaremos um hash, para não precisarmos ficar dependendo da ordem dos paramêtros.

{% highlight ruby %}
class ShippingCalculator

  def initialize(options = {})
    @options = options
  end

  def call
    options
  end

  private

    attr_reader :options
end
{% endhighlight %}

Nosso método `call` simplesmente retorna os paramêtros apenas para a simplicidade do exemplo, também não nos preocupamos em checar a presença dos valores. Nosso exemplo funciona e é um padrão adotado pela comunidade Ruby.

Percebemos que é muito comum definirmos o paramêtro `unit` sempre com o valor `:metric` para utilizar a unidade de medida métrica. Então vamos definir isto como padrão da nossa classe.
Para isso criamos uma constante com o valor como padrão e o juntamos com os paramêtros passados.

{% highlight ruby %}
class ShippingCalculator

  DEFAULT_OPTIONS = {
    unit: :metric
  }

  def initialize(options = {})
    @options = DEFAULT_OPTIONS.merge(options)
  end

  # ...
end
{% endhighlight %}

Legal! Agora não precisamos mais ficar passando o `unit` apenas quando formos utilizar outro sistema de medida que não seja o métrico.

A partir do Ruby [Ruby 2.0.0](https://www.ruby-lang.org/en/news/2013/02/24/ruby-2-0-0-p0-is-released/) foi incluido o suporte a *named parameters* o que nos permite declarar paramêtros nomeados que é exatamente como fizemos no exemplo anterior com o hash. Vamos alterar o nosso código parar utilizar do named parameter.

{% highlight ruby %}
class ShippingCalculator

  def initialize(unit: :metric)
    @options = { unit: unit }
  end

   # ...
end
{% endhighlight %}

Como pode ver temos acesso ao paramêtro passado diretamente, simplesmente o adicionamos a um hash para mantermos o nosso retorno.
No entanto no Ruby 2.0.0 para definirmos os named parameter temos que definir todos com um valor padrão. Se tentarmos passar o atributo `weight` por exemplo receberemos uma exceção `unknown keyword: weight (ArgumentError)` dado que ele não foi declarado.

Para não precisarmos ter que definir um valor padrão para cada um dos atributos podemos receber mais opções utilizando do `**` e juntarmos estas opções com os paramêtros que definimos no named parameter.

{% highlight ruby %}
class ShippingCalculator

  def initialize(unit: :metric, **options)
    @options = { unit: unit }.merge(options)
  end

   # ...
end
{% endhighlight %}

A partir do [Ruby 2.1.0](https://github.com/ruby/ruby/blob/v2_1_0/NEWS) foi removida esta necessidade de todos os named parameter ter que ter um valor padrão. Alteramos nosso exemplo para aceitar cada um dos named params.

{% highlight ruby %}
class ShippingCalculator

  def initialize(unit: :metric, weight:, lenght:, width:, height:, price:)
    @options = { unit: unit, weight: weight, lenght: lenght, width: width,
                 height: height, price: price }
  end

   # ...
end
{% endhighlight %}

Sei que você vai me dizer que este código é mais verboso do que aquele que fizemos com o Ruby 2.0 certo? Concordo, no entanto aqui ganhamos de bônus estes paramêtros serem requeridos, que foi uma parte da nossa classe que não implementamos, no entanto não queremos que seja possivel realizar um cálculo de frete sem o peso certo? Agora será lançada uma exceção `ArgumentError` informando os paramêtros que não foram enviados.

Lembrando que além do named parameter, que pode ter seu valor padrão definido ou não, podemos fazer como no exemplo do Ruby 2.0 e aceitar paramêtros extras.

{% highlight ruby %}
class ShippingCalculator

  def initialize(unit: :metric, weight:, lenght:, width:, height:, price:, **options)
    @options = { unit: unit, weight: weight, lenght: lenght, width: width,
                 height: height, price: price }.merge(options)
  end

   # ...
end
{% endhighlight %}

É muito legal ver um padrão criado pela comunidade ser adotado como funcionalidade da linguagem. Como pode ver temos uma grande flexibilidade quando vamos definir paramêtros para os nossos métodos.
