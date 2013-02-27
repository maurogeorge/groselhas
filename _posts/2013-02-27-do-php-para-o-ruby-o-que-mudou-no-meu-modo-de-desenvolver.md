---
layout: post
title: Do PHP para o Ruby o que mudou no meu modo de desenvolver
meta_description: O que mudou no meu modo de desenvolver depois de mais de 1 ano trabalhando com Ruby
meta_keywords: Ruby
category: Ruby
tags: [Ruby]
---

Este post não é uma comparação de linguagens para saber qual é a melhor, afinal linguagens são só ferramentas. Hoje estou no Ruby quem sabe amanhã não seja no Closure, Scala ou qualquer outra. O objetivo aqui é dizer o que mudou no meu modo de desenvolver depois de mais de 1 ano trabalhando com Ruby.

## Controle de versão

Onde eu trabalhava era muito complicado convencer que o Git era uma boa ferramenta, apesar de eu usar sozinho, mas convencer a equipe toda a usar era complicado, era questionado ainda o por que de não usar o SVN, sim em 2011.

Na [HE:labs](http://helabs.com.br) utilizamos o Git, todos do time, inclusive os designs não sendo problema para nenhum do time o seu uso.

## Testes

Trabalhando com PHP tive poucas oportunidades de fazer testes, era o usual vai demorar mais etc. O Pouco que eu fiz, foi bem pouco mesmo em projetos de clientes e fico triste em conversando com o pessoal da comunidade que esta é a realidade. E no [BoletoPHP](https://github.com/maurogeorge/boletophp/tree/2.x-dev) onde estava fazendo testes, migrando a aplicação do procedural para uma orientada a objetos.

No Ruby a comunidade é mais madura em relação a isso, toda Gem(lib) só é considerada completa se possui testes, afinal você não quer instalar uma dependência em seu sistema que pode estar funcionando. E no trabalho todos os nossos projetos na [HE:labs](http://helabs.com.br) possuem testes e outra **100%** testados, não vai nenhum código para produção que não possui 100% de cobertura de testes.

Se você não possui 100% de coverage em seus testes deixo apenas este [vídeo aqui do Uncle Bob](http://cleancoder.posterous.com/100-test-coverage).

## Integração e deploy continuo

Trabalhando com PHP para colocar um projeto em produção/staging era através de FTP, consegui mudar um pouco isso inserindo um simples hook no Git para fazer o deploy. Mas podia ser melhor.

Hoje em dia utilizamos o [Integration](https://github.com/mergulhao/integration) que envia o nosso código para o ambiente de staging, e produção quando necessário, no entanto antes de enviar qualquer código entre outras coisas é rodada todas as specs, e verificado se está com 100% de coverage, caso não esteja com 100% de coverage o código não é enviado. Garantindo assim apenas que código que esteja funcionando seja integrado ao projeto.

## Técnicas de orientação a objetos

No PHP, as ferramentas mais populares infelizmente são procedurais, e acredito que continuaram assim por muito tempo, entre elas digo o WordPress e o Drupal, fazem muito bem o seu papel, mas não são nenhum exemplo de engenharia de software. Os frameworks por outro lado, possuem um código orientado a objetos e com testes como o Zend Framework e o CakePHP no entanto via muito as pessoas presas ao MVC que o framework oferecia e só.

Em Ruby como temos nosso código 100% testado, não ficamos apenas no MVC e no Fat Models Skiny Controllers, preferimos o Skiny Models e Controllers e uma camada de serviço. Esta camada de serviço, além de services, temos outras classes como Decorators, FormObjets etc separados logicamente claro.

## Refactories KISS, DRY e YAGNI

Trabalhando com PHP se uma coisa estava "pronta" ela estava "pronta", pois como não possuía testes não me sentia confortável em refatorar algo já que teria que testar toda a interação pelo browser o que com certeza teria falhas.

Em Ruby tenho total confiança para fazer um refactory, pois se algo quebrar o meu teste irá avisar. E assim consigo aplicar técnicas de [KISS](http://en.wikipedia.org/wiki/KISS_principle) quebrando métodos maiores em menores ou classes grandes em mais de uma classe dividindo a responsábilidade, [DRY](http://en.wikipedia.org/wiki/DRY_principle) se eu tenho repetição refatoro até não à ter e [YAGNI](http://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) desenvolvendo para a interface e não criando métodos que posso precisar, desenvolvo utilizando [TDD](http://en.wikipedia.org/wiki/Test-driven_development) para a minha interface.

## Agile no mundo real

Trabalhando com PHP nunca trabalhei com agile, era sempre escopo fechado.

Atualmente trabalho com agile, mas especificamente com [XP](http://en.wikipedia.org/wiki/Extreme_programming). Temos iterações semanais, contrato de escopo variável, testes automatizados e diversas outras práticas do XP.

Como pode ver considero que evolui bastante migrando do PHP para o Ruby. E com certeza não foi apenas a mudança de linguagem mais sim a mudança para a [HE:labs](http://helabs.com.br) onde temos um [time foda](http://helabs.com.br/#time)! E você tem evoluído bastante? Comenta aí.


