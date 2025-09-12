# Baixar o Go

1. Instalar o compilador da linguagem Go: 
https://go.dev/dl/

## Notas do Heron:

### Compilação:
Go (ou Golang) é uma linguagem que foi concebida como uma linguagem compilada, então oque você está baixando é o compilador da linguagem, compilador são programas que convertem código fonte (que humanos conseguem ler) para código de maquina, gerando um binario para o sistema operacional (ELF no linux), (Exe no windows), como você precisa compilar antes de rodar Go é uma linguagem compilada AOT (ahead of time)

O compilador na verdade faz várias coisinhas antes de chegar no executavel

Etapas de um compilador:

    Preprocessing
    Compiling
    Assembling
    Linking

Golang abstrai a maioria dessas coisas, então você só vai interagir com o compilador com comandos simples como Go build, go Run a maioria do tempo, outro exemplos de linguagem compiladas: C (pai das linguagens modernas), C++, Rust

---

### Interpretação
As linguagens que vocês usaram anteriormente (python, javascript) são linguagens que são comumente usadas com um interpretador, o interpretador é outro programinha, só que no lugar dele converter o código pra linguagem de máquina ele vai converter para uma linguagem intermediaria e as vezes converter para codigo de maquina também (compilando conforme o intepretador vê a necessidade o nome dessa paradinha é JIT (just in time compilation))

---

Muita gente fala que linguagem A ou B são interpretadas ou compiladas, mas isso na real depende muito, a linguagem define uma especificao, e normalmente essas especificacoes conseguem ser implementadas tanto com interpretadores, quanto com compiladores, Python pode ser compilada, python vem com um interpretador, mas nada proibe você de usar um interpretador com 

---

# Criando um modulo Go
1. Deem uma olhada nisso aqui:
https://micilini.com/conteudos/golang/modulos-em-go

Essa aqui é a pagina oficial do Go, muita coisas vocês podem aprende por aqui, não precisa abrir eu acho haushhas, mas é bom que conheçam esse site:
https://go.dev/doc/tutorial/create-module

2. Eu quero que o modulo de vocês sejam algo como `github.com/seu_nome/go-back`

# Instalando bibliotecas:

Vocês vão trabalhar muito com bibliotecas (principalmente se usarem javascript ou python hauhau), bibliotecas são código compartilhado, vocês não vão reinventar a roda toda vez que forem criar um serviço novo, então usem bibliotecas hasuhaus

1. instalem isso aqui https://github.com/gin-gonic/gin

Se acostumem a ler documentação ahushau

# Criando um backend simples 

na documentação vocês vão conseguir esse codigo simples aqui:

```
package main

import (
  "net/http"

  "github.com/gin-gonic/gin"
)

func main() {
  r := gin.Default()
  r.GET("/ping", func(c *gin.Context) {
    c.JSON(http.StatusOK, gin.H{
      "message": "pong",
    })
  })
  r.Run() // listen and serve on 0.0.0.0:8080 (for windows "localhost:8080")
}
```

Reparem que no começo vocês importaram um dos pacotes que pedi para instalarem, esse é o primeiro serviços de vocês usando a biblioteca Gin como framework de API