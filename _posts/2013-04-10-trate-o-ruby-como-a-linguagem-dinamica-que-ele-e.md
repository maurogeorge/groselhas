---
layout: post
title: Trate o ruby como a linguagem dinâmica que ele é
meta_description: Utilize o poder da linguagem dinâmica ruby sem transformá-lo em uma linguagem estática
meta_keywords: Ruby, OOP, dinâmica, duck typing
category: Ruby
tags: [Ruby, OOP]
---

O Ruby é uma linguagem de tipagem dinâmica, e para pessoas que vem de linguagens de tipagem estática tem que mudar sua mentalidade para trabalhar com uma linguagem dinâmica, veremos o que fazer e o que não fazer e como podemos lidar com o que as linguagens dinâmicas tem de ruim, afinal nem tudo são flores.

## Não fazemos checagem de tipos

Pode assustar um pouco a idéia de não fazermos nenhuma checagem de tipo, quando definimos um método que aceita parâmetros, definimos apenas o nome dos parâmetros e não o seu tipo:

{% highlight ruby %}

def my_name_is(name)
  "My name is #{name}"
end

{% endhighlight %}

Veja que não definimos nenhum tipo para o parâmetro `name`. Assim como não precisamos definir o tipo de nossos atributos de classe. E graças a esta não checagem de tipo que podemos usar uma das coisas mais legais da linguagem o Duck Typing, que veremos a seguir.

## Não fazemos interfaces

Imagine que em nosso sistema tenhamos 2 entidades Post e AwesomePost, que é um post em destaque. Agora sempre queremos garantir que ambos tenham definidos os atributos `title` e `content`.
Talvez tenderíamos a usar uma interface, no entanto o ruby não tem suporte a interfaces, mas podemos simular uma fazendo algo como:

{% highlight ruby %}

class BasePost

  def title
    raise "Not implemented"
  end

  def content
    raise "Not implemented"
  end
end

class Post < BasePost

  def initialize(title, content)
    @title = title
    @content = content
  end

  def title
    @title
  end

  def content
    @content
  end
end

class AwesomePost < BasePost

  def initialize(title, content)
    @title = title
    @content = content
  end

  def title
    "The awesome post: #{@title}"
  end

  def content
    "The awesome content: #{@content}"
  end
end

{% endhighlight %}

Que poderia ser utilizado:

{% highlight ruby %}
post = Post.new("A new post", "content")
p post.title # "A new post"
p post.content # "content"

post = AwesomePost.new("A new awesome post", "Awesome content")
p post.title # "The awesome post: A new awesome post"
p post.content # "The awesome content: Awesome content"
{% endhighlight %}

O código acima funciona, no entanto não é um bom código ruby.
O problema não é nas classes `Post` e `AwesomePost` e sim na classe `BasePost` que não faz absolutamente nada. `BasePost` existe apenas como uma tentativa de mantermos uma interface padrão para as classes `Post` e `AwesomePost` ou qualquer outra classe de `Post` que venhamos a criar.

No entanto é um esforço em vão pois como vimos anteriormente, não fazemos checagem de tipos na chamada de um método, sendo assim pouco importa a hierarquia de classes de um tal objeto, desde que ele saiba responder a tal método e é neste ponto que chegamos ao [Duck Typing](http://en.wikipedia.org/wiki/Duck_typing).

## Abrace o Duck Typing

Utilizando o Duck Typing, primeiro podemos remover a classe `BasePost` e remover a herança de `Post` e `AwesomePost` afinal ambas as classes implementam os mesmos métodos ficando assim:

{% highlight ruby %}

class Post
  # Mesmo conteúdo
end

class AwesomePost
  # Mesmo conteúdo
end

{% endhighlight %}

Agora imagine que tenhamos um decorator responsável por gerar um HTML para a exibição destas classes `Post`s sua implementação seria algo como:

{% highlight ruby %}
class PostDecorator

  def initialize(post)
    @post = post
  end

  def show
    %Q{<h1>#{@post.title}</h1>
       <div id="content">#{@post.content}</div>}
  end
end
{% endhighlight %}

E como já foi dito anteriormente que não fazemos checagem de tipos, seria aceito ambas as classes veja abaixo:

{% highlight ruby %}
post = Post.new("A new post", "content")
awesome_post = AwesomePost.new("A new awesome post", "Awesome content")

p PostDecorator.new(post).show # "<h1>A new post</h1>\n       <div id=\"content\">content</div>"

p PostDecorator.new(awesome_post).show # "<h1>The awesome post: A new awesome post</h1>\n       <div id=\"content\">The awesome content: Awesome content</div>"

{% endhighlight %}

E assim utilizamos o poder do Duck Typing, afinal:

    When I see a bird that walks like a duck and swims like a duck and quacks like a duck, I call that bird a duck.

Fazendo isso observamos a verdadeira diminuição de código de que se houve falar ao comparar linguagens estáticas com dinâmicas, e isto não está na remoção de `int`, `string` etc. na declaração de variáveis e sim na não declaração de classes abstratas e interfaces que nunca escreveremos.

## Sério, não tenha medo do Duck Typing

As vezes podemos encontrar código que tenta forçar uma tipagem estática, algo como assim:

{% highlight ruby %}
def initialize(title, content)
  raise "title isn't a string" unless title.kind_of?(String)
  raise "content isn't a string" unless content.kind_of?(String)
  @title = title
  @content = content
end
{% endhighlight %}

Por favor não faça isso. Pois assim só estará perdendo:

1. Destrói o baixo acoplamento da tipagem dinâmica
2. Aumenta bastante o código, no entanto melhora um pouco apenas a garantia de que vai funcionar.

Como pode ver utilizando o ruby como a linguagem dinâmica que ela é, diminuímos bastante a complexidade de nossos códigos.

## Mas como garantimos que tudo simplesmente funcione

Até agora vimos que não precisamos declarar tipo, mas e se por um acaso eu passar um tipo inválido para um método? Então temos as seguintes dicas para lidar com isso.

### Utilize nome descritivos

Abaixo criamos um método tags que recebe "n", pode ser claro para alguns que ele retorne as tags de um post limitando pela quantidade declarada em "n".

{% highlight ruby %}
def tags(n)
...
end
{% endhighlight %}

No entanto podemos fazer melhor:

{% highlight ruby %}
def tags(total_of_tags)
...
end
{% endhighlight %}

Como pode ver agora ficou mais fácil de saber que o valor é um inteiro. A dica é escreva nomes para serem lidos por humanos.

### Como eu sei qual tipo(s) o parâmetro de um método espera

Além da dica de nome descritivo, que já ajuda bastante.
Escreva testes automatizados, com testes saberá exatamente quais tipos de objetos são aceitos por um método ou não.

## Conclusão

* Não crie mas complexidade do que precisa
* aproveite o que a linguagem dinâmica tem para oferecer
* abrace o Duck Typing
* lembre-se que as classes não precisam de herança para compartilharem a mesma interface, só precisam implementar os mesmos métodos
* não faça checagem de tipos sem sentido, lembre-se abrace o Duck Typing
* Escreva código para humanos

Este post foi baseado no Cap. 8 do [livro Eloquent Ruby](http://www.amazon.com.br/Eloquent-Addison-Wesley-Professional-Series-ebook/dp/B004MMEJ36/ref=sr_1_1?s=digital-text&ie=UTF8&qid=1365598968&sr=1-1&keywords=eloquent+ruby), espero escrever mais alguns posts baseado em conteúdo do livro, recomendo fortemente a compra do mesmo.
