---
layout: post
title: Factory Girl e Paperclip usando os 2 de mãos dadas
meta_description: Saiba como resolver o problema de ter um campo obrigatório do Paperclip em uma de suas factories
meta_keywords: factory, factory_girl, paperclip, validacao
category: Ruby
tags: [Factory Girl, Paperclip]
---

Imagine a seguinte situação seu model User ganhou um atributo novo, avatar, e este atributo é uma imagem do [Paperclip](https://github.com/thoughtbot/paperclip/) detalhe que está imagem é obrigatória. Como arrumar a sua [factory](https://github.com/thoughtbot/factory_girl/) para definir esta imagem? Já que agora todos as specs que usam esta factory estão quebrando devido a obrigatoriedade do avatar.

## Usando um File

Talvez o primeiro movimento seria passar um file para a factory.

{% highlight ruby %}
FactoryGirl.define do
  factory :user do
    name "John Doe"
    avatar File.new('spec/fixtures/photo.jpg')
  end
end
{% endhighlight %}

No entanto isto é problemático pois:

A imagem realmente está sendo enviada para a S3, deixando meus testes mais lentos e dependentes de rede. Claro que posso usar o [VCR](https://github.com/vcr/vcr) para salvar estas requisições, mas imagina a quantidade de specs usando esta factory, imagina agora o trabalho de colocar todas estas specs no VCR, realmente não parece uma boa ideia. E o pior teria que se lembrar de toda vez que fosse usar tal factory as specs teriam que está usando o VCR.

## Usando um before create

A solução que tenho usado no momento é definir um before create na factory, definindo os campos salvos pelo paperclip.

{% highlight ruby %}
FactoryGirl.define do
  factory :user do
    name "John Doe"

    before :create do |user|
      user.avatar_file_name= "photo.jpg"
      user.avatar_content_type = "image/jpeg"
      user.avatar_file_size = 2588 
      user.avatar_updated_at = Time.zone.now
    end
  end
end
{% endhighlight %}

Assim não é enviado nada para a S3, assim não preciso me preocupar em chamar o VCR nas minhas specs antigas ou novas usando tal factory. E o melhor não preciso fazer nada onde a factory já é usada, pois agora ela simplesmente funciona não dando erro de validação do campo avatar.

Se você resolve este problema de outra forma ou achou útil a dica, comenta aí!
