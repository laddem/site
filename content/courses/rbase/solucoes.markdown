---
title: "Soluções"
subtitle: ""
summary: "Soluções para os exercícios feitos em sala e os exercícios de revisão do final das aulas."
authors: []
tags: []
categories: []
date: 2021-06-15T20:04:57-03:00
lastmod: 2021-06-15T20:04:57-03:00
featured: false
draft: false
type: book

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.

image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---




Soluções para os exercícios feitos em sala e os exercícios de revisão do final das aulas.

## Aula 1: Básico

### Fundamentos

#### Funções


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

Só de olhar para as funções e seus resultados, você conseguíria dizer:

- O que cada função faz?
- Quais entradas (inputs) ela pede?
- Qual saída ela produz (output)?
- Que opções alternativas (argumentos) elas poderiam conter?

A função `round()` arredonda valores decimais, enquanto `factorial()` calcula o fatorial de um número. Ambas recebem inputs numéricos, e emitem outputs numéricos também. `round()` tem um argumento extra `digits`, que permite determina para quantas casas decimais o número deverá ser arredondado.

#### Argumentos


```r
dado <- 1:6

# dado limpo
sample(dado, size = 1)
```

```
## [1] 5
```

```r
# dado viciado
sample(dado, size = 1, prob = c(1/8, 1/8, 1/8, 1/8, 1/8, 3/8))
```

```
## [1] 2
```


Repita a operação de sample acima com o dado “viciado” e com o dado “limpo” e verifique se você consegue perceber empiricamente que meu dado está “viciado”.

Esse exercício era para repetir o código várias vezes e verificar se o resultado 6 aparecia mais no dado viciado que no limpo.

Que outros argumentos existem na função sample? Qual o papel do argumento replace?

Sample tem os argumento opcionais `size`, `replace` e `prob`. O primeiro define quantas amostram devem ser retiradas, o segundo define se a amostra tem reposição, ou seja, se um valor sorteado é retirado da amostra ou não e o terceiro define com que probabilidades devem ser feitos os sorteios, o padrão é com probabilidades iguais para todos os valores do vetor `x`.

#### Sua primeira função

Escreva uma função que role 2 dados de 10 faces e some seus resultados.


```r
role10 <- function() {
  x <- sample(x = 1:10, size = 1)
  y <- sample(x = 1:10, size = 1)
  x + y
}

role10()
```

```
## [1] 10
```

#### Ajuda

Consulte a ajuda das funções sum, mean, min, max, range. Porque todas elas tem o argumento na.rm? O que argumento o trim em mean faz? Qual a melhor maneira de rapidamente entender o que uma função faz através da página de ajuda?


```r
?sum
?mean
?min
?max
?range
```

O argumento na.rm permite que os cálculos das medidas sejam feitos mesmo na presença de valores nulos/desconhecidos, registrados no R como `NA`. Você pode ler o argumento `na.rm` como "Remova os `NA`s". A melhor maneira de entender o funcionamento de uma função rapidamente é ler as seções "Description" e "Examples", que você pode utilizar para rapidamente entender como a função funciona na prática.

### Objetos em R

#### Vetores

Teste se vetor2, criado anteriormente é um vetor. Crie um vetor com os nomes de cinco pessoas da sala.


```r
vetor2 <- c(1, 2, 3, 4, 5)
is.vector(vetor2)
```

```
## [1] TRUE
```

```r
nomes <- c("Pessoa 1", "Pessoa 2", "Pessoa 3", "Pessoa 4", "Pessoa 5")
nomes
```

```
## [1] "Pessoa 1" "Pessoa 2" "Pessoa 3" "Pessoa 4" "Pessoa 5"
```

A função `is.vector` testa se um objeto é do tipo vetor. Existem outras funções da família `is.____` para todos os objetos R. Você pode criar um vetor de nomes usando aspas ao redor dos valores das informações em cada posição do vetor e atribuir isso `<-` para um objeto com um nome da sua preferência.

________________________________

Considerando os dois vetores abaixo, calcule as suas médias. Porque não é possível calcular a média do segundo vetor.


```r
idade1 <- c(20, 25, 30, 35, 40, 45, 50)
idade2 <- c("20", "25", "30", "35", "40", "45", "50")

mean(idade1)
```

```
## [1] 35
```

```r
mean(idade2)
```

```
## Warning in mean.default(idade2): argumento não é numérico nem lógico: retornando
## NA
```

```
## [1] NA
```

O primeiro vetor retorna normalmente porque `idade1` é um vetor de números. Apesar da sua aparência, `idade2` é um vetor de texto. Podemos verificar o tipo do vetor da seguinte forma:


```r
typeof(idade1)
```

```
## [1] "double"
```

```r
typeof(idade2)
```

```
## [1] "character"
```

________________________________

Qual a diferença entre: 1, “1”, “one” no R? Quais são números e quais são caracteres?

Ao invés de responder essa pergunta, podemos usar o R para satisfazer nossa curiosidade:


```r
typeof(1)
```

```
## [1] "double"
```

```r
typeof("1")
```

```
## [1] "character"
```

```r
typeof("one")
```

```
## [1] "character"
```

O primeiro é um número, os outros são caracteres de texto.

#### Atributos

Usando seus conhecimentos sobre atributos, construa uma pequena matriz com o nome de 5 pessoas da turma e seu sexo.


```r
info <- c("Pessoa 1", "Pessoa 2", "Pessoa 3", "Pessoa 4", "Pessoa 5", "Masculino", "Masculino", "Masculino", "Feminino", "Feminino")

dim(info) <- c(5, 2)

info
```

```
##      [,1]       [,2]       
## [1,] "Pessoa 1" "Masculino"
## [2,] "Pessoa 2" "Masculino"
## [3,] "Pessoa 3" "Masculino"
## [4,] "Pessoa 4" "Feminino" 
## [5,] "Pessoa 5" "Feminino"
```

#### Matrizes

Reconstrua a sua matriz original usando a função matrix ao invés de alterar os atributos. E os nomes das colunas?


```r
info_matrix <- matrix(info, nrow = 5, dimnames = list(1:5, c("Nome", "Sexo")))
info_matrix
```

```
##   Nome       Sexo       
## 1 "Pessoa 1" "Masculino"
## 2 "Pessoa 2" "Masculino"
## 3 "Pessoa 3" "Masculino"
## 4 "Pessoa 4" "Feminino" 
## 5 "Pessoa 5" "Feminino"
```

#### Classe

Experimente brincar com o valor de objeto e ver

- Qual é a data de referência do R para calcular tempos?
- O que acontece se o valor for negativo?


```r
objeto <- 0
class(objeto) <- c("POSIXct", "POSIXt")

objeto
```

```
## [1] "1969-12-31 21:00:00 -03"
```

```r
objeto <- -100000000
class(objeto) <- c("POSIXct", "POSIXt")

objeto
```

```
## [1] "1966-10-31 11:13:20 -03"
```

Este exercício parte do princípio de que vocês compreenderam como o R entende tempos: eles são armazenadas como o número de segundos entre a data de referência do sistema e o tempo que ele pretendem medir. Assim, para descobrir a data de referência basta criar um objeto com o tempo 0 e passar a classe adequada para seus atributos e ver o resultado na tela. Da mesma forma, os números negativos representam tempos ocorridos antes dessa data de referência.

__________________________

Construa um fator a partir do vetor a seguir que registre os meses do ano. Dica: utilize o argumento levels da função factor.


```r
f <- c(1, 3, 9, 4, 11, 2, 6, 6, 3, 2, 9, 11, 12, 12, 1, 8)

factor(f, levels = 1:12, 
       labels = c("Jan", "Fev", "Mar", "Abr", "Mai", "Jun",
                  "Jul", "Ago", "Set", "Out", "Nov", "Dez"))
```

```
##  [1] Jan Mar Set Abr Nov Fev Jun Jun Mar Fev Set Nov Dez Dez Jan Ago
## Levels: Jan Fev Mar Abr Mai Jun Jul Ago Set Out Nov Dez
```

#### Coerção

Porque o R prefere coagir vetores lógicos mistos para números e vetores numéricos e lógicos para caractere?

A resposta é mais conceitual que prática, mas o objetivo é usar o tipo de dado mais genérico possível de tal forma que a gente não perca as informações. Veja alguns exemplos abaixo:


```r
# Números misturados com valores lógicos
v <- c(32, 64, 128, 256, TRUE, FALSE)
v
```

```
## [1]  32  64 128 256   1   0
```

```r
# Posso "recuperar" meus valores lógicos depois
as.logical(v[5:6])
```

```
## [1]  TRUE FALSE
```

```r
# Texto misturado com números
v <- c(15, 21, 19, 80, "Abóbora", "Caqui")
v
```

```
## [1] "15"      "21"      "19"      "80"      "Abóbora" "Caqui"
```

```r
# Posso "recuperar" meus números depois
as.numeric(v[1:4])
```

```
## [1] 15 21 19 80
```

#### Listas

Crie uma lista de compras em que cada elemento da lista seja um vetor atômico de itens que você vai comprar de cada seção do supermercado. Para simplificar, utilize as seções: “limpeza”, “mercearia” e “hortifruti”.


```r
lista_de_compras <- list(
  limpeza = c("Creme dental", "Sabonete", "Xampú", "Condicionador", "Tira-limo"),
  mercearia = c("Pão", "Leite", "Ovos", "Queijo", "Presunto", "Manteiga", "Biscoito"),
  hortifruti = c("Cebola", "Tomate", "Alho", "Banana", "Mamão", "Abacate", "Batata", "Pimenta dedo-de-moça")
)

lista_de_compras
```

```
## $limpeza
## [1] "Creme dental"  "Sabonete"      "Xampú"         "Condicionador"
## [5] "Tira-limo"    
## 
## $mercearia
## [1] "Pão"      "Leite"    "Ovos"     "Queijo"   "Presunto" "Manteiga" "Biscoito"
## 
## $hortifruti
## [1] "Cebola"               "Tomate"               "Alho"                
## [4] "Banana"               "Mamão"                "Abacate"             
## [7] "Batata"               "Pimenta dedo-de-moça"
```

A principal característica da lista que eu queria ressaltar aqui é que podemos armazenar várias informações de tipos e tamanhos diferentes numa lista. É a estrutura de dados mais flexível no R.

#### Data Frames

Crie um data frame contendo informações de cinco colegas de turma: registre o nome, a idade presumida, o sexo, a profissão e a renda presumida. Não precisa perguntar, basta chutar um valor que você ache.


```r
df <- data.frame(nome = c("Colega 1", "Colega 2", "Colega 3", "Colega 4", "Colega 5"),
                 idade = c(20, 30, 40, 25, 35),
                 sexo = c("M", "F", "F", "F", "M"), 
                 profissao = c("Professor", "Pesquisador", "Programador", "Jornalista", "Analista de dados"),
                 renda = c(2000, 3000, 1500, 2000, 4000))

df
```

```
##       nome idade sexo         profissao renda
## 1 Colega 1    20    M         Professor  2000
## 2 Colega 2    30    F       Pesquisador  3000
## 3 Colega 3    40    F       Programador  1500
## 4 Colega 4    25    F        Jornalista  2000
## 5 Colega 5    35    M Analista de dados  4000
```

### Revisão

1. Como você poderia identificar o tipo de um objeto? Como você poderia identificar a classe dele? Qual a diferença entre essas duas coisas? Porque isso é relevante?

   Supondo que você tenha um objeto desconhecido, você pode usar as funções descritas na aula para identificar seu tipo:

   
   ```r
   v <- c(1, 2, 3, 4, NA)
   
   typeof(v)
   ```
   
   ```
   ## [1] "double"
   ```
   
   ```r
   class(v)
   ```
   
   ```
   ## [1] "numeric"
   ```
   
   ```r
   is.vector(v)
   ```
   
   ```
   ## [1] TRUE
   ```
   
   ```r
   is.list(v)
   ```
   
   ```
   ## [1] FALSE
   ```

   O tipo de um objeto define quais informações estão guardadas nele:

   - double guardam números reais, que contém casas decimais
   - integer guardam números inteiros
   - character guardam strings de caracteres em formato texto
   - listas guardam objetos R, como vetores, funções, outras listas

   A classe de um objeto define como o R vai tratá-lo:

   - fatores serão armazenados como números inteiros, mas serão representados por rótulos de texto sempre que o usuário desejar ver seu conteúdo.
   - datas serão armazenadas como números de segundos, mas serão representadas em formato de data/hora legível por pessoas.
   - matrizes codificam operações como transposição e multiplicação de matrizes que não funcionam em outros tipos de dado

   Esses são apenas alguns exemplos de classe, mas acho que deu pra pegar a ideia.

2. Digamos que você quer armazenar algumas informações na memória do computador. Que tipo de objeto você utilizaria para armanzenar:

   - Os nomes dos colegas da sua turma
   - Seus números de telefone
   - Uma variável que indica se esta pessoa nasceu antes de 1989
   - A idade de um grupo de pessoas
   - Informações de cadastro de uma pessoa: nome completo, afiliações, telefones para contato, endereços, etc.
   - Uma coleção de funções que você utiliza frequentemente

Você pode armazenar valores em vetores atômicos, então nomes de colegas poderiam ficar num vetor de caracteres, números de telefone num vetor de números inteiros, uma variável que indica se alguém nasceu antes de 1989 ou depois é perfeita para um vetor lógico.

A idade de um grupo de pessoas já parece conter mais de uma informação por observação, então ela pode ser armazenada em um data.frame, contendo a identificação da pessoa e a sua idade.

Um caso parecido, mas levemente diferente são as informações de cadastro de uma pessoa, que deveriam ser armazenadas numa lista, pois o cadastro pode conter informações de diferentes formatos e tamanhos e não necessariamente teria o formato retangular do data.frame.

Uma coleção de funções deve ser armazenadas numa lista, pois este é objeto R adequado para armazenar tudo que não for uma coleção de informações como num vetor atômico.

3. Porque no resumo eu disse que as funções são verbos? Que tipo de ações as funções que vimos na aula fazem nos nossos objetos? Se as funções são verbos, que classe de palavras a gente poderia dizer que são os nossos objetos? E nós, que usamos o software, o que somos?

Se você parar para pensar, programação de computadores é uma forma de comunicação envolvendo pelo menos 3 interlocutores: você, a máquina e outros programadores. Na sintaxe do português, as frases são divididas em sujeito e predicado, sendo que o predicado geralmente tem um verbo e alguns complementos opcionais. O sujeito de uma frase na programação geralmente está oculto, pois ele é implicitamente você, que pede que a máquina execute tarefas. O verbo são as funções, que indicam à máquina que ações deverão ser executadas, e os complementos são os argumentos da função, em geral, objetos contendo informações, bem como opções (adjetivos, advérbios) que descrevem não apenas EM QUEM as tarefas serão executadas, mas também COMO.

4. Digamos que eu quero armazenar as informações de cadastro dos membros da turma. Que estrutura de dados eu deveria utilizar? Como você implementaria esta estrutura no R? Desenvolva um pequeno exemplo.

Você deveria utilizar um data frame para guardar as várias informações de cadastro da turma. Note que é um pouco diferente do exemplo acima, em que eu tenho o cadastro de uma pessoa. Em geral, parte do processo de tabulação de um banco de dados é coletar informações que estão em formatos díspares e transformá-las em algo que pode ser analisado com facilidade, ou seja, um data frame.


```r
df <- data.frame(nome = c("Fulano", "Beltrano"),
                 telefone = c(123456, 456321),
                 endereco = c("Onde Judas perdeu as botas", "Duas ruas pra baixo"),
                 sexo = c("M", "F"),
                 email = c("fulano@email.com", "beltrano@nomail.com"), 
                 uf = c("SP", "RN"), 
                 cidade = c("Borá", "São Miguel do Gostoso"))

df
```

```
##       nome telefone                   endereco sexo               email uf
## 1   Fulano   123456 Onde Judas perdeu as botas    M    fulano@email.com SP
## 2 Beltrano   456321        Duas ruas pra baixo    F beltrano@nomail.com RN
##                  cidade
## 1                  Borá
## 2 São Miguel do Gostoso
```

Em geral, não é comum construir bancos de dados no R, existem softwares com facilidades melhores para a digitação de informações. O exemplo acima apenas demonstra minimamente o resultado do que, provavelmente, seria uma importação de um cadastro já salvo.

5. Quais são os atributos de um data frame? Como você poderia descobrí-los e alterá-los? Em que situações isso seria proveitoso?

Usando o data frame construído no exercício anterior, podemos verificar seus atributos:


```r
attributes(df)
```

```
## $names
## [1] "nome"     "telefone" "endereco" "sexo"     "email"    "uf"       "cidade"  
## 
## $class
## [1] "data.frame"
## 
## $row.names
## [1] 1 2
```

Podemos alterar esses atributos usando a forma `<-` da mesma função:


```r
attributes(df) <- list(row.names = c("Oi", "Td bem?"),
                       names = c("Nome", "Telefone", "Endereço", "Sexo", "Email", "Uf", "Cidade"),
                       class = "data.frame")
df
```

```
##             Nome Telefone                   Endereço Sexo               Email
## Oi        Fulano   123456 Onde Judas perdeu as botas    M    fulano@email.com
## Td bem? Beltrano   456321        Duas ruas pra baixo    F beltrano@nomail.com
##         Uf                Cidade
## Oi      SP                  Borá
## Td bem? RN São Miguel do Gostoso
```

Em geral, no entanto, evitamos usar essa última forma e usamos as funções acessórias para modificar os atributos sem bagunçar nosso objeto! Se você não especificar TODOS os atributos na lista, ele vai desmanchar sua festa... Por isso, preferimos alterar cada um individualmente:


```r
row.names(df) <- c(1, 2)
df
```

```
##       Nome Telefone                   Endereço Sexo               Email Uf
## 1   Fulano   123456 Onde Judas perdeu as botas    M    fulano@email.com SP
## 2 Beltrano   456321        Duas ruas pra baixo    F beltrano@nomail.com RN
##                  Cidade
## 1                  Borá
## 2 São Miguel do Gostoso
```

Em geral, é proveitoso alterar atributos de um objeto como seus nomes para facilitar a legibilidade do código e a digitação. Outros atributos mais consequentes como a classe, em geral, não é proveitoso alterar diretamente. As funções que criam e manipulam objetos costumam modificar a classe de um objeto automaticamente e da forma correta, e não precisamos nos preocupar com isso. No entanto, é útil saber a classe de um objeto pois isto pode nos ajudar a identificar a origem de um erro. Há um exemplo na aula 2 em que tentamos tirar a média de um valor que está salvo num data frame incorretamente.


```r
df <- data.frame(idade = c(20, 30, 40, 50))

mean(df["idade"])
```

```
## Warning in mean.default(df["idade"]): argumento não é numérico nem lógico:
## retornando NA
```

```
## [1] NA
```

O código não funciona, pois estamos tentando tirar a média de uma lista:


```r
typeof(df["idade"])
```

```
## [1] "list"
```

Para tirar uma média, precisamos acessar os valores que estão no vetor atômico guardado dentro da lista:


```r
df[["idade"]]
```

```
## [1] 20 30 40 50
```

```r
typeof(df[["idade"]])
```

```
## [1] "double"
```

```r
# ou

df$idade
```

```
## [1] 20 30 40 50
```

```r
typeof(df$idade)
```

```
## [1] "double"
```

Agora sim:


```r
mean(df$idade)
```

```
## [1] 35
```

```r
mean(df[["idade"]])
```

```
## [1] 35
```


6. Suponha que você têm o vetor atômico abaixo:


```r
v <- c(1, 1, TRUE, FALSE)
```

O que acontecerá com as informações desse vetor ao ser armazenado no R? Como você poderia alterar esse resultado? Porque o R se comporta dessa maneira?

Como já discutido anteriormente, os valores `TRUE/FALSE` serão coagidos a `1/0`, e você pode usar as funções `as._____` para converter para o formato desejado.


```r
as.logical(v)
```

```
## [1]  TRUE  TRUE  TRUE FALSE
```

O R se comporta dessa maneira pois ele visa preservar as informações no formato mais genérico possível.

7. Considere a operação matemática abaixo:


```r
v1 <- c(1, 2, 3)
v2 <- c(10, 20, 30, 40, 50, 60, 70, 80, 90, 100)

v1 * v2
```

O que você espera encontrar na saída do R ao rodar essa seção? Rode o código e responda: você se surpreendeu? O que aconteceu e porquê? Qual o significado da mensagem de aviso?

Antes de rodar, você já deve ter notado que estamos fazendo uma multiplicação de cada elemento do vetor 1 por cada elemento do vetor 2, porém, eles tem tamanhos distintos. Não nos deve surpreender então que o R recicle os argumentos do vetor menor até dar o tamanho do vetor maior. Como o comprimento do v2 não é múltiplo de v1, recebemos um aviso, mas mesmo que não recebamos esse aviso, precisamos estar atentos a reciclagens que não fizemos intencionalmente!

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

O valor da variável id está incorreto, pois temos ids repetidas para pessoas diferentes. O valor da variável sexo está correto, aparentemente, de acordo com a variável nome. Em ambos os casos, R está reciclando os vetores mais curtos para preencher os espaços vazios e criar um data frame completo. A variável id precisa ser corrigida, pois a reciclagem aqui está prejudicando a consistência da informação, mas no caso de sexo, é um uso válido da regra da reciclagem para evitar repetição.

## Aula 2: Manipulação de dados

### Leitura/Importação

Visualize os dados que você acabou de importar em formato de planilha através do Environment e usando a função View().
É possível também visualizar as 6 primeiras linhas do data.frame ou as 6 últimas linhas. Para isso, use as funções head() e tail().

Obs: se você quiser ver mais que 6 linhas, coloque o segundo argumento na função head(df, 10). Quantas linhas vão ser mostradas neste caso?


```r
url <- "https://raw.githubusercontent.com/laddem/site/master/amostra_pnad.csv"
df <- read.csv(url)
```



```r
View(df)
```



```r
head(df, 10)
```

```
##     Ano Trimestre               UF  V1022  V2007 V2009   V2010 VD2003
## 1  2021         1            Ceará Urbana  Homem    80  Branca      2
## 2  2021         1 Distrito Federal Urbana Mulher    19   Parda      4
## 3  2021         1       Pernambuco Urbana Mulher    36   Preta      2
## 4  2021         1   Santa Catarina Urbana  Homem    50   Parda      2
## 5  2021         1        São Paulo Urbana Mulher    43   Parda      3
## 6  2021         1            Goiás Urbana  Homem    35   Parda      3
## 7  2021         1            Amapá Urbana  Homem    72   Parda      3
## 8  2021         1        São Paulo Urbana Mulher    23 Amarela      8
## 9  2021         1 Distrito Federal Urbana  Homem    23   Parda      5
## 10 2021         1         Maranhão  Rural  Homem    47   Parda      9
##                       VD3005 VD4016
## 1  16 anos ou mais de estudo     NA
## 2          11 anos de estudo     NA
## 3          12 anos de estudo   1045
## 4           5 anos de estudo   1500
## 5           9 anos de estudo   1600
## 6          14 anos de estudo   3000
## 7  16 anos ou mais de estudo     NA
## 8          12 anos de estudo   1200
## 9          14 anos de estudo   1200
## 10          5 anos de estudo   2000
```

```r
tail(df)
```

```
##       Ano Trimestre             UF  V1022  V2007 V2009  V2010 VD2003
## 995  2021         1          Bahia  Rural Mulher    15  Preta      3
## 996  2021         1       Amazonas  Rural Mulher    32  Parda      3
## 997  2021         1       Maranhão Urbana  Homem    69  Parda      3
## 998  2021         1          Ceará Urbana  Homem    33 Branca      6
## 999  2021         1          Goiás Urbana  Homem    58  Parda      3
## 1000 2021         1 Santa Catarina  Rural  Homem    66 Branca      2
##                         VD3005 VD4016
## 995           8 anos de estudo     NA
## 996          12 anos de estudo     NA
## 997           3 anos de estudo     NA
## 998          12 anos de estudo   1000
## 999  16 anos ou mais de estudo   7000
## 1000          5 anos de estudo   1000
```

Exercício apenas para vocês se familiarizem com as formas de ver o conteúdo de bancos de dados em R. A função View() é útil para bancos pequenos, mas tem performance ruim para bancos com muitas informações. head() e tail() são ótimas formas de verificar a forma geral do banco de dados sem precisar sobrecarregar seu computador.

### Selecionando valores

Retorne a informação da observação 800, e as colunas Ano, VD3005, V2009, V2010 do nosso data frame da amostra da pnad. O que o R retornará? Faça a mesma indexação usando inteiros positivos ou inteiros negativos.


```r
df[800, c(1, 8, 6, 7)]
```

```
##      Ano VD2003 V2009  V2010
## 800 2021      3    45 Branca
```

```r
df[-c(1:799, 801:1000), -c(2:5, 9, 10)]
```

```
##      Ano V2009  V2010 VD2003
## 800 2021    45 Branca      3
```

Aqui, o objetivo era praticar a seleção de linhas e colunas usando inteiros positivos e negativos. É meio trabalhoso e nem sempre é a forma mais eficiente, mas é ocasionalmente útil. Note o uso de vetores tanto no caso de seleções positivas quanto de negativas e de sequências com `:` para facilitar a digitação.

#### Diferentes seleções: `[[` e `$`

Assim como na lista, é possível usar os colchetes `[[ ]]` em data frame. Tente usá-los no data frame da pnad. Quais são as diferenças encontradas?


```r
df[[1]]
```

```
##    [1] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [15] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [29] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [43] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [57] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [71] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [85] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [99] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##  [113] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##  [127] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
....
Saída truncada para visualização
```

```r
df[[6]]
```

```
##    [1]  80  19  36  50  43  35  72  23  23  47  71  16  48  13  25   3  27  83
##   [19]  55  79  14  80  20  21  22  69  57  27  63  35  28  41  39  71   7  45
##   [37]  11   9  48  14  48  47   5  36  25  55   8  64   7  58  30   1  18  44
##   [55]  45  17   9  24  11  11  71  75  49  20  55  31  60   2   7  81  68  15
##   [73]  31   2  44  40  49  55   9  40  55   0  35  32   7  63  62  54  48   5
##   [91]  46  49  20  90  16   0  41  15  67  26  22  18  58  63   0  31  14  39
##  [109]  34  44  61  48  47  29  43  65  10  36  20   7  28  18  23  75  37  49
##  [127]  45   3  61  54  38  50  41  47  39  44  32  27  66  55  70  69  41  31
##  [145]  38  91  62  30  21  59   4  29   5  24  83   6  15  38  20  89  85  44
##  [163]  73   7  49  22  33   5  18   4  27  22   0  21  74  26  58  52  77  36
....
Saída truncada para visualização
```

```r
df[["Ano"]]
```

```
##    [1] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [15] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [29] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [43] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [57] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [71] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [85] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [99] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##  [113] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##  [127] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
....
Saída truncada para visualização
```

```r
df[["V2009"]]
```

```
##    [1]  80  19  36  50  43  35  72  23  23  47  71  16  48  13  25   3  27  83
##   [19]  55  79  14  80  20  21  22  69  57  27  63  35  28  41  39  71   7  45
##   [37]  11   9  48  14  48  47   5  36  25  55   8  64   7  58  30   1  18  44
##   [55]  45  17   9  24  11  11  71  75  49  20  55  31  60   2   7  81  68  15
##   [73]  31   2  44  40  49  55   9  40  55   0  35  32   7  63  62  54  48   5
##   [91]  46  49  20  90  16   0  41  15  67  26  22  18  58  63   0  31  14  39
##  [109]  34  44  61  48  47  29  43  65  10  36  20   7  28  18  23  75  37  49
##  [127]  45   3  61  54  38  50  41  47  39  44  32  27  66  55  70  69  41  31
##  [145]  38  91  62  30  21  59   4  29   5  24  83   6  15  38  20  89  85  44
##  [163]  73   7  49  22  33   5  18   4  27  22   0  21  74  26  58  52  77  36
....
Saída truncada para visualização
```

Os colchetes duplos `[[` funcionam no data frame porque ele é, por debaixo do capô, uma lista. Eles retornam os vetores que compõem o nosso data frame e permitem a realização de operações com vetores. São diferentes dos colchetes simples `[`, que retornarão listas/data frames menores, contendo apenas as partes que você selecionar. Lembrem-se da metáfora do trem!

### Modificando valores

#### Modificando os valores diretamente

Considere o vetor abaixo. Atribua um novo valor na posição 3 deste vetor. É possível atribuir uma string no lugar de um número?


```r
vetor_x<- c(10,30,50,10)

vetor_x[3] <- 70

vetor_x
```

```
## [1] 10 30 70 10
```

A atribuição em si é um processo relativamente simples. Vamos ver o que acontece se atribuímos um texto.


```r
vetor_x[3] <- "Texto"

vetor_x
```

```
## [1] "10"    "30"    "Texto" "10"
```

R usa suas regras de coerção para preservar as informações e transforma os valores numéricos em texto.

#### Modificando com testes lógicos

Extraia a coluna `V2010` do `df2` e teste os valores igual a 'Branca'. Além disso conte quantas linhas são iguais a 'Branca'. Dica: use a função sum() para fazer a contagem. 


```r
# Cópia de df
df2 <- df

sum(df2$V2010 == "Branca")
```

```
## [1] 407
```

Usamos um teste lógico no vetor `df2$V2010` para criar um vetor de valores lógicos `TRUE/FALSE` e as regras de coerção do R para fazer a soma dos valores verdadeiros `1` e descobrimos que 407 pessoas da amostra são brancas.

#### Valores desconhecidos

Conte o número de NAs na coluna `VD4016` do data frame `df2`.


```r
sum(is.na(df2$VD4016))
```

```
## [1] 613
```

Usando a mesma lógica do exercício anterior, porém, usando a função `is.na()` no lugar de `==` por causa do comportamento especial de valores `NA` em R.

### Revisão

Para os próximos exercícios considere o data frame da pnad que estamos usando nas aulas.

1. Como você selecionaria o mesmo valor abaixo, mas usando inteiros negativos na seleção?


```r
df2[1,5]
```

```
## [1] "Homem"
```

```r
df2[-(2:1000), -c(1:4, 6:20)]
```

```
## [1] "Homem"
```

Exercício maroto, apenas para vocês botarem a cabeça para trabalhar e lembrarem de usar `:` ao invés de digitar milhares de números.

2. Qual a diferença em usar em selecionar determinada informação em um data frame usando colchetes duplos `[[]]` e usando colchetes simples? Observe o exemplo abaixo para responder:


```r
df2[['Ano']]
```

```
##    [1] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [15] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [29] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [43] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [57] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [71] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [85] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##   [99] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##  [113] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
##  [127] 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021 2021
....
Saída truncada para visualização
```

```r
df['Ano']
```

```
##       Ano
## 1    2021
## 2    2021
## 3    2021
## 4    2021
## 5    2021
## 6    2021
## 7    2021
## 8    2021
## 9    2021
....
Saída truncada para visualização
```

A diferença é o tipo da saída do R e o que você pode fazer com ela:


```r
# Vetor!
typeof(df2[['Ano']])
```

```
## [1] "integer"
```

```r
class(df2[['Ano']])
```

```
## [1] "integer"
```

```r
# Lista/data.frame!
typeof(df2['Ano'])
```

```
## [1] "list"
```

```r
class(df2['Ano'])
```

```
## [1] "data.frame"
```

```r
# Funciona!
mean(df2[['Ano']])
```

```
## [1] 2021
```

```r
# Erro!
mean(df2['Ano'])
```

```
## Warning in mean.default(df2["Ano"]): argumento não é numérico nem lógico:
## retornando NA
```

```
## [1] NA
```

3. Considere o data frame abaixo:


```r
sala <- data.frame(
   id = c(1, 2, 3, 4, 5, 6),
   idade = c(20, 25, 30, 35, 40, 45),
   nome = c("Fulano", "Cicrano", "Beltrano", "Herculano", "Mariano", "Carrano"),
   sexo = "Masculino",
   origem = c("Campinas", "Barueri", "Monte Verde", "Rio de Janeiro", "Natal", "Belo Horizonte")
)
```

Modifique o nome de 'Mariano' por 'Mariana'. E altere a variável sexo para que seja um vetor que altere o valor para 'Feminino' na posição em que o nome foi trocado para Mariana.  


```r
# Ver a informação
sala[sala$nome == "Mariano", "nome"]
```

```
## [1] "Mariano"
```

```r
sala[sala$nome == "Mariana", "sexo"]
```

```
## character(0)
```

```r
# Modificar a informação
sala[sala$nome == "Mariano", "nome"] <- "Mariana"
sala[sala$nome == "Mariana", "sexo"] <- "Feminino"

# Checar o resultado
sala
```

```
##   id idade      nome      sexo         origem
## 1  1    20    Fulano Masculino       Campinas
## 2  2    25   Cicrano Masculino        Barueri
## 3  3    30  Beltrano Masculino    Monte Verde
## 4  4    35 Herculano Masculino Rio de Janeiro
## 5  5    40   Mariana  Feminino          Natal
## 6  6    45   Carrano Masculino Belo Horizonte
```

Vocês podiam utilizar uma seleção por números inteiros, porque o banco é pequeno, mas optei por fazer o exercício usando testes lógicos, porque é um caso mais geral e serve para mais de uma posição.

4. Considere o mesmo data frame sala construído no exercício anterior. Usando testes lógicos e operadores Booleanos. Verifique:  

   a. se existe algum aluno que seja do sexo Masculino e seja de São Paulo.  
   b. se existe algum aluno que tem idade maior ou igual a 20 anos ou que seja de Campinas. 
   c. se existe alguém com o nome Beltrano.  


```r
# a. se existe algum aluno que seja do sexo Masculino e seja de São Paulo.
sala[sala$origem == "São Paulo", ]
```

```
## [1] id     idade  nome   sexo   origem
## <0 linhas> (ou row.names de comprimento 0)
```

```r
# b. se existe algum aluno que tem idade maior ou igual a 20 anos ou que seja de Campinas. 
sala[sala$idade >= 20 & sala$origem == "Campinas", ]
```

```
##   id idade   nome      sexo   origem
## 1  1    20 Fulano Masculino Campinas
```

```r
# c. se existe alguém com o nome Beltrano.
sala[sala$nome == "Beltrano", ]
```

```
##   id idade     nome      sexo      origem
## 3  3    30 Beltrano Masculino Monte Verde
```

Exercício para exercitar os músculos de testes lógicos dentro de bancos de dados!

5. Agora considere o seguinte data frame e, usando as funções apropriadas ensinadas na aula de hoje, responda:  


```r
cadastro <- data.frame(
   id     = c(10, 105, 299, 645, 7907, 8660, 4992, 630),
   idade  = c(20, 25, 30, 35, 40, 45, 60, 53),
   nome   = c("Thais", NA, "Guilherme", "Betânia", "Ana Lucia", NA, NA, "Pâmela"),
   sexo   = c("Feminino", "Masculino","Masculino", 
              NA ,NA, "Feminino", "Masculino", "Feminino"),
   origem = c("Campinas", NA, "Monte Verde", "Rio de Janeiro",
              NA, "Belo Horizonte","São Paulo", "Rio de Janeiro")
)
```

   1. Quantos NAs existe na coluna nome?
   2. Quantos NAs existe na coluna sexo?
   3. Quantos NAs existe na coluna origem?
   4. Substitua os NAs na coluna sexo pelo valor 'Feminino'
   

```r
# 1. Quantos NAs existem na coluna nome?
sum(is.na(cadastro$nome))
```

```
## [1] 3
```

```r
# 2. Quantos NAs existem na coluna sexo?
sum(is.na(cadastro$sexo))
```

```
## [1] 2
```

```r
# 3. Quantos NAs existem na coluna origem?
sum(is.na(cadastro$origem))
```

```
## [1] 2
```

```r
# 4. Substitua os NAs na coluna sexo pelo valor 'Feminino'

     # Ver os dados
     cadastro[is.na(cadastro$sexo), ]
```

```
##     id idade      nome sexo         origem
## 4  645    35   Betânia <NA> Rio de Janeiro
## 5 7907    40 Ana Lucia <NA>           <NA>
```

```r
     # Modificar
     cadastro[is.na(cadastro$origem), "sexo"] <- "Feminino"

     # Ver o resultado
     cadastro
```

```
##     id idade      nome      sexo         origem
## 1   10    20     Thais  Feminino       Campinas
## 2  105    25      <NA>  Feminino           <NA>
## 3  299    30 Guilherme Masculino    Monte Verde
## 4  645    35   Betânia      <NA> Rio de Janeiro
## 5 7907    40 Ana Lucia  Feminino           <NA>
## 6 8660    45      <NA>  Feminino Belo Horizonte
## 7 4992    60      <NA> Masculino      São Paulo
## 8  630    53    Pâmela  Feminino Rio de Janeiro
```
   

6. Salve o data frame 'cadastro' do exercicio anterior em um arquivo `.csv`. Como você faria isso? Quais os argumentos que devem ser usados?


```r
write.csv(cadastro, "cadastro.csv", row.names = FALSE)
```

Só para relembrar o uso da função `write.csv`. Não deixem de consultar também as funções `?read.table`, `?write.table`, `?read.csv2`, `?write.csv2`, `?read.fwf` e `?write.fwf`. Essas funções permitem ler a maior parte dos dados em formato texto que vocês encontrarão nas suas vidas como analistas ou pesquisadores, que utilizam convenções como separadores de colunas (`;`, `,`, `\t`, `" "`), separadores de casas decimais (`,`, `.`), caracteres de escape (`\`, `"`), codificação de caracteres acentuados ("UTF-8", "Windows-1252", etc.) e outras questões similares.

_____________________________________________________
