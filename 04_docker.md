# Docker/Podman: O mundo dos containers GNU/Linux

> Você sabe o que é um container? - Nunca vi nem comi, eu só ouço faleiner

# Idade da pedra:

Nosso querido sistema operacional GNU/Linux sempre foi o queridinho para servidores. Ele pode ser pequeno, pode ter estabilidade, pode ser um servidor, um desktop, ou o motor de um SO de celular.

No mundo antigo, administradores de sistemas e programadores precisavam configurar as máquinas para cada aplicação. Sua aplicação é feita em Java? Então você instalava o Java em uma distribuição do GNU/Linux (a partir de agora vou chamar só de Linux para simplificar).

O programa nessa máquina precisava do Java 8 para rodar, então você baixava e instalava ele. Mas de repente você precisava rodar outro programa, esse em Java 11, então você instalava o Java 11. Só que agora, quando você tentava rodar a aplicação Java 8, ela automaticamente tentava usar o Java 11 porque é o padrão da máquina, mas a aplicação não roda em Java 11, então você tenta separar nas configurações do projeto e usar um Java para cada. Esse programinha por acaso criava sempre um diretório de logs no diretório `/home/programa/logs`, SÓ QUE O OUTRO TAMBÉM FAZ ISSO! Agora os 2 programas estão escrevendo no mesmo lugar, tá tudo misturado, tá uma bagunça. Então você, como um bom administrador, decidia que na verdade isso tava tão complicado que era melhor instalar um programa em uma máquina e o outro programa em outra máquina, assim nenhum mexe com o outro. AGORA IMAGINEM TER QUE ARRUMAR 20 MÁQUINAS, UMA PRA CADA PROGRAMA ☠️

Claro que o pessoal usava máquinas virtuais, então não era tão complicado como comprar uma máquina nova. Normalmente o pessoal comprava apenas uma máquina e criava várias máquinas virtuais dentro dela, uma para cada programa. Ainda assim era complicado gerenciar porque eram muitas máquinas, tinha que ficar decidindo em qual botar cada programa, quais podiam juntar programas para economizar recursos... era doideira.

Isso também atrapalhava a vida dos desenvolvedores, porque para desenvolver uma aplicação na sua máquina com banco de dados você precisava ficar instalando o banco no seu PC. Às vezes um programa usava uma versão do banco e outro usava outra, às vezes você tinha que ficar trocando a versão do compilador para cada programa que ia compilar, aí voltava o problema de antes hahaha.

# Containers: Meu Docker, minha vida

Em março de 2013, uma empresa chamada dotCloud apresentou pela primeira vez uma ferramenta chamada Docker. Essa empresa hoje se chama Docker Inc.

Docker não é emulação, Docker não é uma máquina virtual.

O que o Docker fazia e ainda faz era isolar aplicações com um mini sistema operacional Linux "falso", então não tinha problema de conflitos nem nada. Cada aplicação pensa que está sozinha na máquina. Dentro do seu container ela pode ter a versão que quiser do Java instalado, tudo em um ambiente isolado. O programa não sabe que está rodando dentro de um container; para o programa ele está rodando normalmente em uma máquina Linux. Mas quando o programa precisa de alguma biblioteca, precisa criar algum arquivo, ele não vai criar no sistema do hospedeiro, ele vai criar no seu mini sistema operacional, seu container, seu mundinho.

![Comparação Docker vs VM](https://test.k21academy.com/wp-content/uploads/2020/11/Docker-and-Vm-blog-image_result-1.webp)

Isso é bem mais leve que uma máquina virtual. Não é realmente uma máquina instalada no seu computador, é apenas o seu programa rodando no seu sistema operacional real (host), mas com as bibliotecas já embutidas no sistema de arquivos dele para não afetar seu sistema real.

Esse mini mundinho é definido em uma `Imagem Docker`, um arquivo que diz ao Docker como criar esse mundo falso hahaha. A imagem é estática/read-only (imutável): você define essa imagem e inicia um container usando ela. Você pode iniciar quantas quiser, elas não vão entrar em conflito nunca, pois cada execução usa a imagem para criar seu próprio container. Se você apagar aquele container, tudo dentro dele é apagado. Instalar banco de dados na máquina é muito chato hahaha, requer um monte de coisa instalada no seu PC, e ainda tem o problema de um dia você ter que instalar outro banco de dados que precisa das coisas instaladas no seu PC em outra versão. Docker simplifica isso porque você pode instalar o container em outra máquina, e quando não quiser mais pode apenas apagar aquele container com um comando (muito bom para testar coisas e descartar).

# O mundo moderno

Hoje Docker é padrão de mercado. Existem até alternativas, protocolos, gerenciadores. Se você for rodar sua aplicação em alguma máquina, quase certeza que seu time vai exigir que seu programa tenha uma imagem Docker, assim eles criam um container no servidor usando essa imagem, no lugar de instalar seu programa no baremetal (diretamente na máquina).

---
**Tarefa que não é tarefa, mas é tarefa**
---

Para desenvolver nossa aplicação nós vamos precisar de um banco de dados. Eu não quero que vocês precisem instalar um banco totalmente no computador de vocês, já que vamos apagar o banco várias vezes durante o desenvolvimento, então vocês vão estudar sobre Docker e tentar rodar um banco com Docker. Quero que rodem o MySQL, esse vai ser nosso banco, mas vocês podem usar PostgreSQL também como alternativa.

---

Assistam essa série aqui:
https://youtu.be/Wm99C_f7Kxw?si=UW5Zh9DKVJOQIFx5

Se acharem alguma melhor no YouTube também serve hahahas, provavelmente qualquer opção vai explicar o básico do Docker o suficiente.