---
layout: post
title: A notação de porcento em Ruby
meta_description: Entenda como funciona a notação de porcento no Ruby.
meta_keywords: Ruby, %, %q, %Q, %w, notação, porcento
category: Ruby
tags: [Ruby, Dica]
---

No Ruby temos a notação de `%` que veio inspirada do Perl. Além do `%` podemos usar um caractere modificador que muda o comportamento do nosso `%`. Vamos ver como cada um destes funcionam.

## %

Ao utilizarmos a sintaxe `%` temos o mesmo resultado do uso de aspas duplas, no entanto não temos a necessidade de escapar nenhuma aspas, seja simples ou duplas.

{% highlight ruby %}
blog = "Groselhas"
p %{O #{blog} é o blog do Mauro} # => "O Groselhas é o blog do Mauro"
{% endhighlight %}

## %q

Ao utilizarmos a sintaxe `%q` temos o mesmo resultado do uso de aspas simples, no entanto não temos a necessidade de escapar nenhuma aspas, seja simples ou duplas.

{% highlight ruby %}
blog = "Groselhas"
p %q{O #{blog} é o blog do Mauro} # => "O \#{blog} é o blog do Mauro"
{% endhighlight %}

## %Q

Utilizamos o `%Q` obtemos o mesmo resultado do uso de aspas duplas, assim como o `%`, ou seja `%` e `%Q` possuem o mesmo comportamento.

{% highlight ruby %}
blog = "Groselhas"
p %Q{O #{blog} é o blog do Mauro} # => "O Groselhas é o blog do Mauro"
{% endhighlight %}

## %w

Utilizamos o `%w` para a criação de arrays de strings, e como o `%q` ele não faz interpolação de strings.

{% highlight ruby %}
blog = "Groselhas"
p %w{#{blog} Mauro Blog} # => ["\#{blog}", "Mauro", "Blog"]
{% endhighlight %}

## %W

Com o uso do `%W` criamos um array de strings, e assim como o `%Q` podemos fazer interpolação de strings

{% highlight ruby %}
blog = "Groselhas"
p %W{#{blog} Mauro Blog} # => ["Groselhas", "Mauro", "Blog"]
{% endhighlight %}

## %i

Recentemente adicionado no Ruby 2.0, o `%i` é usado para gerarmos um array de symbols, e como você já deve esperar ele não faz interpolação de strings.

{% highlight ruby %}
blog = "Groselhas"
p %i{#{blog} item} # => [:"\#{blog}", :item]
{% endhighlight %}

## %I

O `%I` possui quase o mesmo comportamento do `%i`, no entanto ele faz interpolação de strings. Também adicionado no Ruby 2.0.

{% highlight ruby %}
blog = "Groselhas"
p %I{#{blog} item} # => [:Groselhas, :item]
{% endhighlight %}

## %r

Com o `%r` podemos criar uma expressão regular, e também podemos fazer interpolação de strings

{% highlight ruby %}
blog = "Groselhas"
p %r{#{blog}.*Mauro} # => /Groselhas.*Mauro/
{% endhighlight %}

## %s

Utilizamos o `%s` para a criação de um symbol, e não conseguimos fazer interpolação de strings.

{% highlight ruby %}
blog = "Groselhas"
p %s{#{blog} item} # => :"\#{blog} item"
{% endhighlight %}

## %x

Usamos o `%x` para executar comandos do sistema operacional, e ainda conseguimos fazer interpolação de strings.

{% highlight ruby %}
option = "-l"
p %x{ls #{option}} # => total 88\n-rw-r--r--   1 mauro...
{% endhighlight %}

## Trocando o delimitador

Em todos os nossos exemplos usamos o `{` para iniciar e o `}` para finalizar nosso comando, no entanto podemos usar qualquer um caractere que possua o seu par como os parêntesis(`(` e `)`) e o colchete(`[` e `]`). Caso utilizemos o mesmo delimitador não é necessário escapar desde que se use o de abertura e fechamento.
Também é possivel utilizar 2 caracteres iguais como delimitadores.
Veja um exemplo

{% highlight ruby %}
blog = "Groselhas"
p %Q{O #{blog} {blog do Mauro}} # => "O Groselhas {blog do Mauro}"
p %Q(O #{blog} { blog do Mauro) # => "O Groselhas { blog do Mauro"
p %Q$O #{blog} {blog do Mauro}$ # => "O Groselhas {blog do Mauro}"
{% endhighlight %}

Como pode ver ao trocarmos o delimitador podemos usar o `{` e o `}` livremente em nosso código.