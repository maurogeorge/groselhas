---
layout: post
title: Git config - configurando o git
meta_description: Git config, saiba como configurar o git para um melhor aproveitamento da ferramenta.
meta_keywords: git, git config, alias, help, --global, shell, repositório, global
category: Git
tags: [Git]
---

Para quem está usando o git diariamente sabe que tem comandos que digitamos freqüentemente como <code>git status</code>, <code>git commit</code>, <code>git checkout</code> entre outros. Iremos ver como digitar menos e mais algumas configurações legais no git.

## Git config

É onde você define configurações do git que podem ser globais que serão válidas para todos os seus repositórios ou configurações de um repositório em especifico.

### Configurações globais

Para inserir alguma configuração com o git usamos o comando <code>git config</code> e caso queiramos que nossa configuração seja valida para todos os repositórios que criarmos passamos o parâmetro <code>--global</code> ficando <code>git config --global</code>.

Provavelmente você já executou o seguinte comando, passando seus dados obviamente, ao instalar o git:

{% highlight bash %}
$ git config --global user.name "Seu nome"
$ git config --global user.email "email@example.com"
{% endhighlight %}

caso tenha seguido algum tutorial e não saiba o que esta acontecendo aqui, o que você está fazendo é definir seu nome e email para o git que será usado em **todos** os seus repositórios.

### Configurações por repositório

Se ao passarmos o parâmetro <code>--global</code> definimos a informação para todos os repositórios, ao omitirmos o parâmetro definimos a informação para apenas um de nossos repositórios.

Lembrando de executar o comando estando **dentro do diretório onde encontra se o repositório que quer definir as configurações**. Exemplo:

{% highlight bash %}
$ git config user.email "email@empresa.com"
{% endhighlight %}

Assim você pode definir o email para o email de sua empresa por exemplo enquanto nos demais repositórios o email usado seria o seu pessoal.

Lembrando que o git é esperto o suficiente e só sobrescreve o que você redefinir e as demais configurações ele mantém. Ou seja no exemplo anterior ainda teriamos o valor nome definido como "Seu nome" em que definimos usando as configurações globais.

## Git alias

Agora que entendemos como definimos as configurações no git, iremos ver como podemos definir alias, um apelido, para os comandos que digitamos freqüentemente. Veja abaixo os alias que costumo definir como global.

{% highlight bash %}
$ git config --global alias.co "checkout"
$ git config --global alias.ci "commit"
$ git config --global alias.st "status -sb"
$ git config --global alias.br "branch"
$ git config --global alias.wdiff "diff --word-diff"
{% endhighlight %}

Agora com os alias definidos podemos fazer <code>git ci</code> ao invés de <code>git commit</code> quando formos realizar um commit. Além dos demais alias que definimos. Lembrando que você pode definir outros alias que julgar interessante.

Detalhe que defino como global pois quero isto válido em todos os meus repositórios.

### Um log bem mais poderoso

Ao executarmos <code>git log</code> obtemos o log de nossos commits. No entanto vem bastante informação e em um formato não muito agradável de se ler. Podemos melhorar o nosso log adicionando o seguinte alias.

{% highlight bash %}
$ git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset %Cblue[%an]%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)%Creset' --abbrev-commit"
{% endhighlight %}

Agora ao executarmos o comando <code>git lg</code> teremos um log com mais informações úteis do que o log padrão, lembrando que ainda poderá usar o <code>git log</code> normalmente.
Não se esqueça que podemos ainda usar o <code>git lg -p</code> para vermos o diff do que foi alterado nos commits.

## Mais configurações

### Editor de texto

Em alguns momentos um editor de texto é chamado pelo git, como ao darmos um commit sem mensagem com o comando <code>git commit</code>. Para escolhermos o nosso editor de preferencia podemos definir a configuração com o comando:

{% highlight bash %}
$ git config --global core.editor "mate -wl1"
{% endhighlight %}

Usei aqui o comando para chamar o textmate, e como está em seu [blog](http://blog.macromates.com/2005/textmate-shell-utility-tmmate/) devemos passar o parâmetro <code>-w</code> e o parâmetro adicional <code>l1</code> que passei é para o cursor se posicionar na primeira linha do arquivo.

Para o pessoal de linux, que não quiser o Vim, poderíamos definir o gedit por exemplo usando o comando:

{% highlight bash %}
$ git config --global core.editor "gedit"
{% endhighlight %}

### Colorindo o terminal

Se ao executar um <code>git status</code> por exemplo e não vermos colorido o que foi alterado, ou os novos arquivo por exemplo. É bom definir a configuração abaixo.

{% highlight bash %}
$ git config --global color.ui true
{% endhighlight %}

que como você deve imaginar, colore as informações que o git lhe enviar no terminal.

### O git te corrigindo ao digitar errado

Em alguns momentos pode acontecer de digitarmos errado como por exemplo digitar <code>git lg</code> ao invés de <code>git log</code>. Para não tomarmos um aviso de comando errado simplesmeste, podemos deixar o git definir por nos o que ele acha que tentamos executar, para isso habilitamos a seguinte configuração:

{% highlight bash %}
$ git config --global help.autocorrect 1
{% endhighlight %}

Assim ele nos dará um aviso e executará o comando que ele acredita que tentamos digitar.

### Evitando tiro de shotgun com o git ao dermos push

Como o [Argentino](http://twitter.com/argentinomota) diz o git da um tiro de shotgun ao executarmos o <code>git push</code>, isso por que os commits de outros branches são enviados, mesmo estando em um branch especifico.

Imagine que tenhamos 3 branches master, feature/awesome-thing e feature/other-thing, ambos feature branches tem commits locais que ainda não foram enviados, estando no branch feature/awesome-thing e executar um <code>git push</code> **os commits dos 2 feature branches** serão enviados, pode não ser o comportamento esperado no momento.

Utilizando o:

{% highlight bash %}
$ git config --global push.default current
{% endhighlight %}

dizemos para o git que no <code>git push</code> ele deve enviar o conteúdo do branch atual para um branch de mesmo nome. Sendo assim se executassemos o <code>git push</code> no exemplo anterior, apenas o branch feature/awesome-thing seria enviado, o que faz sentido pois é o branch que estamos no momento. Mais informações e outras configurações possiveis podem ser encontradas no [manual do git](http://schacon.github.com/git/git-config.html).

### Não precisando resolver conflitos já resolvidos

É comum quando estamos em um feature branch que tenha uma vida relativamente longa, fazermos merge do branch master em nosso feature branch. Em algum destes merges talvez tenhamos que resolver algum conflito. O problema ocorre quando formos fazer merge do nosso feature branch no master, pois teremos que resolver os mesmos conflitos. Para resolvermos isto podemos ativar o `rerere`(Reuse recorded resolution of conflicted merges) com o seguinte comando.

{% highlight bash %}
$ git config --global rerere.enabled true
{% endhighlight %}

### Documentação em HTML

Caso queira que a documentação, ao digitar <code>git help comando</code> seja em HTML pode usar a seguinte configuração:

{% highlight bash %}
$ git config --global help.format web
{% endhighlight %}

Assim ao digitar por exemplo <code>git help config</code> será aberto o browser e poderá ver a documentação do comando digitado em HTML.

Lembrando que para ver a documentação em HTML a mesma deve ser instalada, [mais informações de como instalar aqui](http://help.github.com/install-git-html-help/).

## Listando as configurações

Agora que você já sabe definir configurações tem que ter um lugar onde seja possível ver o que esta configurado, e para isso usamos o comando

{% highlight bash %}
$ git config -l
{% endhighlight %}

que executado fora de qualquer repositório listará nossas configurações globais. E ao estar dentro de algum repositório, listará as configurações do repositório em especifico.

E se ainda assim dentro do diretório do repositório quiser listar as configurações globais. Podemos usar o comando:

{% highlight bash %}
$ git config --global -l
{% endhighlight %}


## Shell

Caso goste de definir seus alias no shell, pode usar o mesmo para definir comandos do git. No mac utilizaríamos o ~/.bash_profile ou o ~/.bash_profile no linux, exemplo:

{% highlight bash %}
alias go='git checkout '
alias gc='git commit'
alias gs='git status '
alias gb='git branch '
{% endhighlight %}