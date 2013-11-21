---
layout: post
title: Criando um ambiente de staging no heroku para nossa rails app
meta_description: Dicas de como criar um ambiente de staging no heroku para nossa rails app
meta_keywords: heroku, rails, staging
category: Heroku
tags: [Rails, Heroku]
---

É uma boa prática termos um ambiente que rode no heroku, mas que não seja o ambiente de produção, este ambiente é chamado de staging, um ambiente que é o mais proximo de produção.

Primeiro temos que criar uma app de staging no heroku, dado que nossa app tenha o nome de `myapp` crie uma heroku app com o nome de `myapp-staging` para isto podemos utilizar do [`heroku fork`](https://devcenter.heroku.com/articles/fork-app).

{% highlight bash %}
$ heroku fork -a myapp myapp-staging
{% endhighlight %}

Primeiro temos que dizer ao rails que temos um ambiente de staging para isso temos que criar o `config/environments/staging.rb`. Simplesmente cria o arquivo `config/environments/staging.rb` com o mesmo conteúdo do `config/environments/production.rb` apenas trocando o `log_level` para `:debug`.

{% highlight ruby %}
Myapp::Application.configure do

  # ...
  config.log_level = :debug
end
{% endhighlight %}

No heroku agora utilizamos a gem [`rails_12factor`, no entanto ele apenas recomenda o uso em `:production`](https://devcenter.heroku.com/articles/rails4#logging-and-assets), devemos incluir o nosso ambiente de staging aqui também no nosso Gemfile.

{% highlight ruby %}
gem "rails_12factor", '0.0.2', group: [:production, :staging]
{% endhighlight %}

Como este ambiente é um ambiente para apenas os usuários que desejamos visualizar, como os colaboradores do nosso sistema, colocamos um basic auth apenas no ambiente de staging.

{% highlight ruby %}
class ApplicationController < ActionController::Base

  #...
  http_basic_authenticate_with :name => "name", :password => "secret" if Rails.env.staging?
end
{% endhighlight %}

Para finalizar definimos as variaveis de ambiente no heroku, devemos definir o `RACK_ENV` e o `RAILS_ENV`.

{% highlight bash %}
$ heroku config:set RACK_ENV=staging RAILS_ENV=staging --app myapp-staging
{% endhighlight %}

Caso estejamos utilizando serviços como Amazon S3, Facebook etc. É uma boa pratica termos um bucket exclusivo de staging no caso da S3 e uma app do facebook para staging, assim como qualquer outro serviço que estejamos utilizando.

Adicione as variaveis de ambiente destes serviços como S3(`AMAZON_S3_BUCKET`, `AMAZON_ACCESS_KEY_ID`, `AMAZON_SECRET_ACCESS_KEY`), Facebook(`FACEBOOK_APP_KEY`, `FACEBOOK_APP_SECRET`) e qualquer outra variável de ambiente necessária para a sua app funcionar.

Deste modo temos uma app que é bem próxima de produção, assim reduzimos a chance de termos alguma surpresa desagradável.
