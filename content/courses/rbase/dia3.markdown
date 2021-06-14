---
date: 2021-06-16
linkTitle: Estatística e Visualização
summary: Funções para estatísticas descritivas e gráficos em R
title: "Estatística e visualização: analisando seus dados e obtendo resultados"
type: book
draft: true
weight: 30
---

## Introdução

Sempre que você obtém um novo conjunto de dados para examinar, uma das primeiras tarefas que você precisa fazer é encontrar maneiras de resumir os dados de uma forma compacta e de fácil compreensão. É disso que se trata a estatística descritiva. Nesta aula vamos aprender funções de estatística descritiva usando R e fazer gráficos usando o pacote stat de gráficos básicos. 

Para esta aula vamos usar o mesmo data frame da pnad que olhamos nas aulas anteriores. Primeiro vamos importar (você aprendeu como fazer isso na aula anterior, lembra?):


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

## Estatística descritiva em R
A fim de ter uma ideia sobre o que está acontecendo no data frame, precisamos calcular algumas estatísticas descritivas e desenhar alguns gráficos. 

### Somas

Para calcular a soma, a função mais usada é `sum()`


```r
vetor<- c(1,3,2,5,6,10)

sum(vetor)
```

```
## [1] 27
```

```r
sum(vetor[2:4])
```

```
## [1] 10
```

A função permite que você some um vetor específico, ou posições dentro deste vetor. 

#### Exercício
Some as primeiras três posições da variável V2009  do data frame `df`


### na.rm
Caso os dados tenham NA, será necessário usar o argumento `na.rm = TRUE` por que o R, caso tenha NA, a soma dos elementos será o NA.


```r
vetor <- c(2, 5, 10, 28, NA)

sum(vetor[3:5])
```

```
## [1] NA
```

```r
sum(vetor[3:5], na.rm = TRUE)
```

```
## [1] 38
```

#### Exercício
Some as primeiras 5 observações da variável VD4016 do data frame df. O que aconteceu? Foi necessário usar na.rm?


### Contagens


#### Exercício

### Médias

Entre as medidas de tendência central, temos a média, calculada no R pela função `mean()`


```r
vetor<- c(20,10,30,4085,1292)

mean(vetor)
```

```
## [1] 1087.4
```

```r
mean(vetor[2:4])
```

```
## [1] 1375
```

A função `mean()` também permite calcular a média especificando a posição das observações que você quer no cálculo.

#### Média "trimmed"
Quando nos deparamos com casos em os dados podem ser suspeitos, como este por exemplo: -15,2,3,4,5,6,7,8,9,12 o valor -15 é meio suspeito, porém não conseguimos saber se é verdadeiro ou não. A média é altamente sensível a um ou dois valores extremos e, portanto, não é considerado uma medida robusta. Uma forma de resolver é usado a média trimmed. Para calcular este tipo de média, o que acontece é que você descarta os valores mais extremos nas duas pontas do vetor (o maior e o menor) e depois tira a média de todo o resto. 

Geralmente, descrevemos uma média trimmed em termos da porcentagem de observação de cada lado que é descartada. Por exemplo, uma média trimmed de 10% descarta os maiores 10% das observações e os menores 10% e, em seguida, toma a média dos 80% restantes das observações.


```r
vetor<- c(-15,2,3,4,5,6,7,8,9,12)

mean(vetor)
```

```
## [1] 4.1
```

```r
mean(vetor, trim = 0.1)
```

```
## [1] 5.5
```

```r
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
Se eu pegar uma média trimmed de 10%, deixaremos os valores extremos de cada lado e pegaremos a média do resto como foi feita acima usando o argumento `trim = 0.1`

#### Exercício
Calcule a média da variável V2009 do data frame df. Calcule a média e média trimmed. Quais são as principais diferenças?

### Medianas

A segunda medida de tendência central que se usa muito é a mediana. A mediana de um conjunto de observações é apenas o valor médio. A funcão do R que calcula a mediana é `median()`


```r
vetor<- c(20,30, 50, 47,59)

median(vetor)
```

```
## [1] 47
```

#### Exercício
Calcule a mediana da variável V2009 do data frame df. Quais as diferenças encontradas em relação a média calculada no exercício anterior?

### Amplitude

A amplitude é uma das medidas de dispersão calculada pelo R através da função `range()`. Além dela, podemos ver através do R, o valor máximo através da função `max()` e o valor mínimo através da função `min()` de um vetor de valores.
A amplitude basicamente calcula o valor máximo menos o valor mínimo de um vetor. 


```r
vetor<- c(20,30,60,80,17)

range(vetor)
```

```
## [1] 17 80
```

Podemos calcular esses valores também usando a função max() e minimo separado


```r
vetor<- c(20,30,60,80,17)

max(vetor)
```

```
## [1] 80
```

```r
min(vetor)
```

```
## [1] 17
```

#### Exercício
Calcule o valor máximo, mínimo e a amplitude da variável V2009 do data frame df. 

### Percentis
A divisão dos conjunto de dados em percentuais é o que se denomina de percentil. Os percentis mais famosos são justamente o 1º quartil ( = percentil 25%), o 2º quartil ( = percentil 50% = mediana), e o 3º quartil (= percentil 75%). Para calcular o percentil no R, usa-se a função `quantile()` o argumento `probs` permite o calculo dos percentis que você quer calcular. 


```r
quantile(df$V2009, probs = c(.25, .75))
```

```
## 25% 75% 
##  18  55
```

### Distância interquartil
A distância interquartil é uma medida de dispersão estatística, sendo igual à diferença entre o quartil superior (Q3) e quartil inferior (Q1). No R, a função que calcula o interquatil é `IQR()`.


```r
a <- c(1, 1.2, 1.5, 1.7, 1.8, 1.9, 2.2, 2.3, 2.6, 2.7, 8)

IQR(a)
```

```
## [1] 0.85
```

#### Exercício
Calcule o interquatil da variável V2009 do data frame df da pnad. Que valor você encontrou? Compare com os valores encontrados no percentil 25% e 75%. 

### Variância
A variância de um conjunto de valores é calculada pelo R usando a função `var()`.


```r
b <- c(1, 2, 2.5, 4, 4.5, 5.5, 6, 6.4, 7, 7.5, 8)

var(b)
```

```
## [1] 5.492727
```

#### Exercício

Calcule a variância da variável V2009 do data frame df.

### Desvio padrão

Pegue a raiz quadrada da variância e teremos mais uma medida conhecida como desvio padrão. Para calcular o desvio padrão no R, usamos a função `sd()` (Standard Deviation).

```r
a <- c(1, 1.2, 1.5, 1.7, 1.8, 1.9, 2.2, 2.3, 2.6, 2.7, 8)

sd(a)
```

```
## [1] 1.919043
```

#### Exercício
Calcule o desvio padrão da variável V2009 do data frame df.

### Desvio absoluto da mediana
Cada observação no conjunto de dados fica a alguma distância do valor típico (a mediana). Portanto, o desvio absoluto da mediana(median absolute deviation - MAD) é uma tentativa de descrever o desvio atípico de um valor típico no conjunto de dados. A função que calcula essa medida pelo R chama-se `mad()`


```r
a <- c(1, 1.2, 1.5, 1.7, 1.8, 1.9, 2.2, 2.3, 2.6, 2.7, 8)

mad(a)
```

```
## [1] 0.59304
```

#### Exercício
Calcule o Desvio absoluto da mediana da variável V2009 do data frame df.

### Sumário
Existe uma função do R que capaz de calcular as principais medidas apresentadas de acima, de uma vez, em uma única função. Dessa forma, é muito mais prático as funções que sumarizam algumas medidas estatísticas principais `summary()` e `describe()`


```r
summary(df$V2009)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    0.00   18.00   36.00   36.94   55.00  104.00
```

```r
summary(df$V2010)
```

```
##    Length     Class      Mode 
##      1000 character character
```

A função `summary()` se comporta de maneiras diferentes dependendo do tipo de variável, por exemplo, na variável V2009 numérica, a função `summary()` retorna as medidas: min, 1º quartil, mediana, média, 3º quartil e max. Na variável V2010, categórica, a função `summary()` retorna a frequência absoluta de cada categoria. É possível fazer o summary de um data frame inteiro de uma vez. 


```r
summary(df)
```

```
##       Ano         Trimestre      UF               V1022          
##  Min.   :2021   Min.   :1   Length:1000        Length:1000       
##  1st Qu.:2021   1st Qu.:1   Class :character   Class :character  
##  Median :2021   Median :1   Mode  :character   Mode  :character  
##  Mean   :2021   Mean   :1                                        
##  3rd Qu.:2021   3rd Qu.:1                                        
##  Max.   :2021   Max.   :1                                        
##                                                                  
##     V2007               V2009           V2010               VD2003      
##  Length:1000        Min.   :  0.00   Length:1000        Min.   : 1.000  
##  Class :character   1st Qu.: 18.00   Class :character   1st Qu.: 2.000  
##  Mode  :character   Median : 36.00   Mode  :character   Median : 3.000  
##                     Mean   : 36.94                      Mean   : 3.609  
##                     3rd Qu.: 55.00                      3rd Qu.: 4.000  
##                     Max.   :104.00                      Max.   :11.000  
##                                                                         
##     VD3005              VD4016     
##  Length:1000        Min.   :   50  
##  Class :character   1st Qu.: 1000  
##  Mode  :character   Median : 1262  
##                     Mean   : 2001  
##                     3rd Qu.: 2300  
##                     Max.   :19000  
##                     NA's   :613
```

### Estatísticas por grupo
É muito comum que você precise olhar para estatísticas descritivas, divididas por alguma variável de agrupamento
Isso é muito fácil de fazer no R e há duas funções em particular que vale a pena conhecer: `by()` e `aggregate()`.

A função `by()` mostra algo semelhante a função `summary()` porém divido por grupo especificado no argumento INDICES.


```r
by(data=df, INDICES=df$V1022, FUN=summary)
```

```
## df$V1022: Rural
##       Ano         Trimestre      UF               V1022          
##  Min.   :2021   Min.   :1   Length:257         Length:257        
##  1st Qu.:2021   1st Qu.:1   Class :character   Class :character  
##  Median :2021   Median :1   Mode  :character   Mode  :character  
##  Mean   :2021   Mean   :1                                        
##  3rd Qu.:2021   3rd Qu.:1                                        
##  Max.   :2021   Max.   :1                                        
##                                                                  
##     V2007               V2009           V2010               VD2003      
##  Length:257         Min.   :  0.00   Length:257         Min.   : 1.000  
##  Class :character   1st Qu.: 18.00   Class :character   1st Qu.: 2.000  
##  Mode  :character   Median : 35.00   Mode  :character   Median : 3.000  
##                     Mean   : 37.01                      Mean   : 3.763  
##                     3rd Qu.: 54.00                      3rd Qu.: 5.000  
##                     Max.   :104.00                      Max.   :11.000  
##                                                                         
##     VD3005              VD4016    
##  Length:257         Min.   :  50  
##  Class :character   1st Qu.: 500  
##  Mode  :character   Median :1022  
##                     Mean   :1199  
##                     3rd Qu.:1500  
##                     Max.   :5800  
##                     NA's   :169   
## ------------------------------------------------------------ 
## df$V1022: Urbana
##       Ano         Trimestre      UF               V1022          
##  Min.   :2021   Min.   :1   Length:743         Length:743        
##  1st Qu.:2021   1st Qu.:1   Class :character   Class :character  
##  Median :2021   Median :1   Mode  :character   Mode  :character  
##  Mean   :2021   Mean   :1                                        
##  3rd Qu.:2021   3rd Qu.:1                                        
##  Max.   :2021   Max.   :1                                        
##                                                                  
##     V2007               V2009          V2010               VD2003      
##  Length:743         Min.   : 0.00   Length:743         Min.   : 1.000  
##  Class :character   1st Qu.:18.00   Class :character   1st Qu.: 2.000  
##  Mode  :character   Median :37.00   Mode  :character   Median : 3.000  
##                     Mean   :36.92                      Mean   : 3.556  
##                     3rd Qu.:55.00                      3rd Qu.: 4.000  
##                     Max.   :96.00                      Max.   :10.000  
##                                                                        
##     VD3005              VD4016     
##  Length:743         Min.   :   50  
##  Class :character   1st Qu.: 1100  
##  Mode  :character   Median : 1500  
##                     Mean   : 2237  
##                     3rd Qu.: 2700  
##                     Max.   :19000  
##                     NA's   :444
```
Neste exemplo, a função by mostra as medidas separadas pelas categorias de variáveis V1022, ou seja, mostra o summary para categoria Urbana e um summary para categoria Rural.

Para a `aggregate()` também é necessário especificar três argumentos. O argumento da fórmula é usado para indicar qual variável você deseja analisar e quais variáveis são usadas para especificar os grupos. 

```r
aggregate(formula = V2009 ~ V1022 + V2007,  data = df, FUN = mean)
```

```
##    V1022  V2007    V2009
## 1  Rural  Homem 35.60800
## 2 Urbana  Homem 36.13859
## 3  Rural Mulher 38.33333
## 4 Urbana Mulher 37.67733
```

### Escores e testes

<!-- p-valores, t-valores, f-valores e seus respectivos testes -->

#### Exercício

### Correlações

```r
cor(df$VD2003,df$V2009)
```

```
## [1] -0.3656368
```


#### Exercício

### complete.obs

```r
cor(df$VD2003,df$V2009, use = "complete.obs")
```

```
## [1] -0.3656368
```


#### Exercício

## Visualizações

### Diferentes sistemas

<!-- Contextualizar a diferença entre os gráficos básicos do pacote graphics e outros gráficos modernos construídos a partir do grid (lattice, ggplot2) -->

### Parâmetros gráficos vs argumentos de função

<!-- Mostrar que há dois tipos de argumentos nas funções de plotagem do graphics. Um deles são os argumentos próprios de cada função de plotagem, o outro são os argumentos gerais compartilhados por quase todas as funções de plotagem, e estão em par() -->

### Plot

Introdução ao scatterplot através de um exemplo de plotagem simples

### Tipo de gráfico

argumento type: "p", "l", "h", "o", "b", "s", "S", "c", "n"

#### Exercício

### Histogramas

hist

bins, breaks

interface de formula

#### Exercício

### Boxplots

boxplot

range

interface de formula

#### Exercício

### Diagramas de dispersão

plot com x e y

plot com fórmula e data.frame

#### Exercício

#### Adicionar linhas a um scatterplot

lines

#### Exercício

#### Matriz de scatterplots

pairs

interface de formula

### Barras

barplot

las = 2

#### Exercício

#### O primo pobre: gráficos de pizza

pie

#### Exercício

### Parâmetros globais

par

mar

mfrow

#### Exercício

### Salvando seus gráficos

#### Usando a interface do RStudio

#### Usando grDevices

dev.list

dev.print

png, jpeg, tiff

#### Exercício

### Customização de gráficos

<!-- Isso fica como referência para eles, não vai ter exercícios aqui -->

#### Títulos de gráficos e eixos

main, sub, xlab e ylab

#### Customização de fonte

estilos: font.main, font.sub, font.axis

cores: col.main, col.sub, col.axis

tamanho: cex.main, cex.sub, cex.axis

família: "sans", "serif", "mono"

#### Cores

argumento col

#### Forma

argumento pch

#### Tamanho

argumento cex

#### Tipo de linha

argumento lty: "blank", "solid", "dashed", "dotted", "dotdash", "longdash", "twodash"

#### Espessura de linha

argumento lwd

#### Eixos

##### Escalas dos eixos

xlim, ylim

##### Remover títulos

ann = FALSE

##### Remover eixos

axes = FALSE

##### Manter área de plotagem

frame.box = TRUE

#### Argumentos específicos de cada função

angle e shading

border

labels

boxplot parts: box, med, whisk, staple e out

horizontal

## Revisão

### Exercícios
