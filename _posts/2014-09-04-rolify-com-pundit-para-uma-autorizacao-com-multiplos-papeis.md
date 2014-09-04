---
layout: post
title: Rolify com Pundit para uma autorização com múltiplos papéis
meta_description: Integre o Rolify com o Pundit para implementar uma autorização com múltiplos papéis em uma aplicação Rails
meta_keywords: Ruby, Rails, Autorização, Rolify, Pundit, Roles
category: Rails
tags: [Ruby, Rails]
---

No [último post utilizando o Pundit](http://groselhas.maurogeorge.com.br/autorizacao-com-pundit.html) vimos como implementar um sistema de autorização com um único papel(*single role*). Porém os requisitos mudam e temos que [responder a mudanças](http://agilemanifesto.org/). Nosso problema agora é:

> No sistema de gerenciamento de empresas temos 3 papeis o do gerente e o do empregado. O Gerente pode visualizar todas as telas do sistema. O empregado não pode criar, editar ou excluir nenhuma empresa. **O investidor deve visualizar apenas as empresas em que ele investe.**

Agora que nossos requisitos mudaram saimos de um sistema de um unico papel para multiplos papéis(*multiple roles*). Pois eu devo poder definir para cada um dos investidores quais empresas ele tem acesso, supondo que tenhamos as empresas A, B e C, deve ser possivel dizer que o investidor tem o papel *investidor* apenas na empresa A e C, ou seja ele possui 2 papeis diferentes.

## Conhecendo o Rolify

O [Rolify](https://github.com/RolifyCommunity/rolify) é uma gem responsável para lidar com papéis, possui uma interface legal que veremos durante o nosso exemplo.

## Setup

Começamos pelo clássico, adicionar `gem "rolify"` no nosso `Gemfile`. Depois de instalado temos que rodar o gerador do Rolify.

{% highlight bash %}
$ rails g rolify Role User
{% endhighlight %}

Depois do gerador criado rode a migration gerada com `$ rake db:migrate`. Além da migration gerada será criado um model `Role` e adicionado uma macro `rolify` no nosso model `User`.

No exemplo do Pundit utilizamos o `ActiveRecord::Enum` para definirmos o nosso papel, o ideal seriamos movermos toda esta parte de papeis agora para o rolify, para não ficarmos com 2 sistemas de papeis diferentes, não entrarei em detalhes dado a isso ser bem simples como verá a seguir.

## Definindo os recursos que aplicaremos um papel

Simplesmente adicionamos o macro `resourcify` no model que queremos utilizar na definição dos papeis.

{% highlight ruby%}
class Company < ActiveRecord::Base
  # ...

  resourcify
end
{% endhighlight %}

## Adicionando e removendo um papel

Agora com o setup definido vamos adicionar o nosso primeiro papel a um usuário. Para isso simplemente chamamos o método `#add_role` passando o papel e o recurso que estamos dando acesso ao usuário.

{% highlight ruby%}
user.add_role :investor, company
{% endhighlight %}

Não entrarei no mérito de controller, ou onde este papel é definido.

Para removermos um papel de um usuário simplesmente utilizamos do método `#remove_role`.

{% highlight ruby%}
user.remove_role :investor, company
{% endhighlight %}

Com isso já somos capazes de definir e remover papeis de usuários.

## Garantindo a integridade dos papeis

O Rolify nos permite definir qualquer papel e ainda em qualquer entidade, mesmo aquelas em que não foram aplicados o `resourcify`.

{% highlight ruby%}
user.add_role :magician, store
{% endhighlight %}

Como no nosso caso só temos o papel `investor` e o único recurso que estamos adicionando papel é o `Company` seria uma boa definirmos uma validação para isso. Assim evitamos que novos papeis sejam criados por engano, deixando o nosso [código mais explicito](http://legacy.python.org/dev/peps/pep-0020/).

O Rolify nos fornece um model `Role` então o que temos que fazer é apenas definir as validações de inclusão no mesmo.

{% highlight ruby%}
class Role < ActiveRecord::Base

  NAMES = %w{ investor }
  RESOURCE_TYPES = %w{ Company }

  validates :name, inclusion: { in: NAMES }
  validates :resource_type, inclusion: { in: RESOURCE_TYPES }

  # ...
end
{% endhighlight %}

Agora ao tentarmos adicionar um papel e uma entidade que não existem com o `#add_role` não seremos capazes, receberemos um objeto `Role` não persistido como retorno.

## Recuperando as empresas do usuário

O Rolify nos oferece *query methods* para interagirmos com nossos recursos que estão como `resourcify`. E com o método `.with_role` conseguimos recuperar todas as empresas em que possuem o papel `investor` e atrelada a um usuário especifico.

{% highlight ruby%}
Company.with_role(:investor, user)
{% endhighlight %}

Assim temos todas as empresas em que o `user` é investidor, com uma simples chamada de método.

## Pundit em conjunto com o Rolify

Voltando nos nossos requisitos, temos que listar apenas as empresas que o investidor investe. O Pundit nos permite definir um escopo baseado nas permissões do usuário para isso definimos uma classe `Scope` dentro do nosso `CompanyPolicy` e implementamos o método `#resolve`. Como estamos herdando do `Scope` do `ApplicationPolicy` já temos acesso ao `user` e ao `scope` a partir daí apenas fazemos a consulta que precisamos.

O Rolify define também o método `#has_role?` o utilizamos para verificar se o usuário possui qualquer papel de `investor`, caso ele possua listamos apenas as empresas em que ele tem acesso, caso não possua nenhum papel de investidor exibimos todas as empresas.

{% highlight ruby%}
class CompanyPolicy < ApplicationPolicy

  # ...

  class Scope < Scope
    def resolve
      if user.has_role?(:investor, :any)
        scope.with_role(:investor, user)
      else
        scope.all
      end
    end
  end
end
{% endhighlight %}

Com o nosso escopo definido simplesmente chamamos ele no controller com o `policy_scope`.

{% highlight ruby%}
def index
  @companies = policy_scope(Company)
end
{% endhighlight %}

Agora dependendo do papel do usuário é exibido uma listagem com todas as empresas, ou apenas com as que o usuário é investidor.

## Conclusão

O Rolify é uma excelente ferramenta para quando necessitamos lidar com multiplos papéis em uma aplicação rails, e facilita ainda mais a nossa vida ao integrarmos ele com o Pundit.
No nosso exemplo utilizamos o Active Record, mas o Rolify tem suporte ao Mongoid também.
Não deixe de visualizar a [wiki do Rolify](https://github.com/RolifyCommunity/rolify/wiki) para conhecer mais formas de adicionar/remover papéis, além das queries que ele implementa.

## Referências

- [https://github.com/elabs/pundit](https://github.com/elabs/pundit)
- [https://github.com/RolifyCommunity/rolify](https://github.com/RolifyCommunity/rolify)
- [http://eng.joingrouper.com/blog/2014/03/20/rails-the-missing-parts-policies/](http://eng.joingrouper.com/blog/2014/03/20/rails-the-missing-parts-policies/)
- [http://railsapps.github.io/rails-authorization.html](http://railsapps.github.io/rails-authorization.html)
- [https://github.com/RolifyCommunity/rolify/wiki](https://github.com/RolifyCommunity/rolify/wiki)
