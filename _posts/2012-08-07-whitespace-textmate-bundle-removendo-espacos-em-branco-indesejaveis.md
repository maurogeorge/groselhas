---
layout: post
title: Whitespace TextMate Bundle - removendo espaços em branco indesejáveis
meta_description: Removendo espaços em branco indesejáveis no textmate
meta_keywords: textmate, mac, editor, espaço em branco, branco, whitespace
category: Mac
tags: [Textmate, Mac, Editor]
---

É chato ter espaços em branco após suas declarações de código, piora ainda para quem é da sua equipe e quando da um `git pull` recebe seu código com uns espaços em branco sem sentido. Quem usa o textmate pode usar o [Whitespace TextMate Bundle](https://github.com/vigetlabs/whitespace-tmbundle) é um bundle bem simples, mas muito útil, a única coisa que ele faz é: **antes de salvar o seu artigo ele remove qualquer espaço em branco desnecessário**.

Para instalar apenas execute os seguintes comandos abaixo no terminal:

{% highlight bash %}
$ mkdir -p ~/Library/Application\ Support/TextMate/Pristine\ Copy/Bundles/
$ cd ~/Library/Application\ Support/TextMate/Pristine\ Copy/Bundles/
$ git clone git://github.com/vigetlabs/whitespace-tmbundle.git Whitespace.tmbundle
$ osascript -e 'tell app "TextMate" to reload bundles'
{% endhighlight %}

Verifique se o bundle foi instalado corretamente acessando no textmate **Bundles** e na listagem procure por **Whitespace**. Caso o bundle não apareça na lista faça **Bundles** > **Bundle Editor** > **Reload Bundles**.

E é isso, agora sempre ao salvar um arquivo, este terá seus espaços em branco removidos.