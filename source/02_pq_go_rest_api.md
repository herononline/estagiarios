# Porque Go
> pq eu gosto

mentira é isso não, na verdade eu acho que Go é uma boa linguagem pra iniciantes porque ela é muito simples, tem uma boa biblioteca padrão (não precisa instalar muita coisa), e também ele expõe algumas coisas que outras linguagens de programação vão esconder pra vocês ou até deixar de forma opcional

Go usa um coletor de lixo (garbage collector) https://pt.wikipedia.org/wiki/Coletor_de_lixo_(inform%C3%A1tica)

então vocês não vão precisar tanto com gerenciamento de memoria manual, isso é bom porque outras linguagens com C e C++ requerem esse gerenciamento de memoria manual, então se vocês alocarem algo na heap precisariam desalocar, no Go isso é apenas coletado pelo coletor de lixo, e vocês ainda conseguem trabalhar para manter coisas na stack, então dá pra garantir um desempenho legal com Go, ela é uma das linguagens "novas" (acho que é de 2009 ou 2010 ahsudha) famosinhas para backend e serviços da web/cloud, já que ela nasceu pra resolver varios problemas que a Google enfrentava na epoca, então Go é gostosinho de programar

* me avisem se voces nao souberem oque é stack ou heap, mas isso nao é uma coisa que vocês precisam saber muito no mundo moderno (mas bons programadores sabem oque é isso viu viu viu viu 👀), ainda mais se estiver com linguagens como python e javascript hauhauh)

# O programinha que criamos

> antes só queria deixar claro que go é uma linguagem com uma biblioteca padrão muito rica, então dá pra fazer muita coisa sem instalar nada de fora, inclusive vocês poderiam fazer esse serviço web usando apenas a biblioteca padrão com o `net/http` do Go, essas bibliotecas que vamos usar apenas facilitam isso, porque o net/http segue a ideia de ser algo muito simples, então para um serviço da web ela poderia exigir muito codigo que essas bibliotecas já implementam (inclusive o Gin usa net/http por baixo dos panos )

vou seguir o codigo com vocês comentando cada parte:
```Go
1   package main
2   
3   import (
4     "net/http"
5   
6     "github.com/gin-gonic/gin"
7   )
8   
9   func main() {
10    r := gin.Default()
11    r.GET("/ping", func(c *gin.Context) {
12      c.JSON(http.StatusOK, gin.H{
13        "message": "pong",
14      })
15    })
16    r.Run() // listen and serve on 0.0.0.0:8080 (for windows "localhost:8080")
17  }
```

```
1   package main
```

esse é o ponto de entrada do seu programa Go, praticamente todo programa Go tem um package main e uma função main(), aqui é o entrypoint para o programa que vai ser compilado

```Go
3   import (
4     "net/http"
5   
6     "github.com/gin-gonic/gin"
7   )
``` 
essa aqui são as importações que fazemos para esse programa, se vocês leram a paradinha que falei dos modulos go vocês vão reparar que `github.com/gin-gonic/gin` é um modulo externo, Go é diferente do Nodejs, que tem um registry padrão, no go você pode hospedar seu codigo onde quiser, go vai fazer um fetch nas dependencias e adicionar ao seu projeto, isso é muito bom, vez ou outra vocês vou ouvir que o NPM foi hackeado, ou tá fora de ar, ou que tal biblioteca infectada afetou milhões de projetos hsauhau, no Go você tem controle total de onde vai botar seu codigo para outras pessoas usarem, você pode até mesmo deixar ele privado, apagar a biblioteca pra sempre (no NPM existem regras pra isso), enfim, você tem muito controle, então sempre que vocês verem um modulo que é um link, significa que é uma dependencia externa e o caminho é onde o Go vai baixar o codigo, vocês podem até mesmo abrir o link e conferir todo o codigo da biblioteca `github.com/gin-gonic/gin`

o `net/http` não possui um link de registry porque ele faz parte da biblioteca padrão do Go, ela é instalada na maquina de vocês junto com a instalação do Compilador, então ela tá disponivel pra todo mundo que programa em Go :)

```Go
9   func main() {
```

`func main()` define a função main, o entrypoint para o código do seu programa

```Go
10    r := gin.Default()
11    r.GET("/ping", func(c *gin.Context) {
12      c.JSON(http.StatusOK, gin.H{
13        "message": "pong",
14      })
15    })
16    r.Run() // listen and serve on 0.0.0.0:8080 (for windows "localhost:8080")
17  }
```

`10    r := gin.Default()` essa é a syntax do go para `declaração` + `definição`, normalmente vocês vão ver linguagens fazendo algo como `var/const/let/type = valor`, se vocês não sabem a diferença de declaração para definição me perguntem aí que eu explico, (eu realmente ná̃o sei como foram as aulas de vocês hfaudshfua)

enfim, nessa função basicamente cria e retorna uma instancia do Gin (framework que estamos usando), se vocês passarem o nome na função (`.Default()`), vocês conseguem ver a documentação dela, vocês também pode clicar nela com o lado direito e pular para definição, com o codigo fonte da biblioteca com o texto da documentação explicando a função:

```Go
// Default returns an Engine instance with the Logger and Recovery middleware already attached.
func Default(opts ...OptionFunc) *Engine {
	debugPrintWARNINGDefault()
	engine := New()
	engine.Use(Logger(), Recovery())
	return engine.With(opts...)
}
``` 

só mostrei isso só porque essa uma das filosofias do Go, o código das bibliotecas está disponivel pra você, então se você usa uma biblioteca, você tem todo o código dela, e não apenas cabeçalhos e definições assim com Javascript ou C.

```Go
11    r.GET("/ping", func(c *gin.Context) {
12      c.JSON(http.StatusOK, gin.H{
13        "message": "pong",
14      })
15    })
```

aqui chamamos a nossa engine que colocamos o nome de `r`, que é do tipo `*gin.Engine`, já que ela é uma engine, podemos usar um dos metodos de uma `gin.Engine`: `GET`

> O metodo GET é um metodo do protocolo HTTP, se não me engano ele foi definido no HTTP 1.0, então pode ter certeza que esse metodo vai ser suportado por quase tudo huahu

O metodo GET do Gin pede como primeiro argumento um caminho para  rota, no caso colocamos o caminho como '`/ping`', quando nossa aplicação estiver rodando, toda vez que ela receber um GET para /ping, ele vai executar o segundo argumento, que é uma `gin.HandlerFunc`, vocês podem olhar no código do Gin oque é uma HandleFunc, basicamente é uma função com essa syntax aqui: `HandlerFunc func(c *gin.Context)`, então criamos uma função anonima (função sem nome dentro de outra função) com essa syntax, exatamente igual, eu sempre achei a syntax de passar uma função como argumento estranho huahua, ainda mais nesse jeito anonimo que não passamos nome nem nada hausuhas, o código fica muito estranho com várias ({}), então é normal se estranharem também, inclusive vocês poderiam criar essa função fora e só usar ela, deixando mais fácil de ler e talvez entender:

```Go 
// mesma coisa da outra mas não anonima 
func sendJSONPongHandler(c *gin.Context) {
    c.JSON(http.StatusOK, gin.H{
      "message": "pong",
    })
}

func main() {
  r := gin.Default()
  r.GET("/ping", sendJSONPongHandler) // mesma coisa
  r.Run() // listen and serve on 0.0.0.0:8080 (for windows "localhost:8080")
}
```

Vamos dar uma olhadinha nessa função haushasu, quase finalizando
```
func sendJSONPongHandler(c *gin.Context) {
    c.JSON(http.StatusOK, gin.H{
      "message": "pong",
    })
}
``` 
basicamente usamos o contexto do Gin, o contexto é uma estrutura que encapsula tudo do HTTP e da requisição, ele consegue enviar as respostas para o requisitante, consegue saber quem requisitou, consegue pegar metadados, e outras coisinhas

> essa parada de contexto se não me engano foi popularizada por um framework Javascript chamada Express, então quando temos acesso ao contexto assim no framework dizemos que ele é inspirado no Express ("Express-like") hauhau}

Então usamos a função de resposta do contexto, respondendo o request que recebemos na nossa rota '`ping`', respondemos com um JSON que é basicamente
 
```JSON
{
"message": "pong"
} 
``` 

> se vocês não souberem oque é um JSON podem perguntar também 

no final metemos um 

```Go
16    r.Run() // listen and serve on 0.0.0.0:8080 (for windows "localhost:8080")
```

que é basicamente pegar a engine que inicializamos e mandar ela ficar rodando, o Gin por padrão usa a porta `:8080` na rede

> eu esqueci de falar, mas isso aqui '`//`' em Go é um comentario, tudo pra frente disso não entra no programa compilado e é totalmente ignorado, mas no Go você pode usar isso pra documentar o código, é importante saber usar os comentários de forma inteligente, comentar tudo é loucura também, com o tempo vocês vão construindo intuição pra isso, mas tem umas regrinhas de ouro hasuhau

isso de `porta` (port) vocês devem ter estudado em redes, mas se não estão familiarizados com isso recebam o conhecimento:

https://www.cloudflare.com/pt-br/learning/network-layer/what-is-a-computer-port/

--- 
# pronto, agora vocês já são devs backend ou fullstack

pra rodar essa parada vocês usam o compilador do go, basicamente abram um terminal e rodem `go run main.go`, ou `go run .`, o compilador é esperto e vai encontrar a main, compilar, e rodar o codigo na mesma hora, serviços web não desligam muito (se desligar ou é manutencao ou deu ruim), então vocês precisam deixar o serviço rodando para mandar requisições, essa funçãozinha Run() já vai manter ele rodando pra sempre, é basicamente um loop infinito que não para ahsuhsa (ao menos que vocês desliguem o programa com CTRL+C ou matando o processo)

mas vamos testar agora, vocês precisam de um programa para fazer requisições a internet, um programa que consiga usar o protocolo HTTP pra isso, existem muitos, de terminal tem o `wget` ou `curl` (curl é super famoso e carrega nas costas quase tudo hasuhas), mas como vamos fazer de uma forma visual e simples, vocês já tem um programa instalado que faz requisições GET facil facil hauhau, é o navegador, então basicamente acessem o ip local e entrem na porta 8080, 

só abrir esse link aqui se o programa estiver rodando
`http://localhost:8080`

o navegador de vocês vai fazer um GET nesse ip e renderizar a resposta na tela 

### (404 not found)
enganei vocês hasdfuhasuf, (na vrdd eu sou burro), então, essa hota que eu passei pra vocês é onde a aplicação tá rodando, quando vocês tentarem acessar ela o nosso framework é esperto e não vai achar uma rota pra isso, e vai dar o 404 automaticamente, a rota que criamos nesse programa é `/ping`, então vocês precisam acessar: `http://localhost:8080/ping`

se estiver tudo certo deu boa

> essa parada de redes vocês vão estudar na faculdade também, então se vocês fizerem podem ficar tranquilos que vão estudar haushas, mas se quiserem aprender um pouco agora e por fora vou passar o basiquinho aqui do que vocês vão precisar entender 

metodos HTTP:
https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Reference/Methods

oque é uma API REST (oque começamos a fazer):
https://www.redhat.com/pt-br/topics/api/what-is-a-rest-api

livro (se forem malucos hasuhas, mas provavelmente vão ver isso na faculdade, não recomendo comprarem pq é caro ashuhas e tem mais coisa do que vocês precisam saber agora, mas caso algum dia estiverem no hype de ler um livro de programação sobre redes da grossura de um tijolo fica ai a recomendação):

https://archive.org/details/tanenbaum-rede-de-computadores-6a/mode/2up

> enfim, esse livro ai já vai do começo até o final, e um junior hoje em dia não precisa saber de tudo isso hauhsa, mas talvez vocẽs vejam esse livrinho na faculdade então joguei ele ai tambem

---

é isso ai galera, mandaram bem, deem uma lida ai no que deixei de link (menos o livro hdfuashfudha), acho que a proxima coisa que vamos fazer é o banco de dados, mas tô pensando ainda haushas, por agora fico ai pra duvidas 