---
layout: post
title: Como foi criar um jogo para fazer um pedido de casamento
meta_description: Um pouco do processo de como foi criar um jogo para fazer o pedido de casamento para a Jéssica.
meta_keywords: JavaScript, HTML 5, Game, Casamento, Mauro, Jéssica
category: JavaScript
tags: [JavaScript]
---

## A ideia

Ano passado em algum dia próximo ao fim do ano a noite estava em casa e me bateu
uma ideia "E se eu fizesse um jogo para pedir a Jéssica em casamento?". Começei
a pensar um pouco sobre o assunto, fui anotando as ideias no Google Calendar.
Sim, utilizei o calendar pois pretendia começar o projeto apenas em Janeiro.

Anotei que teria que ser um jogo de plataforma, com a Jéssica como personagem
principal, jogo 2D, assets do Mario, interação com personagens conhecidos
durante a fase etc.

## Imprevistos acontecem

Comecei o ano de 2016 doente, fiquei com dengue e não consegui trabalhar nem me
dedicar ao novo projeto.

Tive que ficar de repouso e visitar o médico basicamente de 2 em 2 dias.

Mas deu tudo certo e depois de aproximadamente 2 semanas, consegui começar o
projeto.

## Organização do projeto

Para controlar as tarefas e o que fazer no projeto utilizei o Trello. Com um
simples kanban com 3 colunas "A fazer", "Fazendo" e "Finalizado".

Além disso criei uma coluna chamada "Sempre lembrar de" de coisas que deveria
ter sempre em mente enquanto trabalhava no projeto. Como por exemplo tinha um
card com a ideia inicial, um que falava para manter as coisas simples, um para
salvar todas as referências etc.

### O planning

Não queria que este projeto se extendesse por muito tempo, queria que no começo
de maio já o tivesse apresentado para Jéssica e feito uma surpresa a ela.

Então criei um card na coluna "Sempre lembrar de" para analisar a duração das
tarefas. Então toda segunda que, era quando começava a trabalhar no projeto, eu
começava olhando para a semana anterior e vendo se ela foi produtiva, seja de
aprendizado ou de desenvolvimento de fato.
Sempre colocava um _due date_ no card assim toda vez eu era lembrado dele quando
chegava o começo da iteração.

Além de criar mais tarefas para serem realizadas, caso ainda não tenham sido
criadas. Também era comum tarefas serem criadas durante o próprio
desenvolvimento.

### Roadmap

Sabemos que tentar ser previsivel no desenvolvimento de software é algo
complicado. Levando em conta que estava desenvolvendo algo que nunca tinha feito
antes então a imprevisibilidade era maior ainda.

No entanto depois de ter algum conhecimento do processo, provavelmente depois de
ter finalizado a prova de conceito, decidi criar um roadmap para ver se as
tarefas que eu havia criado seriam plausiveis de serem realizados dado que tinha
um deadline.

E como todo bom roadmap ele falhou miseravélmente. No dia 25/03/2016 eu fiz o
pedido de casamento. Ou seja com aproximadamente um mês de antecedência.

### Acompanhando o andamento

Utilizo o [Coach.me](https://www.coach.me/) há algum tempo para acompanhar
certas tarefas.

Para quem não o conhece é uma ferramenta para ajudar a criar hábitos e o legal é
poder acompanhar o andamento, e assim ver quando foi realizada tal tarefa.

Então para me dedicar a este projeto eu tive que parar outros, como por exemplo
contribuir com projetos open source. Sendo assim criei este novo hábito no
Coach.me e coloquei que iria me dedicar a ele de segunda a quinta em torno de 2
horas por dia.

Segue a visão do Coach.me dos dias que trabalhei no projeto.

![Andamento do projeto no Coach.me](/images/posts/2016-04-06/coach-me.png)

Como pode ver foram 42 dias que trabalhei no projeto. Algumas semanas com mais
dias e outras com menos, provavelmente devido a ter que resolver outros
compromissos nas que estavam com menos e nas que estavam com mais dias é devido
a não ter compromisso em alguns sábados em especifico e nestes normalmente o
trabalho foi maior do que 2hs.

### Ritmo sustentável

Para quem esteja se perguntando por que de parar uma coisa para começar outra.
Primeiro para ter foco e conseguir entregar algo de qualidade. É bastante
complicado querermos iniciar e manter N iniciativas ao mesmo tempo ainda mais
que todos nós trabalhamos e temos outras tarefas no nosso dia a dia. Então
preferi cortar algumas atividades por um tempo.

Há um tempo escrevi sobre
[Ritmo Sustentável no blog da HE:labs](http://helabs.com/blog/2015/02/06/ritmo-sustentavel/).
Além do ritmo sustentável no trabalho trago esta prática para os projetos
paralelo que inicio também.

## Qual engine usar?

Como fazer um jogo é algo que nunca tinha feito, comecei realizando pesquisas de
engines para jogos 2D e que atendecem aos requisitos que eu tinha que
desenvolver.

A principio a ideia era utilizar Unity, no entanto continuei pesquisando
[engines](https://github.com/showcases/javascript-game-engines)
de [JavaScript](https://html5gameengine.com/), passei por
[Ruby](https://www.ruby-toolbox.com/categories/game_libraries) também.

Por mais que desenvolver algo em Unity fosse uma experiência nova e
desafiadora, tinha que lembrar que tinha um deadline então o que fosse mais
próximo da minha realidade seria melhor de atingir um resultado num espaço de tempo
menor.

Então depois de pesquisar diversas engines de JavaScript acabei escolhendo o
[Phaser](http://phaser.io/) que parecia resolver meu problema, tinha diversos
cases e uma documentação boa.

## Prova de conceito

Escolhida a Game Engine, parti para a prova de conceito. Como tinha que ter um
servidor web rodando e não poderia ser apenas HTMLs soltos dentro de uma pasta,
montei um pequeno projeto utilizando o
[GitHub Pages](https://pages.github.com/). Antes utilizei um pouco do
SimpleHTTPServer do Python, mas depois que comecei a desenvolver a projeto de
fato prefiri utilizar o Github Pages pois assim facilitaria o deploy e também
estou mais acostumado com o mundo Ruby de qualquer maneira.

Comecei lendo alguns
[tutoriais](https://software.intel.com/en-us/html5/hub/blogs/how-to-make-a-sidescroller-game-with-html5)
[sobre](http://phaser.io/tutorials/getting-started) o
[Phaser](http://phaser.io/tutorials/making-your-first-phaser-game).

E ir implementando cada um dos requisitos iniciais que eu tinha.

## Resolução

Antes estava utilizando a resolução de 800X600. No entanto ao adicionar um
sprite do Mario Bros de NES numa resolução dessa, como pode imaginar o
personagem ficou bem pequeno.

Então pesquisei sobre qual era a resolução do NES para fazer exatamente a mesma.
Com isso alterei a resolução do jogo para a mesma do NES. No entanto em nossos
monitores com resoluções maiores como deve imaginar a resolução de 256x240 é bem
pequena.

Para resolver o problema eu fiz o resize do jogo diretamente pelo CSS aumentando
em 2 vezes a resolução.
Pesquisei sobre algumas soluções de fazer o resize, fiz testes e no final das
contas utilizando apenas CSS consegui o mesmo resultado e com menos
complexidade.

## Game Design

No meio dos estudos acabei conhecendo o [Tiled](http://www.mapeditor.org/) uma
excelente ferramenta para criação de tile maps.

Com o Tiled consegui importar facilmente incluir o tilemap do Mario Bros de NES
e criar a fase conforme eu quisesse.

Um fato legal do Tiled, é que por um acaso no processo do desenvolvimento do
jogo estava jogando Shovel Knight: Plague of Shadows e ao finalizar percebi que
nos créditos é listado o Tiled. Fiquei feliz de está utilizando a mesma
ferramenta que o pessoal do Shovel Knight.

Como os arquivos originais do Tiled o `.tmx` é um XML e não era grande preferi
manter eles no repositório e estava utilizando o formato JSON para carregar o
tile map no jogo.

## Assets

Para os assets utilizei o [The Spriters Resource](http://www.spriters-resource.com/)
onde consegui os tile maps, itens, inimigos etc.

Para criar os personagens customizados, no caso Jéssica e eu, utilizei o
[Pixel.Tools](http://pixel.tools/) um editor de pixel-art online.
Na criação dos personagens customizados mantive a mesma paleta de cores do NES.
Utilizei também o Pixel.Tools para gerar o logo do jogo.

Para os efeitos sonoros utilizei o [The Sounds Resource](http://www.sounds-resource.com).

## Vendor

No processo preciva criar balões de fala no entano não queria ter que fazer
isso do zero. Acabei encontrando uma
[discussão sobre isso aqui](http://www.html5gamedevs.com/topic/8837-speech-bubble-text-with-rectangle-as-background/)
a solução pode ser encontrada [aqui](http://jsfiddle.net/lewster32/81pzgs4z/).
Para manter isso isolado, adicionei tal código em uma pasta de vendor e alterei
para atingir o que eu realmente precisava.

Não achei nenhum plugin pronto para o Phaser que fazia isso, aparentemente tem
algum sistema de plugins, mas não cheguei a utilizar nenhum plugin no processo.

## Roteiro

Além das tarefas de programar o jogo, era necessário escrever os textos para o
mesmo, então para não negligenciar tais pontos criei tarefas no Trello para
criar texto para todas as partes que tinha texto envolvido.

## Testes e mais testes

Foi necessário bastante teste para ver se tudo estava rodando como deveria, e no
caso aqui abrir o browser e rodar de fato o jogo, passar pelo jogo e ver se
estava tudo certo.

Como a ideia era fazer o pedido o mais rápido possivel não pesquisei sobre
testes automatizados.
Acredito que seja razoavelmente simples testar classes especificas do jogo de
forma unitária. Agora testes de ponta a ponta não cheguei a pesquisar.

## Mundo real

Além das tarefas relacionadas ao jogo também criei outras tarefas no Trello
relacionado ao mundo real como por exemplo "Comprar aliança".
Era importante criar tais tarefas para não esquecer de nada, além de poder
priorizar com clareza o que deveria ser realizado na semana.

## Tá mas e o tal jogo?

Então agora que você chegou aqui fica aqui o link para o código no
[GitHub](https://github.com/maurogeorge/segredario) tem também uma
[tag para a prova de conceito](https://github.com/maurogeorge/segredario/releases/tag/proof-of-concept)
e uma [tag para versão que foi utilizada para fazer o
pedido](https://github.com/maurogeorge/segredario/releases/tag/v1.0.0).

Além disso você pode jogar o jogo em [segredario.maurogeorge.com.br](http://segredario.maurogeorge.com.br/).
Recomendo utilizar o Chrome, não sei por que mas no
[Firefox](https://github.com/maurogeorge/segredario/issues/8) a perfomance não
está boa.

Espero que curtam =D
Sim ela curtiu e estamos noivos!
