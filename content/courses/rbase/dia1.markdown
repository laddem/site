---
date: 2021-06-14
linkTitle: Básico
summary: Os conceitos-chave que vão fundamentar sua aprendizagem e seu uso da língua
title: A base da linguagem R
type: book
weight: 10
draft: false
---



## Fundamentos

### A interface do R e do RStudio

O RStudio oferece uma maneira de falar com seu computador. O R te fornece um idioma para falar. 
Para começar, abra o RStudio da mesma forma que você abriria qualquer outro aplicativo em seu computador.


Você digita o código R na linha superior do painel do console RStudio e, em seguida, clica em Enter para executá-lo. O código que você digita é chamado de comando, porque ele comandará seu computador para fazer algo por você. A linha em que você digita é chamada de linha de comando.

A interface do RStudio é simples. Para  criar um script novo é necessário ir em `File > New File > R > script` no menu no canto esquerdo. É recomendado fortemente que você escreva e edite todo o seu código R em um script antes de executá-lo no console. Por quê? Esse hábito cria um registro reproduzível de seu trabalho. Ao terminar o dia, você pode salvar seu script e usá-lo para executar novamente toda a sua análise no dia seguinte, além disso os scripts são muito úteis para editar e revisar seu código e são uma ótima cópia de seu trabalho para compartilhar com outras pessoas. Para salvar é só clicar no disquete no painel do script e depois ir em `File > Save`. 

Quando você digita um comando no script e pressiona Ctrl + Enter ou Run, o computador executa o comando e mostra os resultados no console logo abaixo. Por exemplo, se você digitar 1 + 1 e pressionar Ctrl + Enter, o RStudio exibirá:


```r
1 + 1
```

```
## [1] 2
```

### Objetos

Agora que você já sabe como o R funciona, vamos ver alguns operadores e objetos que podem ser criados. Se você quer que o R crie um vetor, use o operador `:`, esse operador vai retornar um conjunto unidimensional de números:


```r
1:6
```

```
## [1] 1 2 3 4 5 6
```

Mas quando você roda assim dessa forma, o R gera o vetor que você poderá ver o resultado no console, porém esse vetor não vai ficar salvo em lugar nenhum, é basicamente uma pegada de seis números que existiram naquela execução pontual. Se você quiser usar novamente essa sequência de número, você precisa pedir para o R guardar ele em algum lugar. Você pode fazer isso criando um `objeto`.

O R permite salvar dados armazenando-os dentro de um objeto R. O que é um objeto? Apenas um nome que você pode usar para acessar os dados armazenados. Por exemplo, você pode salvar dados em um objeto como `a` ou `b` ou qualquer nome que faça sentido para o que você está fazendo. Sempre que o R encontrar o objeto, ele irá substituí-lo pelos dados salvos nele, da seguinte forma:


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

Quando você cria um objeto no R, esse objeto vai aparecer armazenado na seção `Environment` no lado direito, do lado da seção script (essa que você digita os comandos para usá-los depois).

Você pode nomear um objeto no R de praticamente qualquer nome, só tem algumas regras: não pode começar com número, e também não pode ter alguns símbolos, como por exemplo:  `^`, `!`, `$`, `@`, `+`, `-`, `/`, `%` or `*`.

Além disso, o R é *case-sensitive*, portanto se eu colocar um nome de objeto como `Name` e outro como `name` eles vão se referir a objetos diferentes, ou seja, ele considera se a letra é maíuscula ou minúscula. 

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

Quando você usa dois ou mais vetores em uma operação, o R alinhará os vetores e executará uma sequência de operações individuais. Por exemplo, quando você executa vetor * vetor, o R alinha os dois vetores de dados e, em seguida, multiplica o primeiro elemento do vetor 1 pelo primeiro elemento do vetor 2, então multiplica o segundo elemento do vetor 1 pelo segundo elemento do vetor 2, e assim por diante, até que cada elemento tenha sido multiplicado. O resultado será um novo vetor com o mesmo comprimento dos dois primeiros:


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

O R vem com muitas funções instaladas. Praticamente tudo que você vê que não são `objetos` que você criou são funções e seus componentes. Por isso, dizemos que, no seu núcleo, o R é uma linguagem "funcional". Vejamos alguns exemplos de funções:


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
## [1] 4
```

```r
sample(x = vetor, size = 1)
```

```
## [1] 1
```

```r
sample(x = vetor, size = 1)
```

```
## [1] 1
```

Com os argumentos opcionais, podemos mudar detalhes do processo de amostragem


```r
sample(x = vetor, size = 1, prob = c(1/8, 1/8, 1/8, 1/8, 1/8, 3/8))
```

```
## [1] 4
```

E ao mudar as probabilidades de como cada amostra vai ser selecionada, eu criei um dado viciado!

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

Ok, mas suponha que você esteja cansado de repetir as coisas no seu programa. Existe uma operação que você realiza de novo e novo, e ela é relativamente simples, mas é um tédio ficar copiando e colando a mesma coisa. Por exemplo, se você tem o vetor que criamos com seis números, e precisa sortear dois valores e somar seu resultado, como se estivesse jogando dois dados de 6 faces e somando os resultados?


```r
x <- sample(vetor, size = 1)
y <- sample(vetor, size = 1)
x + y
```

```
## [1] 10
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
## [1] 3
```

```r
role()
```

```
## [1] 11
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

Na maioria das situações, esses conceitos soltos que introduzimos não são muito úteis isoladamente. Afinal de contas, objetos, funções e números individuais não servem para muita coisa. Esses conceitos ganham corpo quando os utilizamos juntos para produzir nossos programas. O que são programas? A metáfora mais comumemente utilizada é a culinária. Um programa é uma sequência de instruções, uma receita para produzir alguma coisa. A diferença é que ao invés de produtos culinários, os ingredientes são informações na memória de um computador, e ao invés de um prato, estamos tentando produzir resultados que podem ser analisados: estatísticas descritivas, representações gráficas, modelos matemáticos, tabelas, etc.

Falaremos mais sobre programas e estratégias de construção de programas (se der tempo), em outra aula. Por aqui, seria interessante que vocês levassem adiante a noção de que o programa é o conjunto da obra. É comum desenvolvermos ou encontrarmos programas para:

- importar e corrigir quaisquer problemas nos meus dados
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

Se você gostaria de consultar a página de ajuda de uma função, mas esqueceu o nome da função, você pode pesquisar por palavra-chave. Para fazer isso, digite dois pontos de interrogação seguidos por uma palavra-chave na linha de comando de R, o R exibirá uma lista de links para páginas de ajuda relacionadas à palavra-chave. Você pode pensar nisso como a página de ajuda para a página de ajuda:


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

#### Exercício

Teste se `vetor2`, criado anteriormente é um vetor. Crie um vetor com os nomes de cinco pessoas da sala.

#### Tamanho

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

Vetores atômicos guardam suas informações em uma única dimensão (como se fosse uma caixinha de pílulas semanal), cada compartimento guarda um valor. E todos os valores tem que ser do mesmo tipo! Números com números, caracteres com caracteres. Não pode misturar.


```r
inteiro <- 1L
double <- 1
texto <- "um"
```

Ou com mais de um valor:


```r
inteiros <- c(1L, 2L)
doubles <- c(1, 2)
textos <- c("um", "dois")
```

Esses tipos de vetores são importantes pois eles interagem com as funções de maneira lógica:


```r
sum(inteiros)
```

```
## [1] 3
```

```r
sum(doubles)
```

```
## [1] 3
```

```r
sum(textos)
```

```
## Error in sum(textos): 'type' inválido (character) do argumento
```

Veja a mensagem de erro ao tentar somar os textos.

#### Exercício

Considerando os dois vetores abaixo, calcule as suas médias. Porque não é possível calcular a média do segundo vetor.


```r
idade1 <- c(20, 25, 30, 35, 40, 45, 50)
idade2 <- c("20", "25", "30", "35", "40", "45", "50")
```

#### Tipos

Vetores atômicos podem ser de 6 tipos, dois são mais utilizados.

#### Números reais


```r
vetor
```

```
## [1] 1 2 3 4 5 6
```

```r
typeof(vetor)
```

```
## [1] "double"
```

A função `typeof` me diz qual o tipo de um vetor atômico.

#### Números inteiros


```r
inteiros
```

```
## [1] 1 2
```

```r
typeof(inteiros)
```

```
## [1] "integer"
```

O padrão do R é aceitar números reais, se você não especificar. Para forçar números inteiros, é precisar utilizar:


```r
inteiros <- c(1, 2)

typeof(inteiros)
```

```
## [1] "double"
```

```r
inteiros <- c(1L, 2L)

typeof(inteiros)
```

```
## [1] "integer"
```

Essas diferenças em geral são inconsequentes, mas existem alguns casos, como resultados de divisão e raíz quadrada, em que podemos encontrar resultados curiosos.


```r
sqrt(2)^2 - 2
```

```
## [1] 4.440892e-16
```

Em teoria, a operação acima deveria ser 0, mas como o computador tem uma quantidade limitada de memória para armazenar as casas decimais, ele acaba gerando uma "sobrinha" infinitesimal da raíz quadrada de dois.

##### Caracteres


```r
texto <- c("Bem", "Vindos")

texto
```

```
## [1] "Bem"    "Vindos"
```

```r
typeof(texto)
```

```
## [1] "character"
```

###### Exercício

Qual a diferença entre: 1, "1", "one" no R? Quais são números e quais são caracteres?

##### Lógicos


```r
3 < 4
```

```
## [1] TRUE
```

```r
3 > 4
```

```
## [1] FALSE
```

```r
logico <- c(TRUE, FALSE)

logico
```

```
## [1]  TRUE FALSE
```

```r
typeof(logico)
```

```
## [1] "logical"
```

Vetores lógicos são a base de várias operações úteis no R e vamos voltar a eles em diversos momentos.

Complexos e crus:


```r
comp <- c(1 + 1i, 1 + 2i, 1 + 3i)
comp
```

```
## [1] 1+1i 1+2i 1+3i
```

```r
typeof(comp)
```

```
## [1] "complex"
```

```r
raw(3)
```

```
## [1] 00 00 00
```

```r
typeof(raw(3))
```

```
## [1] "raw"
```

Vetores complexos servem para armazenar número complexos, enquanto vetores crus servem para armazenar os valores em bits de uma informação. Ambos são menos utilizados na análise de dados e eu os introduzo apenas para vocês saberem que eles existem.

### Atributos

Atributos são informações adicionais que podemos colocar em um objeto para cumprir uma série de tarefas auxiliares. Pense, por exemplo, em dar nomes para os meses do ano, ou classificar um objeto de acordo com seu tipo. Atributos são **metadados** ou dados sobre os dados, e eles nos interessam porque o R pode aproveitar os atributos de um objeto para realizar tarefas específicas.

Você pode ver os atributos de um objeto assim:


```r
attributes(vetor)
```

```
## NULL
```

O nosso vetor não tem nenhum atributo ainda, por isso `NULL`.

O atributo mais comum que os objetos podem ter em R são nomes. Podemos ver o atributo nomes assim:


```r
names(vetor)
```

```
## NULL
```

Novamente, `NULL` indica que o vetor não tem nomes.

A maioria das funções que trabalham com atributos vai ter esses "dois empregos". De um lado, você pode utilizá-las para obter (get) os atributos, de outro, você pode utilizá-las para modificar (set) os atributos, veja:


```r
names(vetor) <- c("one", "two", "three", "four", "five", "six")
```

Agora, veja como ficaram os resultados das duas funções anteriores.


```r
attributes(vetor)
```

```
## $names
## [1] "one"   "two"   "three" "four"  "five"  "six"
```

```r
names(vetor)
```

```
## [1] "one"   "two"   "three" "four"  "five"  "six"
```

O R também vai mostrar o atributo nomes quando você chamar o vetor.


```r
vetor
```

```
##   one   two three  four  five   six 
##     1     2     3     4     5     6
```

Da mesma forma que você modificou os atributos antes, você pode modificá-los ou removê-los:


```r
names(vetor) <- c("um", "dois", "três", "quatro", "cinco", "seis")
names(vetor)
```

```
## [1] "um"     "dois"   "três"   "quatro" "cinco"  "seis"
```

```r
names(vetor) <- NULL
names(vetor)
```

```
## NULL
```

O outro atributo importante para muitas tarefas são as dimensões de um objeto. Lembre que os nossos vetores atômicos são limitados pela exigência de só ter uma dimensão, mas e se quisermos organizar nossos dados em várias dimensões? Um jeito possível é alterar as dimensões:


```r
dim(vetor)
```

```
## NULL
```

```r
# 2 linhas e 3 colunas
dim(vetor) <- c(2, 3)

vetor
```

```
##      [,1] [,2] [,3]
## [1,]    1    3    5
## [2,]    2    4    6
```

```r
# 3 linhas e 2 colunas

dim(vetor) <- c(3, 2)

vetor
```

```
##      [,1] [,2]
## [1,]    1    4
## [2,]    2    5
## [3,]    3    6
```

E não precisa se limitar a objetos bidimensionais. Você pode passar n dimensões para o objeto e o R distribuirá os valores do seu vetor no número de dimensões necessários. O único detalhe importante é você reparar que o R tem uma certa preferência de ir preenchendo os valores da coluna antes dos valores da linha e se você quiser fazer isso de forma diferente, é melhor utilizar as funções `matrix` ou `array`, que veremos adiante.

#### Exercício

Usando seus conhecimentos sobre atributos, construa uma pequena matriz com o nome de 5 pessoas da turma e seu sexo. 

### Matrizes

Matrizes são muito parecidas com o que acabamos de construir: elas guardam informações em um `array` de duas dimensões. A grande diferença é que as matrizes são programadas para executar as operações matemáticas com matrizes de acordo com os princípios e convenções da álgebra linear. Portanto, o R vem com funções para transpor, inverter, solucionar, etc. matrizes. Você pode construir matrizes no R com:


```r
m <- matrix(vetor, nrow = 2)
m
```

```
##      [,1] [,2] [,3]
## [1,]    1    3    5
## [2,]    2    4    6
```

Ou, se você quiser mudar a ordem de preenchimento:


```r
m2 <- matrix(vetor, nrow = 2, byrow = TRUE)
m2
```

```
##      [,1] [,2] [,3]
## [1,]    1    2    3
## [2,]    4    5    6
```

Para mais informações, consulte `?matrix`.

#### Exercício

Reconstrua a sua matriz original usando a função `matrix` ao invés de alterar os atributos. E os nomes das colunas?

### Arrays

O array é uma extensão da ideia de matriz para quantas dimensões você quiser, ou n-dimensões. 3, 4, 5, 10 dimensões, isso é com você. Na prática os arrays não são muito frequentes na análise de dados, mas eles existem se você precisar deles um dia:


```r
a <- array(c(11:14, 21:24, 31:34), dim = c(2, 2, 3))
a
```

```
## , , 1
## 
##      [,1] [,2]
## [1,]   11   13
## [2,]   12   14
## 
## , , 2
## 
##      [,1] [,2]
## [1,]   21   23
## [2,]   22   24
## 
## , , 3
## 
##      [,1] [,2]
## [1,]   31   33
## [2,]   32   34
```

O display de um array multidimensional é meio confuso, porque o R vai te mostrar as "fatias" do array. Quanto mais dimensões, mais fatias. Boa sorte se você precisar deles um dia!

### Classe

A classe de um objeto é um atributo importante para seu funcionamento no R, porque diferentes classes estão associadas a diferentes métodos! Pense, por exemplo, que você deseja organizar uma sequência de valores em um vetor. Se esses valores são números, a ordem crescente ou decrescente é bastante intuitiva. Se são textos, podemos usar a ordem alfabética. Mas e se forem meses do ano? Grupos etários? Categorias educacionais?

Problemas dessa natureza são resolvidos no R através da atribuição de classes aos objetos. Transposição e solução de matrizes utilizarão o atributo classe para saber se aquele objeto é uma matriz, para citar apenas um exemplo. Podemos descobrir a classe de um objeto assim:


```r
class(vetor)
```

```
## [1] "matrix" "array"
```

Mas esse é o nosso vetor modificado. Vejamos o que acontece se retiramos o atributo `dim`:


```r
dim(vetor) <- NULL
class(vetor)
```

```
## [1] "numeric"
```

Veja que não mudamos o tipo do vetor, ele continua sendo "double",


```r
typeof(vetor)
```

```
## [1] "double"
```

Mas sua classe mudou. Podemos mudar a classe de um objeto de maneira arbitrária, mas em geral a gente evita fazer isso, porque esse atributo está muito relacionado com as propriedades desse objeto. Veja dois exemplos do que ocorre ao alterar manualmente a classe de um objeto:


```r
agora <- Sys.time()
agora
```

```
## [1] "2021-06-14 12:51:59 -03"
```

```r
typeof(agora)
```

```
## [1] "double"
```

```r
class(agora)
```

```
## [1] "POSIXct" "POSIXt"
```

Nesse primeiro exemplo, usando `Sys.time` para obter o horário local. O tipo dessa informação é "double", pois o R armazena variáveis tempo como o número de segundos passados entre uma data de início arbitrária e o momento que o seu tempo representa. Mas a classe desse objeto é POSIXct e POSIXt, que são as classes do R para lidar com objetos que registram data e tempo.

Veja o que acontece se eu temporariamente remover a classe desse objeto:


```r
unclass(agora)
```

```
## [1] 1623685919
```

R transformou meu objeto data/tempo em um número. O que ocorre é que quando meu objeto tem as classes adequadas, isso alerta o R para que ele trate esse objeto de forma diferente. As funções do R utilizarão métodos de `print` para facilitar a visualização de objetos data/tempo, mas, por debaixo do capô, preservarão suas características computacionais numéricas. Você pode, por exemplo, somar 24h ao seu objeto:


```r
agora + (24 * 60 * 60)
```

```
## [1] "2021-06-15 12:51:59 -03"
```

Você também pode, se assim desejar, transformar um valor numérico arbitrário numa data (só demonstração, não vale a pena fazer isso) atribuindo uma classe para ele:


```r
objeto <- 1000000000
class(objeto) <- c("POSIXct", "POSIXt")

objeto
```

```
## [1] "2001-09-08 22:46:40 -03"
```

#### Exercício

Experimente brincar com o valor de objeto e ver

- Qual é a data de referência do R para calcular tempos?
- O que acontece se o valor for negativo?

O segundo exemplo relevante são os fatores. Fatores são a classe utilizada pelo R para trabalhar com variáveis categóricas, ou informação qualitativa. Um fator só pode ter alguns valores pré-definidos pelo usuário. Pense, por exemplo, nas categorias de sexo ou raça que o IBGE utiliza em suas pesquisas amostrais. Há um pequeno número pré-definido de categorias. Para construir um fator, você pode fazer o seguinte:


```r
genero <- factor(c("homem", "mulher", "mulher", "homem"))

typeof(genero)
```

```
## [1] "integer"
```

```r
attributes(genero)
```

```
## $levels
## [1] "homem"  "mulher"
## 
## $class
## [1] "factor"
```

Fatores seguem aquela linha que descrevemos para datas. R guarda os valores do seu fator utilizando números simples:


```r
unclass(genero)
```

```
## [1] 1 2 2 1
## attr(,"levels")
## [1] "homem"  "mulher"
```

Mas na hora que você utiliza esse fator, o software apresenta para você os rótulos das categorias.


```r
genero
```

```
## [1] homem  mulher mulher homem 
## Levels: homem mulher
```

Através do atributo `levels`, o R está associando cada valor numérico com um rótulo, e no momento em que você procurar construir uma tabela ou trabalhar com esses fatores de qualquer maneira, o software utilizará o atributo classe para dar-lhe o tratamento adequado. Por exemplo, se eu tentar transformar os vetores a seguir em caractere, o R saberá distinguir:


```r
as.character(c(1, 2, 2, 1))
```

```
## [1] "1" "2" "2" "1"
```

```r
as.character(genero)
```

```
## [1] "homem"  "mulher" "mulher" "homem"
```

O uso da função `as.character` é um gancho perfeito para o próximo assunto. Mas antes:

#### Exercício

Construa um fator a partir do vetor a seguir que registre os meses do ano. Dica: utilize o argumento `levels` da função `factor`.


```r
f <- c(1, 3, 9, 4, 11, 2, 6, 6, 3, 2, 9, 11, 12, 12, 1, 8)
```

### Coerção

Coerção é o comportamento da linguagem R ao encontrar situações em que diferentes tipos de dados estão misturados em um mesmo vetor atômico. Lembrem que cada vetor atômico só pode armazenar 1 tipo de informação, portanto, ao tentar inserir uma informação de outro tipo, R devolverá:

- um erro, indicando que a operação é impossível, OU
- a operação será realizada, porém, o tipo da informação será modificado

No segundo caso, o R aplica uma coerção na tentativa de preservar ao máximo as informações. Veja exemplos:


```r
v <- c(1, 0, 1, TRUE, FALSE)
v
```

```
## [1] 1 0 1 1 0
```

```r
v <- c(1, 2, 3, "4")
v
```

```
## [1] "1" "2" "3" "4"
```

```r
v <- c(TRUE, FALSE, "TRUE")
v
```

```
## [1] "TRUE"  "FALSE" "TRUE"
```

R vai tentar transformar o tipo de dados mais específico num tipo de dado mais geral, assim, evitando que a informação seja perdida. 

Em outros casos, o processo de coerção pode ser iniciado pelo próprio usuário, porque este deseja transformar um tipo de dado em outro:


```r
v <- c(1, 0, 1, TRUE, FALSE)
as.logical(v)
```

```
## [1]  TRUE FALSE  TRUE  TRUE FALSE
```

```r
v <- c(1, 2, 3, "4")
as.numeric(v)
```

```
## [1] 1 2 3 4
```

```r
v <- c(TRUE, FALSE, "TRUE")
as.logical(v)
```

```
## [1]  TRUE FALSE  TRUE
```

Existem funções `as.____` para todos os tipos de dados e classes mais comuns do R, e é praxe que autores de novos pacotes que trazem classes também incluam seus próprios métodos de coerção para suas novas classes.

#### Exercício

Porque o R prefere coagir vetores lógicos mistos para números e vetores numéricos e lógicos para caractere?

### Listas

Até esse momento, trabalhamos com variações no vetor atômico. O vetor atômico tem uma limitação repetidamente discutida: ele só armazena um tipo de dado. Bancos de dados reais certamente armazenarão informações de vários tipos distintos, então precisamos de uma estrutura de dado que tenha a capacidade de guardar estas informações díspares. É aí que entram as listas.

Listas são como vetores atômicos, mas elas não guardam valores! Listas guardam OBJETOS R. Qualquer um. Vetores atômicos? Sim. Funções? Sim. Outras listas? Pode também. Para criar listas:


```r
list1 <- list(100:130, "R", list(TRUE, FALSE))
list1
```

```
## [[1]]
##  [1] 100 101 102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118
## [20] 119 120 121 122 123 124 125 126 127 128 129 130
## 
## [[2]]
## [1] "R"
## 
## [[3]]
## [[3]][[1]]
## [1] TRUE
## 
## [[3]][[2]]
## [1] FALSE
```

Note como cada elemento da lista é um objeto R diferente. A lista não julga, ela apenas armazena seus objetos. Um detalhe interessante e que será importante mais tarde: note como a saída do R diferencia [1] de [[1]] no índice dos elementos da lista. Isso é necessário porque a lista pode guardar muitas coisas dentro de si, e o usuário precisa ter a capacidade de recuperar essas informações que estão lá nas profundezas da lista. Falaremos um pouco mais sobre isso na aula de amanhã, quando discutirmos indexação.

#### Exercício

Crie uma lista de compras em que cada elemento da lista seja um vetor atômico de itens que você vai comprar de cada seção do supermercado. Para simplificar, utilize as seções: "limpeza", "mercearia" e "hortifruti".

### Data Frames

Data frames são o formato de dados mais popular em análise de dados, e por um bom motivo: eles combinam a flexibilidade necessária para armazenar diversos tipos de informações diferentes com a consistência das matrizes e dos vetores. Por trás da cortina, eles são listas com restrições: todos os elementos da lista devem ter o mesmo comprimento, isso garante a retangularidade da nossa informação.

Essa retangularidade é importante do ponto de vista analítico: o data frame deve conter uma observação para cada unidade de análise e uma coluna para cada informação ou variável que foi coletada sobre essa unidade de análise. Se, de alguma forma esta informação não está disponível, isso deverá ficar **explícito** por uma célula com valor desconhecido.

No R, embora isso não seja tão comum, você pode construir data frames manualmente:


```r
df <- data.frame(x = c(1, 2, 3, 4, 5),
                 y = c("a", "b", "c", "d", "e"),
                 z = c(TRUE, FALSE, TRUE, TRUE, FALSE))
df
```

```
##   x y     z
## 1 1 a  TRUE
## 2 2 b FALSE
## 3 3 c  TRUE
## 4 4 d  TRUE
## 5 5 e FALSE
```

Logo de cara, vemos que o `data.frame` do R tem aquela cara de tabela com a qual estamos acostumados. Podemos ver como o `data.frame` é construído observado alguns de seus atributos:


```r
typeof(df)
```

```
## [1] "list"
```

```r
class(df)
```

```
## [1] "data.frame"
```

```r
attributes(df)
```

```
## $names
## [1] "x" "y" "z"
## 
## $class
## [1] "data.frame"
## 
## $row.names
## [1] 1 2 3 4 5
```

Secretamente, o data.frame é uma lista, com nomes, um atributo menos importante, chamado `row.names`, e algumas características como aquelas que mencionamos acima. O `data.frame` e o `vetor` são as principais ferramentas no cotidiano do analista e, portanto, são as que mais vamos utilizar daqui pra frente. Outra função útil é a e`str`utura de uma lista ou data frame:


```r
str(df)
```

```
## 'data.frame':	5 obs. of  3 variables:
##  $ x: num  1 2 3 4 5
##  $ y: chr  "a" "b" "c" "d" ...
##  $ z: logi  TRUE FALSE TRUE TRUE FALSE
```

Ela oferece uma visão geral do data.frame, e é especialmente útil quando seu data.frame é grande e contém muitas variáveis.

Em geral, a digitação de data sets no R não é recomendada. A interface do programa não te ajuda a produzir dados no formato necessário. É provável que você cometa muitos erros no caminho e não há uma ferramenta muito completa dentro do software que facilite esse processo. Isso ocorre porque o R não é um software de produção ou tabulação de dados, como o Microsoft Excel, o OpenOffice Calc ou o IBM SPSS, mas sim um software de análise de dados. O mais comum é você importar um banco de dados pronto e previamente tabulado utilizando uma das funções do R, como no exemplo abaixo, em que importamos uma pequena amostra da PNAD Contínua do primeiro trimestre diretamente um link na internet.


```r
url <- "https://raw.githubusercontent.com/laddem/site/master/amostra_pnad.csv"
df <- read.csv(url)
head(df)
```

```
##    Ano Trimestre               UF  V1022  V2007 V2009  V2010 VD2003
## 1 2021         1            Ceará Urbana  Homem    80 Branca      2
## 2 2021         1 Distrito Federal Urbana Mulher    19  Parda      4
## 3 2021         1       Pernambuco Urbana Mulher    36  Preta      2
## 4 2021         1   Santa Catarina Urbana  Homem    50  Parda      2
## 5 2021         1        São Paulo Urbana Mulher    43  Parda      3
## 6 2021         1            Goiás Urbana  Homem    35  Parda      3
##                      VD3005 VD4016
## 1 16 anos ou mais de estudo     NA
## 2         11 anos de estudo     NA
## 3         12 anos de estudo   1045
## 4          5 anos de estudo   1500
## 5          9 anos de estudo   1600
## 6         14 anos de estudo   3000
```

Veremos mais sobre importação na aula de amanhã!

#### Exercício

Crie um data frame contendo informações de cinco colegas de turma: registre o nome, a idade presumida, o sexo, a profissão e a renda presumida. Não precisa perguntar, basta chutar um valor que você ache.

### Fórmulas

Fórmulas são a maneira que os desenvolvedores do R encontraram para representar equações matemáticas. Porém, como tudo no software livre, elas foram apropriadas por desenvolvedores para muitas tarefas criativas e vocês irão encontrá-las por aí cumprindo diversos papéis. Vejamos alguns exemplos.

Fórmulas são usadas para especificar as equações de modelos matemáticos no R:


```r
modelo_linear <- lm(VD4016 ~ VD3005, data = df)
summary(modelo_linear)
```

```
## 
## Call:
## lm(formula = VD4016 ~ VD3005, data = df)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -3786.1  -744.7  -345.1   382.7 15013.9 
## 
## Coefficients:
##                                                Estimate Std. Error t value
## (Intercept)                                       682.5      946.1   0.721
## VD300510 anos de estudo                           467.5     1221.4   0.383
## VD300511 anos de estudo                           434.8     1064.8   0.408
## VD300512 anos de estudo                           905.3      960.2   0.943
## VD300513 anos de estudo                          1584.8     1104.8   1.434
## VD300514 anos de estudo                          1497.5     1119.4   1.338
## VD300515 anos de estudo                          1525.8     1092.5   1.397
## VD300516 anos ou mais de estudo                  3303.6      970.1   3.406
## VD30052 anos de estudo                            217.5     1445.2   0.150
## VD30053 anos de estudo                            261.5     1269.3   0.206
## VD30054 anos de estudo                            727.8     1119.4   0.650
## VD30055 anos de estudo                            649.5     1019.0   0.637
## VD30056 anos de estudo                            610.5     1064.8   0.573
## VD30057 anos de estudo                           -163.7     1158.7  -0.141
## VD30058 anos de estudo                            990.8     1064.8   0.931
## VD30059 anos de estudo                            762.6     1007.2   0.757
## VD3005Sem instrução e menos de 1 ano de estudo    131.8     1186.0   0.111
##                                                Pr(>|t|)    
## (Intercept)                                    0.471128    
## VD300510 anos de estudo                        0.702120    
## VD300511 anos de estudo                        0.683236    
## VD300512 anos de estudo                        0.346404    
## VD300513 anos de estudo                        0.152291    
## VD300514 anos de estudo                        0.181807    
## VD300515 anos de estudo                        0.163341    
## VD300516 anos ou mais de estudo                0.000733 ***
## VD30052 anos de estudo                         0.880452    
## VD30053 anos de estudo                         0.836892    
## VD30054 anos de estudo                         0.515999    
## VD30055 anos de estudo                         0.524230    
## VD30056 anos de estudo                         0.566756    
## VD30057 anos de estudo                         0.887695    
## VD30058 anos de estudo                         0.352699    
## VD30059 anos de estudo                         0.449441    
## VD3005Sem instrução e menos de 1 ano de estudo 0.911583    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1892 on 370 degrees of freedom
##   (613 observations deleted due to missingness)
## Multiple R-squared:  0.2427,	Adjusted R-squared:  0.2099 
## F-statistic: 7.409 on 16 and 370 DF,  p-value: 4.467e-15
```

Também são usadas para algumas funções que produzem tabulações cruzadas:


```r
xtabs(~ VD3005 + V2007, data = df)
```

```
##                                           V2007
## VD3005                                     Homem Mulher
##   1 ano de estudo                             12     17
##   10 anos de estudo                            8     16
##   11 anos de estudo                           10     14
##   12 anos de estudo                          105    111
##   13 anos de estudo                           12      7
##   14 anos de estudo                            6     13
##   15 anos de estudo                            9      8
##   16 anos ou mais de estudo                   51     61
##   2 anos de estudo                            14     13
##   3 anos de estudo                            16     15
##   4 anos de estudo                            19     27
##   5 anos de estudo                            44     46
##   6 anos de estudo                            25     23
##   7 anos de estudo                            13     20
##   8 anos de estudo                            25     19
##   9 anos de estudo                            44     27
##   Sem instrução e menos de 1 ano de estudo    47     45
```

Algumas funções que produzem gráficos também utilizam fórmulas:


```r
boxplot(VD4016 ~ V2010, data = df)
```

<img src="/courses/rbase/dia1_files/figure-html/unnamed-chunk-70-1.png" width="672" />

Falaremos mais de fórmulas no futuro, a medida que elas forem aparecendo. Nos exemplos acima, é possível ver que a fórmula não tem apenas um significado. Dependendo do contexto da função, ela pode fazer coisas muito diferentes, usando seus lados esquerdo e direito para tarefas distintas. O que é importante vocês levarem com vocês é o formato das fórmulas, e uma intuição de que quando uma fórmula aparece, algo está sendo feito com uma lógica do tipo: "Para cada x, corresponde um y", ou algo similar.

#### Exerício

Consulte o `?xtabs`, qual o significado da fórmula e para que servem os lados esquerdo e direito?

## Revisão

Cobrimos bastante coisa na aula de hoje, e não é nosso interesse que vocês saiam dessa aula decorando tudo. Vamos destacar alguns pontos mais importantes, que são a chave para vocês trabalharem no R.

Objetos são formas de armazenar coisas na memória do computador, eles podem ser de uma variedade de tipos e classes, e ter vários formatos mais ou menos adequados as nossas necessidades. Os objetos mais comuns no nosso arsenal são as funções, os vetores e os data frames.

As funções são os verbos de uma linguagem de programação, elas fazem coisas aos nossos objetos. Você provavelmente vai passar a maior parte do tempo usando funções pré-programadas, mas também pode escrever suas próprias.

Os vetores são a forma mais simples de organizar dados, em geral, trabalharemos com vetores atômicos, que guardam sequências de informações do mesmo tipo e uma única dimensão. Vetores de 2 dimensões, ou matrizes, podem ser ocasionalmente importantes para vocês em algum momento.

Os data frames são o feijão com arroz da análise de dados, eles são flexíveis para acomodar tipos de dados distintos e respeitam as convenções que são importantes para nós: consistência de operações, uma observação por linha, uma variável por coluna.

Os objetos podem ter diversos atributos. Muito deles são apenas estéticos para facilitar a comunicação com o usuário, outros são mais substanciais, e alteram a forma como o R processa os dados guardados ali. Citamos exemplos de fatores e data/tempo como exemplos. Um dos atributos mais importantes de um objeto é sua classe, que discutimos um pouco.

Por fim, falamos de dois comportamentos importantes do software: coerção e reciclagem. Eles podem facilitar ou complicar a nossa vida se não tomamos cuidado com esse comportamento. A coerção transforma o tipo dos vetores para um tipo mais genérico com o intuito de preservar informações. A reciclagem aumenta o tamanho de objetos menores para bater com o tamanho de objetos maiores numa mesma operação.

### Exercícios

1. Como você poderia identificar o tipo de um objeto? Como você poderia identificar a classe dele? Qual a diferença entre essas duas coisas? Porque isso é relevante?

2. Digamos que você quer armazenar algumas informações na memória do computador. Que tipo de objeto você utilizaria para armanzenar:

- Os nomes dos colegas da sua turma
- Seus números de telefone
- Uma variável que indica se esta pessoa nasceu antes de 1989
- A idade de um grupo de pessoas
- Informações de cadastro de uma pessoa: nome completo, afiliações, telefones para contato, endereços, etc.
- Uma coleção de funções que você utiliza frequentemente

3. Porque no resumo eu disse que as funções são verbos? Que tipo de ações as funções que vimos na aula fazem nos nossos objetos? Se as funções são verbos, que classe de palavras a gente poderia dizer que são os nossos objetos? E nós, que usamos o software, o que somos?

4. Digamos que eu quero armazenar as informações de cadastro dos membros da turma. Que estrutura de dados eu deveria utilizar? Como você implementaria esta estrutura no R? Desenvolva um pequeno exemplo.

5. Quais são os atributos de um data frame? Como você poderia descobrí-los e alterá-los? Em que situações isso seria proveitoso?

6. Suponha que você têm o vetor atômico abaixo:


```r
v <- c(1, 1, TRUE, FALSE)
```

O que acontecerá com as informações desse vetor ao ser armazenado no R? Como você poderia alterar esse resultado? Porque o R se comporta dessa maneira?

7. Considere a operação matemática abaixo:


```r
v1 <- c(1, 2, 3)
v2 <- c(10, 20, 30, 40, 50, 60, 70, 80, 90, 100)

v1 * v2
```

O que você espera encontrar na saída do R ao rodar essa seção? Rode o código e responda: você se surpreendeu? O que aconteceu e porquê? Qual o significado da mensagem de aviso?

8. Considere o banco de dados abaixo:


```r
sala <- data.frame(
  id = c(1, 2, 3),
  idade = c(20, 25, 30, 35, 40, 45),
  nome = c("Fulano", "Cicrano", "Beltrano", "Herculano", "Mariano", "Carrano"),
  sexo = "Masculino",
  origem = c("Campinas", "Barueri", "Monte Verde", "Rio de Janeiro", "Natal", "Belo Horizonte")
)
```

Verifique as variáveis `id` e `sexo`. Os valores dessas variáveis fazem sentido? Elas não impedem a construção do data frame, por quê? Que característica do R está operando nessas variáveis?
