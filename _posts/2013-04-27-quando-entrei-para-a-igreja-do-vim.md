---
layout: post
title: Quando entrei para a igreja do Vim 
meta_description: Por que comecei a usar o Vim e algumas dicas para quem quer começar também. 
meta_keywords: Vim, iniciante, .vimrc, peepcode
category: Vim
tags: [Vim]
---

O título do post foi baseado em um comentário do amigo [Argentino](https://twitter.com/argentinomota) enquanto conversavamos no Gtalk ele me perguntou por que eu tinha entrado para igreja do Vim.

## Por que o Vim?

Já há algum tempo tenho vontade de aprender, de verdade, um editor de texto, antes do Vim utilizava o Textmate mas não usava quase nenhuma de suas funcionalidades. E segui a seguinte lógica, se fosse para aprender um editor de texto iria aprender o Vim.

Recentemente assisti uns [PeepCode](https://peepcode.com/) com [Aaron Patterson](https://twitter.com/tenderlove), [Corey Haines](https://twitter.com/coreyhaines) e [Ben Orenstein](https://twitter.com/r00k) e todos utilizavam o Vim, além de uns amigos como o [Pedro](https://twitter.com/lunks), [VP](https://twitter.com/pellegrino) e o próprio Argentino. O que só me motivou mais e decidi botar o plano em prática.

## Por onde começar

Vou relatar aqui o que usei/tenho usado para o aprendizado.

- Os 2 PeepCode sobre vim [I](https://peepcode.com/products/smash-into-vim-i) e [II](https://peepcode.com/products/smash-into-vim-ii)
- [Tradução Aprenda Vim Progressivamente](http://blog.tarsisazevedo.com/post/16543244858/aprenda-vim-progressivamente) do [Tarsis Azevedo](https://twitter.com/tarsisazevedo)

## .vimrc

Caso escolha seguir o caminho de usar o Vim, irá saber que o .vimrc é um dos seus arquivos de configuração para o Vim.
Deixo aqui o link para o [gist da primeira versão do meu .vimrc](https://gist.github.com/maurogeorge/5253970/799fc152503b1316cc38caf7d34f8f712edd5bdf) foi baseado no do PeepCode, no entanto removi bastante coisa que não sabia para que servia, queria tentar manter o mais simples até começar a adicionar plugins e etc. Recomendo usar esta versão para quem está começando já que ele exibe os numeros das linhas, tem syntax highlight etc.
Quando foi escrito este post o meu [.vimrc estava assim](https://gist.github.com/maurogeorge/5253970/8c0ae40e0543786e4323d74066760e63a2dcf5c4) usando o [Vundle](https://github.com/gmarik/vundle) e alguns plugins.

Por hora estou usando apenas o gist, pois ainda estou adicionando os plugins conforme a necessidade surge, mas quem sabe algum dia use um repo para isso.

Caso queira dar uma olhada em como o pessoal tem configurado o vim, deixo aqui uns repos que andei observando também:

- [https://github.com/pellegrino/dotfiles](https://github.com/pellegrino/dotfiles)
- [https://github.com/lunks/vimfiles](https://github.com/lunks/vimfiles)
- [https://github.com/thoughtbot/dotfiles](https://github.com/thoughtbot/dotfiles)
- [https://github.com/r00k/dotfiles](https://github.com/r00k/dotfiles)
- [https://github.com/ryanb/dotfiles](https://github.com/ryanb/dotfiles)
