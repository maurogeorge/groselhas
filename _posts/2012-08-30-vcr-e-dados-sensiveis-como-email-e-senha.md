---
layout: post
title: VCR e dados sensíveis como email e senha
meta_description: Saiba como passar dados sensíveis ao VCR, como senhas e emails, mais não salvar isto no cassete
meta_keywords: ruby on rails, vcr, ruby, password, email
category: Ruby
tags: [Ruby on Rails, VCR, Ruby]
---

Quando estamos utilizando o [VCR](https://github.com/myronmarston/vcr) em nossas specs podemos nos deparar com o seguinte problema: como passar dados sensíveis ao VCR, como senhas e emails, mais não salvar isto em nosso cassete?

Vamos usar um exemplo real. Ao utilizarmos a api do [moip](http://site.moip.com.br/) de pagamento transparente temos que fazer um POST em uma uri

    https://MOIP_TOKEN:MOIP_KEY@desenvolvedor.moip.com.br/sandbox/ws/alpha/EnviarInstrucao/Unica

utilizando [Basic Authentication](http://en.wikipedia.org/wiki/Basic_access_authentication). Em um projeto interno, sem problemas deixar o seu dado de user e password nas specs, afinal o projeto é seu. Já em um projeto open source não seria legal todos terem acesso ao mesmo ambiente de desenvolvimento que você.

Para resolver isto devemos criar uma configuração no VCR, para isto em `spec/support` crie um arquivo com o nome de `vcr.rb` e nele devemos definir além do padrão do vcr que já usamos o:

{% highlight ruby %}
VCR.configure do |c|
  c.filter_sensitive_data('<MOIP_KEY>') { "my_key" }
  c.filter_sensitive_data('<MOIP_TOKEN>') { "my_token" }
end
{% endhighlight %}

Agora em todos os cassetes que o VCR gerá teremos algo assim:

    https://<MOIP_TOKEN>:<MOIP_KEY>@desenvolvedor.moip.com.br/sandbox/ws/alpha/EnviarInstrucao/Unica.

No entanto ainda não está bom, afinal apenas movemos os dados sensíveis de um lugar para o outro.
Para resolvermos de verdade isto podemos usar variaveis de ambiente, assim a única coisa que tem que fazer é definir as variáveis de ambiente em seu sistema e na configuração do VCR mudar para

{% highlight ruby %}
VCR.configure do |c|
  c.filter_sensitive_data('<MOIP_KEY>') { ENV["MOIP_KEY"] }
  c.filter_sensitive_data('<MOIP_TOKEN>') { ENV["MOIP_TOKEN"] }
end
{% endhighlight %}

Pronto! Agora temos um modo de distribuir uma gem, com as specs funcionando, e sem a necessidade de deixarmos hard coded nossas informações.

Para quem utilza o [rvm](https://rvm.io/) e seu arquivo [.rvmrc](https://rvm.io/workflow/rvmrc/), pode definir no .rvmrc as váriaveis de ambiente, assim elas serão carregadas apenas quando estiver dentro do diretório do projeto.

{% highlight bash %}
export MOIP_KEY="my_key"
export MOIP_TOKEN="my_token"
{% endhighlight %}

Não se esqueça de sair e entrar no diretório novamente caso tenha acabado de definir as variáveis de ambiente e de documentar a sua gem para quem for testar definir as váriaveis de ambiente.
