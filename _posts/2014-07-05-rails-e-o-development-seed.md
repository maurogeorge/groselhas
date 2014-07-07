---
layout: post
title: Rails e o Development Seed
meta_description: Saiba como e qual a vantagem de termos seeds que são definidos apenas no ambiente de desenvolvimento.
meta_keywords: Rails, Dica, Seed, development
category: Rails
tags: [Rails, Dica]
---

Você mudou de equipe e começou em um projeto novo, rodou bundle, migrate e iniciou o servidor. Criou uma conta neste novo sistema e... WTF este sistema faz?

É comum o projeto depender de algumas(muitas) informações para ele começar a mostrar estas informações ao invés de um monte de [*blank slates*](http://patternry.com/p=blank-slate/). Algumas destas informações algumas vezes são cadastradas apenas pela área administrativa do sistema. Como podemos deixar mais fácil a entrada de novos membros em um novo projeto?

No Rails podemos definir o nosso *seed* em `db/seeds.rb`. Mas como queremos que nosso seed seja carregado apenas em desenvolvimento vamos criar um arquivo especifico para ele em `db/seeds/development.rb`. Este arquivo não é carregado por padrão quando rodamos nosso seed, para fazer isso simplesmente o carregamos no nosso seed quando o ambiente for *development*.

{% highlight ruby %}
# db/seeds.rb

if Rails.env.development?
  require_relative 'seeds/development'
end
{% endhighlight %}

E agora simplesmente criamos o nosso seed.

{% highlight ruby %}
# db/seeds/development.rb

def create_posts_to(user)
  total_of_posts = rand(1..3)
  total_of_posts.times.each do |i|
    user.posts.create!(title: "Post #{i} from #{user.name}",
                       content: "Content of post #{i}")
  end
end

unless User.any?

  (1..10).each do |i|
    puts "Creating User #{i}"
    User.create!(name: "User #{i}", email: "user#{i}@example.com")
  end
end

unless Post.any?

  User.all.each do |user|
    puts "Creating posts to user #{user.name}"
    create_posts_to(user)
  end
end

{% endhighlight %}

Como queremos que o seed seja executado apenas quando não tivermos estas informações verificamos por elas antes de criarmos utilizando o `any?`. Criamos um pequeno método para gerar os posts, pois assim cada usuário não terá sempre o mesmo número de post para isso utilizamos o `rand`. E como dica sempre use métodos que lance uma exceção em caso de erro pois se alguma validação falhar por exemplo temos o feedback mais rápido.

Seguindo esta prática você está ajudando a:

- Novas pessoas que entrarem no projeto
- Você mesmo quando precisar reiniciar o banco de dados ou trocar de máquina

Lembrando que sempre temos que revisitar estes seeds pois novos campos são adicionados no banco, novas validações e mesmo novos models que ainda não foram criados. Podemos revisitá-los quando rodamos o seed em algum momento ou quando uma nova pessoa entra no projeto e sente falta de alguma informação. Ela já pode contribuir gerando esta informação no seed.

Aprendi esta técnica enquanto trabalhava com o [Fábio](https://twitter.com/fgrehm) =)

Pode parecer uma boa ideia utilizar FactoryGirl, mas não é ;p [saiba mais aqui](http://robots.thoughtbot.com/factory_girl-for-seed-data).

Caso queira dados "mais reais" pode utilizar o [faker](https://github.com/stympy/faker).

Precisando de uma solução mais complexa de uma olhada no [seedbank](https://github.com/james2m/seedbank).
