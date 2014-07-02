---
layout: post
title: Placehold de imagens com o placehold.it
meta_description: Como usar o placehold.it para gerar placeholder de imagens
meta_keywords: Front-end, placeholder, css, design
category: Front-end
tags: [Front-end, Dica]
---

Quando estamos fazendo o nosso Front-end é comum ter momentos onde ainda não termos o upload de imagem funcionando por exemplo e temos que exibir uma imagem padrão. Para evitar o processo de criar uma imagem no Photoshop com as medidas exatas podemos utilizar o [placehold.it](http://placehold.it/) que gera uma imagem para nós.

Para termos um resultado como o:

![Placeholder de 350x150](http://placehold.it/350x150)

Simplesmente criamos uma imagem utilizando o placehold.it da seguinte maneira.

{% highlight html %}
<img alt="Placeholder de 350x150" src="http://placehold.it/350x150">
{% endhighlight %}

Por padrão o formato é gif, podemos alterar simplesmente passando o formato.

{% highlight html %}
<img alt="Placeholder de 350x150" src="http://placehold.it/350x150.png">
{% endhighlight %}

Podemos passar um texto qualquer para ser renderizado, passando o parametro `text`.

{% highlight html %}
<img alt="Placeholder de 350x150" src="http://placehold.it/350x150.png&text=Mauro">
{% endhighlight %}

Podemos customizar a cor do background e do texto, passando os paramêtros nessa ordem.

{% highlight html %}
<img alt="Placeholder de 350x150" src="http://placehold.it/350x150.png/ffffff/000000&text=Mauro">
{% endhighlight %}

As cores devem ser definidas sempre depois das dimensões.

Fica aí com mais uma ferramenta para ajudar no desenvolvimento Front-end.
