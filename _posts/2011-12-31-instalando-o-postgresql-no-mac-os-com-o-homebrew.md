---
layout: post
title: Instalando o PostgreSQL no Mac OS com o homebrew
meta_description: Instalando o PostgreSQL no Mac OS com o homebrew, saiba como fazer.
meta_keywords: PostgreSQL, sql, mac, os x, postgresql, psql, homebrew, brew
category: SQL
tags: [SQL, Mac]
---

Para o pessoal que usa Mac deve conhecer o gerenciador de pacotes [Homebrew](http://mxcl.github.com/homebrew/), caso não conheça acesse o [post do Pedro Menezes sobre o assunto](http://pedromenezes.com/conheca-o-homebrew-o-melhor-gerenciador-de-pacotes-para-mac-os) e instale o Homebrew.

Tendo devidamente o homebrew instalado, execute o seguinte comando:

{% highlight bash %}
$ brew update
{% endhighlight %}

para atualizar o homebrew e suas formulas.

## Instalando o PostgreSQL

Apenas execute o seguinte comando

{% highlight bash %}
$ brew install postgresql
{% endhighlight %}

e aguarde o homebrew fazer todo o processo de instalação para você. Depois de instalado ainda temos algumas pequenas coisas para fazer.

## *Caveats*

Depois de instalado o PostgreSQL o homebrew nos informa sobre as ressalvas, ou *caveats* em inglês, recomendo a leitura do mesmo para seu conhecimento. Nestas ressalvas ele nos informa alguns passos que devemos seguir.

### Iniciando o banco de dados

Depois de instalado o PostgreSQL devemos iniciar o banco de dados com o simples comando:

{% highlight bash %}
$ initdb /usr/local/var/postgres
{% endhighlight %}

apenas aguarde a execução do comando.

#### Problemas com encoding

Caso apresente algum problema de encoding no template de banco de dados, pode utilizar o comando a seguir para forçar o PostgreSQL a criar o template em UTF-8 por exemplo:

{% highlight bash %}
$ initdb /usr/local/var/postgres --locale=en_US.UTF-8 --encoding=UNICODE
{% endhighlight %}

mais informações sobre os comandos passados ao initdb no [manual do PostregesSQL](http://www.postgresql.org/docs/9.1/static/app-initdb.html).

### Iniciando o PostgreSQL após realizar o login na máquina

Caso você queira, o que acredito que seja o seu caso, iniciar o PostgreSQL após realizar o login em sua máquina execute o seguinte comando abaixo:

{% highlight bash %}
$ mkdir -p ~/Library/LaunchAgents
$ cp /usr/local/Cellar/postgresql/9.1.2/org.postgresql.postgres.plist ~/Library/LaunchAgents/
$ launchctl load -w ~/Library/LaunchAgents/org.postgresql.postgres.plist
{% endhighlight %}

Observe que aqui estamos usando o PostgreSQL 9.1.2 sendo assim a sua instalação mudará o path no comando acima dependendo da versão do PostgreSQL, por isso é importante ler as mensagens que o homebrew lhe informar para inserir o comando acima corretamente.

### Iniciando e parando o PostgreSQL manualmente

Caso queira iniciar o PostgreSQL manualmente utilize o comando abaixo:

{% highlight bash %}
$ pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start
{% endhighlight %}

e para parar a execução do PostgreSQL utilize o comando abaixo:

{% highlight bash %}
$ pg_ctl -D /usr/local/var/postgres stop -s -m fast
{% endhighlight %}

### Criando o super usuário postgres

Para criar o super usuário postgres devemos primeiro entrar no psql com

{% highlight bash %}
$ psql postgres
{% endhighlight %} 

caso ele reclame de não existir a database postgres utilize o seguinte comando para criar a mesma, em seguida acesse o psql como indicado acima.

{% highlight bash %}
$ createdb postgres
{% endhighlight %}

Dentro do psql execute o seguinte comando para criar o super usuário postgres e depois para sair do psql utilize <code>\q</code>.

{% highlight sql %}
CREATE USER postgres SUPERUSER;
{% endhighlight %}

Assim teremos criado o super usuário postgres sem senha para o PostgreSQL.













