# Nosso ORM

Go tem vários ORMs 

Os mais famosos são o Ent. (do Facebook), e o GORM (Existe a anos e é feito pela comunidade)

Não tenho dados para provar mas acho que o GORM é o mais famoso devido a idade, o Gorm tem alguns problemas conhecidos como ser muito baseado em `reflection`

> In computer science, reflective programming or reflection is the ability of a process to examine, introspect, and modify its own structure and behavior.

* Basicamente o compilador adiciona dados do código fonte no binário, que estarão disponíveis em tempo de execução, isso é algo até que bem novo para linguagens de programação.
* Tem uma galera que odeia reflections porque ele não tem segurança de tipo, deixa o código mais complicado de manter, e é lentinho, já já vocês vão ver um exemplo de reflection, se não me engano Ent. não usa reflection.

### Setup

Tentem instalar o GORM sozinho no projeto de vocês:
https://gorm.io/docs/index.html 

Tentem conectar ao banco que vocês estão rodando no docker:
https://gorm.io/docs/connecting_to_the_database.html

# Structs para modelo, reflection

### Structs

Uma struct em Go é uma forma de criar um tipo com um grupo de dados

Exemplo: 
```Go
// ./models/user.go
package models 

type User struct {
  ID           uint          
  Name         string        
  Email        string       
}
```

Esse é um tipo que criamos chamado ``User`` (Usuário), ele tem um ID que é do tipo `uint` (lembrem que uint são números não assinados, começam do zero), o tipo usuário também tem um `Name` (Nome) que é uma string, e um `Email` que também é uma string.

Isso é apenas um tipo, `User` não tem valor nenhum, mas vocês podem usar esse tipo para criar variáveis com esse tipo, essa variável sim vai ter valores.

```Go
func main() {
	usuario1 := models.User {
		ID: 1,
		Name: "Heron",
		Email: "heronzinho.bonitinho@gmail.com"
	}
	fmt.Println(usuario1.Name) //Printou a string do Nome do usuario1 ("Heron")
}
```

Caso vocês esqueçam de definir um valor para algum dos tipos do User, o dado vai receber seu valor padrão (já falei sobre isso antes eu acho ahsuhau) 

Vocês podem até criar um variável com um tipo e não inicializar nenhum valor, e inicializar depois (ou não)
```Go
	var usuario2 models.User  
	// ID: 0
	// Name: ""
	// Email: ""

// Vocês podem modificar esses valores depois
	usuario2.ID: 2
	usuario2.Name = "Guigui"
	// Não inicializei o Email lálálá 🐶  
```

### Reflection
Pois é, eu falei que a struct não tem valor nenhum, é só um tipo... Mas o nosso querido amigo GORM utiliza esse tipo sem valor para criar as tabelas no banco

https://gorm.io/docs/models.html

Ele analisa o código na hora que roda, pega o tipo de cada variável do modelo via reflection e cria os dados no banco baseado nisso, pega até mesmo o nome da estrutura no código fonte

```Go
// O GORM vai ler o tipo dessa estrutura, e automaticamente converter pra lowercase (minusculo) e adicionar um "s" no final para colocar no plural

// No banco essa vai ser a tabela `users`
type User struct {
  ID           uint           // O Gorm pega com reflexão que o nome 
//do tipo da estrutura é `ID` e automaticamente coloca isso como o primaryKey

  Name         string         // Campo "name" do tipo TEXT
  Email        string         // Campo "email" do tipo TEXT
}
``` 

### Migração do ORM

```Go
db.AutoMigrate(&models.User{}) 
``` 

Isso vai migrar o modelo que vocês criaram para o banco de vocês, então cada tabela que passarem aí dentro vai ser criada no banco, adicionem essa linha aí depois de conectar ao banco

Abram o banco, atualizem aí, e olhem se essa tabela existe agora


##### Segredo secreto
O GORM já vem com um modelo especial na biblioteca

`gorm.Model:`
```Go
// gorm.Model definition
type Model struct {
  ID        uint           `gorm:"primaryKey"`
  CreatedAt time.Time
  UpdatedAt time.Time
  DeletedAt gorm.DeletedAt `gorm:"index"`
}
``` 
https://gorm.io/docs/models.html#gorm-Model

Você consegue colocar ele Embed na sua struct, o GORM vai ler isso com reflections e adicionar todos os campos da estrutura dele na sua estrutura
```Go
// ./models/user.go
package models 

type User struct {
  ID           uint          
  Name         string        
  Email        string    
  gorm.Model // É um tipo adicionado na struct só que sem nome   
}
```

gorm.Model por baixo dos panos: 
```Go
type Model struct {
  ID        uint           `gorm:"primaryKey"`
  CreatedAt time.Time
  UpdatedAt time.Time
  DeletedAt gorm.DeletedAt `gorm:"index"` // Isso é um paranoid (depois me perguntem pra que serve)
} // Todos esses campos vão ser adicionados na sua struct (boas práticas)
```

Quando vocês usarem o GORM para queries ele vai usar esses dados automaticamente, então se atualizarem algo com GORM ele vai automaticamente mudar o `updated_at` no banco (boas práticas).

# Pois é

GORM deixa muito simples o código, mas em troca as coisas ficam um pouco mais lentas por causa de reflection, e algumas coisas são bem estranhas porque o GORM faz tudo só coletando os tipos com reflection hauhau

Aaaah, vocês viram que o modelo (struct) foi escrito em Go e não em SQL? Mas mesmo assim ele criou no banco as entidades, baseadas no modelo e tipos do Go

ENTÃO, `ORM` signifca `Object-relational mapping` (Mapeamento Objeto-Relacional) 

`Object` (Objeto) Refere-se aos tipos das linguagens de programação, structs no Go, classes no Java...

`Relational` (Relacional): Refere-se aos bancos de dados relacionais (como MySQL, PostgreSQL, SQL Server) que organizam dados em tabelas com relacionamentos entre elas.

`Mapping` (Mapeamento): É o processo de "tradução" ou "ponte" entre esses dois mundos diferentes, de Go pra SQL nesse caso

GORM significa `Go ORM`