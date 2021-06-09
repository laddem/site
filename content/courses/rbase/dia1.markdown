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

Agora que você já sabe como o R funciona, vamos ver alguns operadores e objetos que podem ser criados. Se você quer que o R crie um vetor, use o operador `:`, esse operador vai retornar um conjunto unidimensional de números:


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

Você também pode usar seu objeto em novos comandos R. Já que armazenou anteriormente o valor de 1, agora você está adicionando 1 ao a armazenando no objeto `a + 1`

Então, como você faria para armazenar esse vetor de seis números `1:6`, que a gente criou anteriormente, em um objeto?


```r
vetor <- 1:6

vetor
```

```
## [1] 1 2 3 4 5 6
```

#### Exercício

Quando você cria um objeto no R, esse objeto vai aparecer armazenado na seção `Environment` no lado direito da seção script.

Você pode nomear um objeto no R de praticamente qualquer nome, só tem algumas regras: não pode começar como número, e também não pode ter alguns símbolos, como por exemplo:  `^`, `!`, `$`, `@`, `+`, `-`, `/`, `%` or `*`.

Além disso, o R *case-sensitive*, portanto se eu colocar um nome de objeto como `Name` e outro como `name` eles vão se referir a objetos diferentes, ou seja, ele considera se a letra é maiuscula ou minuscula. 

E se você criar um objeto, salvar uma informação neste objeto e logo em seguida salvar outra, ele subscreve o que estava anteriormente, então tome cuidado para não perder informações. O exemplo abaixo mostra exatamente isso:


```r
meu_numero <- 1

meu_numero
```

```
## [1] 1
```

```r
meu_numero <- 999
meu_numero
```

```
## [1] 999
```

Com a função `ls()` você consegue listar no console todos os objetos criados.

O que é possível fazer com esses objetos no R que você criou? Muita coisa! Por exemplo: é possível usar o objeto vetor e fazer uma operação de divisão


```r
vetor/2
```

```
## [1] 0.5 1.0 1.5 2.0 2.5 3.0
```

```r
vetor - 1
```

```
## [1] 0 1 2 3 4 5
```

O R dividiu por 2 todos os números dentro daquele vetor. Se você subtrair 1 desse objeto, o R vai subtrair 1 de cada elemento dentro deste objeto. 

Quando você usa dois ou mais vetores em uma operação, R alinhará os vetores e executará uma sequência de operações individuais. Por exemplo, quando você executa vetor * vetor, o R alinha os dois vetores de dados e, em seguida, multiplica o primeiro elemento do vetor 1 pelo primeiro elemento do vetor 2, então multiplica o segundo elemento do vetor 1 pelo segundo elemento do vetor 2 , e assim por diante, até que cada elemento tenha sido multiplicado. O resultado será um novo vetor com o mesmo comprimento dos dois primeiros:


```r
vetor * vetor
```

```
## [1]  1  4  9 16 25 36
```

Se você der ao R uma operação com dois vetores de comprimentos diferentes, o R repetirá o vetor mais curto até que seja do mesmo tamanho do vetor maior e, em seguida, fará as contas. Esta não é uma mudança permanente - o vetor mais curto terá seu tamanho original depois que o R fizer as contas. Se o comprimento do vetor curto não se dividir igualmente no comprimento do vetor longo, o R retornará uma mensagem de aviso. Esse comportamento é conhecido como **reciclagem** de vetor e ajuda o R a fazer operações em elementos:


```r
vetor * 1:4
```

```
## Warning in vetor * 1:4: comprimento do objeto maior não é múltiplo do
## comprimento do objeto menor
```

```
## [1]  1  4  9 16  5 12
```

```r
vetor * 1:2
```

```
## [1]  1  4  3  8  5 12
```

Porém, é preciso cuidado pois se você não estava querendo **reciclar**, é possível cometer erros. No exemplo abaixo, eu crio um segundo vetor com os valores para multiplicação, mas ele tem tamanho menor que o primeiro. Por isso, é bom ficar atento aos `Warning message:`s:


```r
vetor2 <- c(1, 2, 3, 4, 5)

vetor * vetor2
```

```
## Warning in vetor * vetor2: comprimento do objeto maior não é múltiplo do
## comprimento do objeto menor
```

```
## [1]  1  4  9 16 25  6
```

### Funções

R vem com muitas funções instaladas. Praticamente tudo que você vê que não são `objetos` que você criou são funções e seus componentes. Por isso, dizemos que, no seu núcleo, o R é uma linguagem "funcional". Vejamos alguns exemplos de funções:


```r
round(3.1415)
```

```
## [1] 3
```

```r
factorial(3)
```

```
## [1] 6
```

#### Exercício

Só de olhar para as funções e seus resultados, você conseguíria dizer:

- O que cada função faz?
- Quais entradas (inputs) ela pede?
- Qual saída ela produz (output)?
- Que opções alternativas (argumentos) elas poderiam conter?

### Argumentos

As informações que são colocadas dentro dos `()` depois do nome de uma função são chamados de **argumentos**. Em geral, uma função tem alguns argumentos obrigatórios (o valor a ser arrendodado, o número para calcular o fatorial) e argumentos opcionais (arredondar para cima ou para baixo? descartar valores nulos?).


```r
mean(1:6)
```

```
## [1] 3.5
```

```r
mean(vetor)
```

```
## [1] 3.5
```

```r
vetor_com_nulo <- c(1, 2, 3, 4, 5, 6, NA)

mean(vetor_com_nulo)
```

```
## [1] NA
```

```r
mean(vetor_com_nulo, na.rm = TRUE)
```

```
## [1] 3.5
```

Argumentos obrigatórios e opcionais tornam as nossas funções mais flexíveis. Essa flexibilidade é crucial para muitas operações, pois podemos passar funções como argumentos de outras funções. Por exemplo:


```r
round(mean(vetor))
```

```
## [1] 4
```

Quando você reúne as funções dessa forma, o R vai avaliar a função de dentro para fora, como uma boneca matrioshka:


```r
resultado <- vetor
resultado
```

```
## [1] 1 2 3 4 5 6
```

```r
resultado <- mean(resultado)
resultado
```

```
## [1] 3.5
```

```r
resultado <- round(resultado)
resultado
```

```
## [1] 4
```

Outro exemplo de argumentos opcionais é a função `sample`, que produz amostras aleatórias:


```r
sample(x = vetor, size = 1)
```

```
## [1] 3
```

```r
sample(x = vetor, size = 1)
```

```
## [1] 6
```

```r
sample(x = vetor, size = 1)
```

```
## [1] 4
```

Com os argumentos opcionais, podemos mudar detalhes do processo de amostragem


```r
sample(x = vetor, size = 1, prob = c(1/8, 1/8, 1/8, 1/8, 1/8, 3/8))
```

```
## [1] 4
```

E ao mudar as probabilidades de que cada uma amostra vai ser selecionada, eu criei um dado viciado!

Você pode verificar os argumentos de uma função com:


```r
args(sample)
```

```
## function (x, size, replace = FALSE, prob = NULL) 
## NULL
```

#### Exercício

Repita a operação de `sample` acima com o dado "viciado" e com o dado "limpo" e verifique se você consegue perceber empiricamente que meu dado está "viciado".

Que outros argumentos existem na função `sample`? Qual o papel do argumento `replace`?

### Sua primeira função

Ok, mas suponha que você esteja cansado de repetir as coisas no seu programa. Existe uma operação que você realiza de novo e novo, e ela é relativamente simples, mas é um tédio ficar copiando e cola a mesma coisa. Por exemplo, se você tem o vetor que criamos com seis números, e precisa sortear dois valores e somar seu resultado, como se estivesse jogando dois dados de 6 faces e somando os resultados?


```r
x <- sample(vetor, size = 1)
y <- sample(vetor, size = 1)
x + y
```

```
## [1] 4
```

Seu código funciona bem, mas você quer simplificar isso para uma função que faça tudo de uma vez, podemos chamar ela de `role`, como se fosse "role dois dados".


```r
role()
```

```
## [1] 3
```

```r
role()
```

```
## [1] 8
```

```r
role()
```

```
## [1] 9
```

A função `role` não existe no R base, mas você mesmo pode construí-la:


```r
role <- function() {
  x <- sample(vetor, size = 1)
  y <- sample(vetor, size = 1)
  x + y
}

role()
```

```
## [1] 7
```

Pause para contemplar os diferentes elementos do construtor de funções do R:

- `role` é o nome da função, e ele deve ser atribuído `<-` para que você possa chamar sua função
- `function` é uma função que constrói funções, e os argumentos dela que vão nos parenteses são aqueles que o usuário deverá digitar. Nesse caso, nenhum argumento é necessário.
- Os `{}` indicam o início e o fim do `corpo` da função, onde a magia acontece.
- Ao rodar o construtor da função, nada acontece. A função só entra em funcionamento na hora que o usuário a utiliza posteriormente `role()`.

#### Exercício

Escreva uma função que role 2 dados de 10 faces e some seus resultados.

### Programas (scripts)

<!-- 

N: Eu coloquei o script direto lá em cima, pra mim não faz muito sentido passar pelo console primeiro, mas não sei, o que você acha?

V: Olha, pra mim depende muito do aluno. Acho que as vezes gente que começa a estudar o R nunca para pra pensar na distinção entre as duas coisas. Pessoalmente acho útil porque eu uso o console o tempo todo para testar coisas que eu não quero que poluam o script, mas muita gente faz isso no script e depois apaga. Meu ponto aqui é outra coisa, é só introduzir pra eles o conceito de programa/script. O que é, o que tem num programa, talvez um ou outro exemplo de um programa. Tipo um programa de importação e tratamento de dados. Um programa que produz uma visualização. Um programa que gera um modelo estatístico. Um programa que faz uma tabela, etc.

-->

Na maioria das situações, esses conceitos soltos que introduzimos não são muito úteis isoladamente. Afinal de contas, objetos, funções e números individuais não servem para muita coisa. Esses conceitos ganham corpo quando os utilizamos juntos para produzir nossos programas. O que são programas? A metáfora mais comumemente utilizada é a culinária. Um programa é uma sequência de instruções, uma receita para produzir alguma coisa. A diferença é que ao invés de produtos culinários, os ingredientes são informações na memória de uma computador, e ao invés de um prato, estamos tentando produzir resultados que podem ser analisados: estatísticas descritivas, representações gráficas, modelos matemáticos, tabelas, etc.

Falaremos mais sobre programas e estratégias de construção de programas (se der tempo), em outra aula. Por aqui, seria interessante que vocês levassem adiante a noção de que o programa é o conjunto da obra. É comum desenvolvermos ou encontrarmos programas para:

- importar a corrigir quaisquer problemas nos meus dados
- produzir estatísticas descritivas a partir de meus dados
- produzir diversos gráficos que descrevem meus dados
- produzir tabulações e exportá-las para outros softwares

Como vários aspectos do R, a flexibilidade aqui é imensa, e vocês são livres para definir o **escopo** dos seus programas. Você vai preferir colocar tudo num programa só, ou talvez dividí-lo em diversos programas e tarefas menores? Cada abordagem carrega consigo vantagens e desvantagens que vocês terão que decidir se valem ou não a pena.

### Pacotes

Você não é a única pessoa que escreve suas próprias funções com R. Muitos professores, programadores e estatísticos usam R para projetar ferramentas que podem ajudar as pessoas a analisar dados. Eles então tornam essas ferramentas gratuitas para qualquer pessoa usar. Para usar essas ferramentas, basta baixá-las. Eles reúnem coleções pré-montadas de funções e objetos chamados pacotes. Veremos o básico aqui.

<!-- Acho melhor evitar qplot. Vamos usar as funções do graphics mesmo para fazer uns gráficos simples. Pode deixar o exemplo de baixar o ggplot2, sem problema, mas é melhor a gente produzir os gráficos no base mesmo para o pessoal ter uma ideia quando eles encontrarem base graphics por aí. -->

Vamos usar a função qplot para fazer alguns gráficos rápidos. qplot vem no pacote ggplot2, o pacote popular para fazer gráficos. Antes de usar o qplot, ou qualquer outra coisa no pacote ggplot2, você precisa fazer o download e instalá-lo.

Os pacotes do R em geral estão hospedados em http://cran.r-project.org, o mesmo site de onde você baixou sua versão do R. No entanto, você não precisa visitar o site para baixar um pacote R; você pode baixar pacotes direto da linha de comando do R. 


```r
install.packages('ggplot2')
```

É isso. O R fará com que seu computador visite o site, baixe ggplot2 e instale o pacote em seu disco rígido exatamente onde o R deseja encontrá-lo. Agora você tem o pacote ggplot2. Se você gostaria de instalar outro pacote, substitua ggplot2 pelo nome do seu pacote no código.

### Ajuda

#### No R

Existem mais de 1.000 funções no núcleo do R e novas funções são criadas o tempo todo. Isso pode ser muito material para memorizar e aprender! Felizmente, cada função R vem com sua própria página de ajuda, que você pode acessar digitando o nome da função após um ponto de interrogação. Por exemplo, cada um desses comandos abrirá uma página de ajuda. Procure as páginas que aparecem na guia Ajuda do painel inferior direito do RStudio:


```r
?sqrt
?log10
?sample
```

As páginas de ajuda contem informações úteis sobre o que cada função faz. Essas páginas de ajuda também servem como documentação de código, portanto, pode ser algo um pouco chato. Muitas vezes parecem ter sido escritas para pessoas que já entendem a função e não precisam de ajuda. Não deixe que isso te faça desistir de entender uma função que você queira usar - você pode ganhar muito com uma página de ajuda examinando-a em busca de informações que façam sentido e ignorando o resto. Essa técnica inevitavelmente o levará à parte mais útil de cada página de ajuda: a parte inferior. Aqui, quase todas as páginas de ajuda incluem algum código de exemplo que coloca a função em ação. Executar esse código é uma ótima maneira de aprender com o exemplo dado.

Se você gostaria de consultar a página de ajuda de uma função, mas esqueceu o nome da função, você pode pesquisar por palavra-chave. Para fazer isso, digite dois pontos de interrogação seguidos por uma palavra-chave na linha de comando de R. R exibirá uma lista de links para páginas de ajuda relacionadas à palavra-chave. Você pode pensar nisso como a página de ajuda para a página de ajuda:


```r
??log
```

Muitos pacotes também incluem **vinhetas**, que são pequenas aulinhas que resumem as principais funções de um pacote através de explicações de uso detalhados. Você pode ver as vinhetas disponíveis num pacote instalado assim:


```r
vignette(package = "ggplot2")
```

E a vinheta em si é acessada assim:


```r
vignette("ggplot2-specs", package = "ggplot2")
```

Que deve abrir a vinheta na sua seção "Help" do RStudio. A maioria dessas vinhetas também está disponível online numa consulta rápida ao Google.

#### Online

Em geral, após uma consulta a página de ajuda, pode ser que você não esteja satisfeito. Você pode complementar sua página de ajuda com diversos recursos online. Vamos deixar alguns links abaixo que utilizamos cotidianamente.

- [Google](https://www.google.com): em geral, uma pesquisa com "r <função>" ou "r \<pacote\>" te leva para onde você quer ir.
- [Stack Overflow](https://stackoverflow.com): similar, mas no stackoverflow se usam `[tags]`, então seria algo como `[r][pacote] sua pergunta`
- [RStudio Community](https://community.rstudio.com): mais pra perguntas relacionadas ao RStudio
- [Lista R-Br](http://r-br.2285057.n4.nabble.com/): lista ativa e em português, em geral, o pessoal é prestativo.
- [RStudio Tips - Twitter](https://twitter.com/rstudiotips): dicas no twitter para ir melhorando no cotidiano.

#### Exercício

Consulte a ajuda das funções `sum`, `mean`, `min`, `max`, `range`. Porque todas elas tem o argumento `na.rm`? O que argumento o `trim` em `mean` faz? Qual a melhor maneira de rapidamente entender o que uma função faz através da página de ajuda?

## Objetos em R

### Vetores

Já trabalhamos com alguns vetores lá em cima, é inevitável. Pensou em salvar um objeto na memória do computador no R, pensou em vetor. Pra ser preciso, estamos usando **vetores atômicos**. Os vetores atômicos são em geral o objeto mais frequentemente usado em R. Para construir um, você utiliza a função `c`:


```r
vetor <- c(1, 2, 3, 4, 5, 6)
vetor
```

```
## [1] 1 2 3 4 5 6
```

E você pode verificar se ele é um vetor mesmo:


```r
is.vector(vetor)
```

```
## [1] TRUE
```

`is.vector` testa se o objeto é um vetor, e retorna `TRUE` se sim, e `FALSE` se não.

Ao contrário de outras linguagens, o R não diferencia entre escalares e vetores. Se você salvar só 1 valor, ele salva num vetor atômico de tamanho 1.


```r
v <- 1
is.vector(v)
```

```
## [1] TRUE
```

```r
length(v)
```

```
## [1] 1
```

```r
length(vetor)
```

```
## [1] 6
```

Vetores atômicos guardam suas informações em uma única dimensão (como se fosse uma caixinha de pílulas semanal), cada compartimento guarda um valor. 

#### Exercício

Teste se `vetor2`, criado anteriormente é um vetor. Crie um vetor com os nomes de cinco pessoas da sala.

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
