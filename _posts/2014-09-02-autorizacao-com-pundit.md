---
layout: post
title: Autorização com Pundit
meta_description: Conheça o Pundit uma excelente alternativa ao CanCan quando o assunto é autorização.
meta_keywords: Ruby, Rails, Autorização, CanCan, Pundit, CanCanCan
category: Rails
tags: [Ruby, Rails]
---

Quem é da comunidade ruby há algum tempo já é acostumado com o [CanCan](https://github.com/ryanb/cancan) e com o seu sucessor o [CanCanCan](https://github.com/CanCanCommunity/cancancan) que é a continuação do CanCan dado que o mesmo foi abandonado.

Em ambas das soluções temos apenas uma class `Ability` e definimos os nossos métodos de autorização ali. Para projetos pequenos pode ser uma boa saida, no entanto com o passar do tempo a complexidade aumenta e temos que extrair isso para classes expecificas e chama-las dentro da nossa interface do CanCan que é o model `Ability`. Mas e se já começassemos com uma abordagem mais inteligente? Com pequenas classes e cada uma definindo sua responsabilidade?

## Olá Pundit

O [Pundit](https://github.com/elabs/pundit) utiliza o conceito de [Policies ou Policy Objects](http://eng.joingrouper.com/blog/2014/03/20/rails-the-missing-parts-policies/) para lidar com autorização. Mas antes de entrarmos a fundo no Pundit primeiro devemos ter um problema, então nosso problema é:

> No sistema de gerenciamento de empresas temos 2 papeis o do gerente e o do empregado. O Gerente pode visualizar todas as telas do sistema. O empregado não pode criar, editar ou excluir nenhuma empresa.

Agora com o nosso problema em mão vamos iniciar. Não entrarei no mérito de autenticação, em que podemos utilizar o [devise](https://github.com/plataformatec/devise), nem na definição de papéis em que podemos utilizar o [ActiveRecord::Enum](http://groselhas.maurogeorge.com.br/good-bye-jacaranda-and-welcome-activerecord-enum.html).

## Setup

Como de costume adicionamos ao nosso Gemfile `gem "pundit"`. Depois de instalado incluimos o Pundit em nosso application controller.

{% highlight ruby %}
class ApplicationController < ActionController::Base
  include Pundit

  # ...
end
{% endhighlight %}

Agora executamos o seu generator para termos a nossa primeira policy definida.

{% highlight bash %}
$ rails g pundit:install
{% endhighlight %}

Esta nossa primeira policy ainda não é utilizada ela será utilizada como base para as policies que formos definir. Ela define alguns padrões como por exemplo o `destroy?` como `false`, sendo assim se invocarmos um policy de `destroy` mas não o definirmos por padrão será `false` desde que herdermos de `ApplicationPolicy`.

Assim como o CanCan o Pundit utiliza do método `current_user` para pegar o usuário atual.

## Criando nossa primeira Policy

Voltando lá ao nosso problema, devemos proteger as ações de criar, editar e excluir de empresa do usuário com o papel de empregado. Então vamos criar uma `CompanyPolicy`, para isso utilizamos do generator da seguinte maneira.

{% highlight bash %}
rails g pundit:policy company
{% endhighlight %}

Que nos gera uma classe para iniciarmos definindo a nossa policy. Então vamos definir primeiro que apenas o gerente pode criar uma nova empresa. Para isso definimos o método `create?`, por convenção o nome da action com um interrogação no final, e simplesmente utilizamos o método `@user.manager?` do `User`.

{% highlight ruby %}
class CompanyPolicy < ApplicationPolicy

  def create?
    @user.manager?
  end
end
{% endhighlight %}

Com isso garantimos que apenas o usuário com o papel de gerente pode criar uma nova empresa. Mas você deve estar se perguntando, por que eu defini apenas o `create?` e não defini `new?` afinal não queremos que o funcionário acesse a tela de criar.

Isto ocorre por que no nosso `ApplicationPolicy` temos definidos estas regras da seguinte maneira.

{% highlight ruby %}
def create?
  false
end

def new?
  create?
end
{% endhighlight %}

Ou seja `new?` executa o `create?` então devido ao poder da herança definimos apenas o `create?` em nossa classe filha. Este é um bom caso para o uso de herança, [mas não se esqueça que composição tem um papel muito importante também quando estamos falando de orientação a objetos](http://groselhas.maurogeorge.com.br/prefira-composicao-ao-inves-de-heranca-um-simples-exemplo-em-ruby.html).

Em seguida definimos as demais policies da nossa company.

{% highlight ruby %}
class CompanyPolicy < ApplicationPolicy

  # ...

  def update?
    @user.manager?
  end

  def destroy?
    @user.manager?
  end
end
{% endhighlight %}

Sem nada de novo aqui.

## Aplicando nossa policy

Agora com nossa policy criada vamos a nosso controller de company aplicá-la. Para isso simplesmente utilizamos do método `authorize` e passamos o objeto que estamos testando.

{% highlight ruby %}
def new
  @company = Company.new
  authorize @company
end
{% endhighlight %}

Não se esqueça de aplicar a policy para todas as actions que quer proteger.

Agora temos nossas actions protegidas, qualquer um que tentar acessar alguma dessas actions e não tiver permissão receberá uma excessão `Pundit::NotAuthorizedError`.

Protegemos o nosso controller, mas também não queremos links espalhados pelo sistema que levem a uma página de erro o ideal é esconde-los. Para isso vamos ao helper do pundit.

## Aplicando nossa policy na view

O Pundit nos oferece o helper `policy` para utilizarmos na view para checarmos alguma policy. Para checarmos a exibição do link de criar por exemplo, fazemos assim.

{% highlight ruby %}
- if policy(Company).new?
  = link_to 'Nova empresa', new_company_path
{% endhighlight %}

Passamos apenas a classe dado que não estamos referenciando nenhum registro. Na listagem passamos o proprio objeto para o Pundit.

{% highlight ruby %}
- if policy(company).update?
  td = link_to 'Editar', edit_company_path(company)
{% endhighlight %}

O legal do passarmos um objeto para dentro do Pundit, é que podemos por exemplo exibir o link de editar apenas para o gerente e em que a empresa tenha sido homologada por exemplo.

## Por uma boa mensagem de erro

Até o momento caso algum usuário acesso alguma das actions em que ele não tem permissão ele recebe apenas uma excessão, o que se torna um erro 500 em produção, e não queremos isso. Então vamos tratar esta excessão para exibirmos uma boa mensagem de erro.

No nosso `ApplicationController` resgatamos da excessão e redirecionamos o usuário com uma mensagem de erro.


{% highlight ruby %}
class ApplicationController < ActionController::Base

  # ...
  rescue_from Pundit::NotAuthorizedError, with: :user_not_authorized

  private

    def user_not_authorized
      flash[:error] = 'Você não tem permissão para fazer esta ação'
      redirect_to(request.referrer || root_path)
    end
end
{% endhighlight %}

Agora o usuário recebe uma melhor mensagem de erro.

## Conclusão

O Pundit é uma boa alternativa ao CanCan, no entanto utilizando de objetos Ruby sem nenhuma DSL ou nada *fancy*.
Não entrei no mérito dos testes para não alongar demais o post, mas o Pundit possui uma integração com o [RSpec](https://github.com/elabs/pundit#rspec) o que facilita bastante a nossa vida.
Não deixe de experimentá-lo e tirar suas próprias conclusões.
