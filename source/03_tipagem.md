# Tipagem 
Falando um pouco sobre tipos: por que gostamos de tipos, por que odiamos tipos, por que tipos são tipos, e qual é o meu tipo 😍

# Tipos primitivos 
>(isso serve para outras linguagens também hahaha)

Eu falei para vocês que Go é uma linguagem "estaticamente tipada". Uma linguagem estaticamente tipada significa que o tipo da variável é definido e verificado em tempo de compilação e não pode mudar em tempo de execução.

Lista dos tipos primitivos mais comuns:

| Tipo | Valor Padrão | Range | Descrição |
|------|--------------|-------|-----------|
| `bool` | `false` | `true` ou `false` | Valor booleano |
| `string` | `""` | - | String vazia |
| `int` | `0` | Depende da arquitetura (32 ou 64 bits) | Inteiro com sinal |
| `int8` | `0` | `-128` a `127` | Inteiro 8 bits com sinal |
| `int16` | `0` | `-32.768` a `32.767` | Inteiro 16 bits com sinal |
| `int32` | `0` | `-2.147.483.648` a `2.147.483.647` | Inteiro 32 bits com sinal |
| `int64` | `0` | `-9.223.372.036.854.775.808` a `9.223.372.036.854.775.807` | Inteiro 64 bits com sinal |
| `uint` | `0` | Depende da arquitetura (32 ou 64 bits) | Inteiro sem sinal |
| `uint8` | `0` | `0` a `255` | Inteiro 8 bits sem sinal |
| `uint16` | `0` | `0` a `65.535` | Inteiro 16 bits sem sinal |
| `uint32` | `0` | `0` a `4.294.967.295` | Inteiro 32 bits sem sinal |
| `uint64` | `0` | `0` a `18.446.744.073.709.551.615` | Inteiro 64 bits sem sinal |
| `byte` | `0` | `0` a `255` | Alias para uint8 |
| `float32` | `0.0` | `±1,18×10⁻³⁸` a `±3,4×10³⁸` | Ponto flutuante 32 bits |
| `float64` | `0.0` | `±2,23×10⁻³⁰⁸` a `±1,8×10³⁰⁸` | Ponto flutuante 64 bits |

> Se tiverem alguma dúvida sobre tipos inteiros sem sinal ou com sinal, tamanho de bits, range, podem perguntar para mim 

### Atribuindo tipos a uma variável em Go

```Go
var nome string = "João"
// tipo definido explicitamente
``` 

1. var: `keyword` para identificar uma variável 
2. nome: identificador
3. string: tipo da variável
4. =: operador de atribuição
5. João: valor/literal

ou 

```Go
nome := "João"
// o tipo está implícito já que ele foi atribuído a uma string
// o compilador sabe que para ser atribuído a uma string ele precisa ser do tipo `string`
```

#### Agora nosso programa sabe que o tipo de `nome` é `string`

Esse tipo não vai mudar durante toda a vida do programa. `nome` vai para sempre ser uma string. Isso não é algo ruim, isso garante que nosso programa seja mais seguro e mais fácil de ler e entender.

> Conforme os recursos de tipagem que a linguagem dá ao programador, podemos classificar a linguagem como "tipagem fraca" ou "tipagem forte", "tipagem segura", "tipagem não segura".

```Go
var nome string = "João"
nome = 1 // Não vai compilar, João é uma string
``` 

# Tipagem dinâmica 

Com a tipagem dinâmica, os tipos são definidos em tempo de execução (runtime). A variável pode mudar de tipo, e erros de tipagem só acontecem em tempo de execução. O compilador ou interpretador não vai dar erro se você tentar atribuir um tipo, porque tudo muda dinamicamente.

Isso deixa o programa muito mais simples e fácil de entender e gerenciar, mas como vocês vão aprender na vida de vocês, isso é o diabo na terra.

* JavaScript é uma linguagem de tipagem dinâmica e tipagem fraca. Você não interage com tipos normalmente, raramente você vai interagir. Isso foi feito porque JavaScript foi pensado para ser uma linguagem simples para interagir com HTML, mas o tempo passou e o mundo ficou mais e mais doente. Hoje JavaScript é usado em todo lugar, e é tão complicado desenvolver coisas complexas sem tipagem que coisas como JSDoc e TypeScript foram criadas, todas tentam "corrigir" esse problema do JavaScript.

Código JavaScript:
```JavaScript
let nome = "Arthur";
// nome é uma string (não existe forma boa de declarar o tipo manualmente, tem que confiar)

let sobrenome = "Morgan"; // sobrenome do cara é Morgan
// e provavelmente ele tem medo

nome = 1; // ops, agora nome é um número

nome = true; // na verdade nome é um booleano agora

sobrenome = 3; // o sobrenome dele agora é o número 3

let nome_completo = juntar_nome(nome, sobrenome);

console.log(nome_completo) // tentem adivinhar o que vamos imprimir no resultado dessa função

// não podemos definir os tipos com palavras-chave no JavaScript,
// então essa função pede 2 valores e eu coloquei o _string para tentar avisar o programador 
// que eu preciso de duas strings, mas essa função na verdade aceita tudo, 
// porque é JavaScript, e qualquer variável pode ter qualquer tipo a qualquer momento
function juntar_nome(nome_string, sobrenome_string) { 
  return nome_string + sobrenome_string;
}
``` 
---
**Tarefa que não é tarefa, mas é uma tarefa**

Tentem reescrever esse programa de JavaScript em Go e descubram o tanto de problema que esse código tem. Go tem tipagem estática e segura, vocês não vão conseguir compilar, mas vejam o tanto de erros que vão ter no desenvolvimento (isso é bom).
---

Tenham piedade da minha alma 💀

Esses problemas que falei no JS são reduzidos com o uso de TypeScript. Por isso a maioria dos novos projetos são feitos em TypeScript no lugar de JavaScript. JavaScript é mais fácil de aprender por não ter tipagem, mas a falta de tipagem segura acaba sendo um problema para projetos maiores e com múltiplos colaboradores (JSDoc é outra alternativa viável).

Quando vocês estiverem trabalhando ou procurando emprego, não se preocupem tanto com a linguagem do serviço. Vocês vão achar o caminho de vocês, vez ou outra vocês vão aprender uma linguagem nova, um framework novo, mas o que importa no fim é o conhecimento de vocês em resolução de problemas. Então tenham um olhar meio cético para qualquer tecnologia, não fiquem odiando linguagem A ou B, ou comparando com outras (que nem eu agora hahaha). Eu estou ensinando Go porque é o que eu sei bem e acredito que ensina os conceitos básicos para vocês. Tem muita coisa legal em JavaScript, Python, Java, o mundo é grande hahahaha. O que eu estou comparando aqui são conceitos de tipagem dinâmica e estática, e não batendo o martelo que Go é melhor que JavaScript em tudo hahaha.