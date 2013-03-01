---
layout: post
title: Testando no Internet explorer 6, 7, 8 e 9 no Mac Os e Linux utilizando máquinas virtuais
meta_description: Saiba como testar no Internet explorer 6, 7, 8 e 9 no Mac Os e Linux utilizando máquinas virtuais.
meta_keywords: Internet explorer, IE, 6, 7, 8 e 9, Microsft, VirtualBox, front-end
category: Front-end
tags: [Front-end, IE]
---

A Microsft disponibiliza vários [VHDs com Windows](http://www.microsoft.com/download/en/details.aspx?id=11575) para os desenvolvedores poderem testar suas aplicações nos internet explorer's 6, 7, 8 e 9 no entanto estes VHDs foram feitos para ser usado no Microsoft's VirtualPC e fazer ele funcionar fora deste ambiente pode ser bastante irritante. No entanto para facilitar a nossa vida [Greg Thornton](https://github.com/xdissent) criou um [script shell](https://github.com/xdissent/ievms/blob/master/ievms.sh) que resolve tudo para nós.

## Requisitos

- [VirtualBox](http://virtualbox.org)
- Curl (Ubuntu: <code>sudo apt-get install curl</code>)
- No Linux apenas: unar (Ubuntu: <code>sudo apt-get install unar</code>)
- Paciência

## Instalação

1. [Instalar o VirtualBox](https://www.virtualbox.org/wiki/Downloads)
2. Baixar e descompactar o as maquinas virtuais
    - Instala o IE versões 6, 7, 8 e 9.
          $ curl -s https://raw.github.com/xdissent/ievms/master/ievms.sh | bash
    - Instala versões especificas do IE (IE7 e IE9 por exemplo): 
          $ curl -s https://raw.github.com/xdissent/ievms/master/ievms.sh | IEVMS_VERSIONS="7 9" bash
3. Inicie a máquina virtual
4. Acesse a máquina virtual da versão do internet explorer que quiser utilizar

A instalação pode demorar muito dependendo de sua conexão, tenha isto em mente.

Uma vez instalada a **senha de cada uma das máquinas virtuais é "Password1"**.

Mais informações podem ser encontradas na [página do ievms no github](https://github.com/xdissent/ievms).


