---
date: 2021-06-14
linkTitle: Básico
summary: Os conceitos-chave que vão fundamentar sua aprendizagem e seu uso da língua
title: A base da linguagem R
type: book
weight: 10
draft: true
---

<!-- Precisamos dar um bom nome para a introdução -->

## Fundamentos

### A interface do R e do RStudio
O RStudio oferece uma maneira de falar com seu computador. O R te fornece um idioma para falar. 
Para começar, abra o RStudio da mesma forma que você abriria qualquer outro aplicativo em seu computador.

{{<figure src=" ">}}

Você digita o código R na linha superior do painel do console RStudio e, em seguida, clica em Enter para executá-lo. O código que você digita é chamado de comando, porque ele comandará seu computador para fazer algo por você. A linha em que você digita é chamada de linha de comando.

A interface do RStudio é simples. Para  criar um script novo é necessário ir em `File > New File > R` script no menu no canto esquerdo.É recomendado fortemente que você escreva e edite todo o seu código R em um script antes de executá-lo no console. Por quê? Esse hábito cria um registro reproduzível de seu trabalho. Ao terminar o dia, você pode salvar seu script e usá-lo para executar novamente toda a sua análise no dia seguinte, além disso os scripts são muito úteis para editar e revisar seu código e são uma ótima cópia de seu trabalho para compartilhar com outras pessoas. Para salvar é só clicar no disquete no painel do script e depois ir em `File > Save`. 

Quando você digita um comando no script e pressiona Ctrl + Enter ou Run, o computador executa o comando e mostra os resultados no console logo abaixo. Por exemplo, se você digitar 1 + 1 e pressionar Ctrl + Enter, o RStudio exibirá:

```r
1 + 1
```

```
## [1] 2
```

#### Exercício

### Objetos

Agora que você já sabe como o R funciona, vamos ver alguns operadores e objetos que podem ser criados. Se você quer que o 
R crie um vetor, use o operador `:`, esse operador vai retornar um conjunto unidimensional de números:


```r
1:6
```

```
## [1] 1 2 3 4 5 6
```
Mas quando você roda assim dessa forma, o R gera o vetor que você poderá ver o resultado no console, porém esse vetor não vai ficar salvo em lugar nenhum, é basicamente um pegada de seis números que existiram brevemente. Se você quiser usar novamente essa sequência de número, você precisa pedir para o computador guardar ele em algum lugar. Você pode fazer isso criando um `objeto`.

R permite salvar dados armazenando-os dentro de um objeto R. O que é um objeto? Apenas um nome que você pode usar para acessar os dados armazenados. Por exemplo, você pode salvar dados em um objeto como `a` ou `b` ou qualquer nome que faça sentido para o que você está fazendo. Sempre que R encontrar o objeto, ele irá substituí-lo pelos dados salvos nele, da seguinte forma:


```r
a <- 1

a
```

```
## [1] 1
```

```r
a + 2
```

```
## [1] 3
```

Ou seja, para você criar um objeto no R, você escolhe um nome e depois usa o símbolo `<-` para salvar o dado naquele objeto no qual você deu um nome. No caso do exemplo acima, o R criou um objeto, deu a ele seu nome e armazenou nele tudo o que vier após a seta. Portanto, `a <- 1` armazena 1 em um objeto denominado a.

Quando você pergunta ao R o que está em um objeto, R diz a você na próxima linha.
Você também pode usar seu objeto em novos comandos R. Já que armazenou anteriormente o valor de 1, agora você está adicionando 1 a 2
`a + 1`

Então, como você faria para armazenar esse vetor de seis números `1:6`, que a gente criou anteriormente, em um objeto?
#### Exercício

Quando você cria um objeto no R, esse objeto vai aparecer armazenado na sessão `Environment` no lado direito da sessão script.


### Funções


#### Exercício

### Sua primeira função

#### Exercício

### Argumentos

#### Exercício

### Programas (scripts)

#### Exercício

### Pacotes

#### Exercício

### Ajuda

#### No R

#### Online

#### Exercício

## Objetos em R

### Vetores

#### Exercício

### Atributos

#### Exercício

### Matrizes

#### Exercício

### Arrays

#### Exercício

### Classe

#### Exercício

### Coerção

#### Exercício

### Listas

#### Exercício

### Data Frames

#### Exercício

### Fórmulas

#### Exerício

## Revisão

### Exercícios
