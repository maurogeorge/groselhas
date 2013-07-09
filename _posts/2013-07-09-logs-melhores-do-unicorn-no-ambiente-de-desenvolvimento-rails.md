---
layout: post
title: Logs melhores do unicorn no ambiente de desenvolvimento Rails
meta_description: Dica de como ter um log melhor do unicorn ambiente de desenvolvimento do Rails
meta_keywords: unicorn, log, development, desenvolvimento, rails
category: Rails
tags: [Unicorn, Rails]
---

Uma boa prática é termos o nosso ambiente de desenvolvimento o mais próximo de produção, de preferência igual.

Há algum tempo trabalho em alguns projetos que usam o [Unicorn](http://rubygems.org/gems/unicorn), o problema que tinha era não ter um log em desenvolvimento parecido com o WEBrick.

Meus logs saiam como:


{% highlight bash %}
127.0.0.1 - - [09/Jul/2013 08:56:08] "POST /usuario/entrar HTTP/1.1" 200 - 0.4803
{% endhighlight %}

Para melhorar o log, podemos definir em `config/environments/development.rb` a seguinte configuração:

{% highlight ruby %}
config.logger = Logger.new(STDOUT)
{% endhighlight %}

E agora meus logs são mais bonitões, saindo assim:

{% highlight bash %}
Started POST "/usuario/entrar" for 127.0.0.1 at 2013-07-09 08:53:52 -0300
Processing by SessionsController#create as HTML
  Parameters: {"utf8"=>"✓", "authenticity_token"=>"OhLmZiYG84pfwAhftomXQaFno5Cq3sPNGAYc9U3KUxM=", "user"=>{"email"=>"johndoe@example.com", "password"=>"[FILTERED]", "remember_me"=>"0"}, "commit"=>"Entrar"}
  MOPED: 127.0.0.1:27017 QUERY        database=estoujogando_development collection=users selector={"$query"=>{"email"=>"johndoe@example.com"}, "$orderby"=>{:_id=>1}} flags=[] limit=-1 skip=0 batch_size=nil fields={"notifications"=>0} (21.7881ms)
Completed 401 Unauthorized in 189ms
Processing by SessionsController#new as HTML
  Parameters: {"utf8"=>"✓", "authenticity_token"=>"OhLmZiYG84pfwAhftomXQaFno5Cq3sPNGAYc9U3KUxM=", "user"=>{"email"=>"johndoe@example.com", "password"=>"[FILTERED]", "remember_me"=>"0"}, "commit"=>"Entrar"}
  Rendered sessions/_login_form.html.haml (16.0ms)
  Rendered registrations/_pre_register_form.html.haml (2.8ms)
  Rendered sessions/new.html.haml within layouts/login (31.1ms)
  Rendered application/_flash_messages.html.haml (9.0ms)
  Rendered application/_login_nav.html.haml (2.6ms)
Completed 200 OK in 378ms (Views: 284.3ms | Solr: 0.0ms)
{% endhighlight %}

É isso, post rápido e útil, espero que seja útil para alguem a dica.
