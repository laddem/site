---
title: "Soluções"
date: 2021-05-25
summary: Soluções dos exercícios
type: book
draft: false
---

Soluções dos exercícios.

## `readr`, `tibble`, `tidyr`


```r
# Não esqueça dos pacotes!
library(tidyverse)
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.3     v purrr   0.3.4
## v tibble  3.1.2     v dplyr   1.0.6
## v tidyr   1.1.3     v stringr 1.4.0
## v readr   1.4.0     v forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```


1. Como você importaria o banco "epa78.csv"


```r
file <- readr_example("epa78.txt")
```

Primeiro, é bom verificar como estão dispostas as informações no arquivo texto


```r
read_lines(file, n_max = 10)
```

```
##  [1] "ALFA ROMEO                                                                     ALFA ROMEO           78010003"
##  [2] "ALFETTA                              03  81  8    74  7   89  9                ALFETTA              78010053"
##  [3] "SPIDER 2000                          01                                        SPIDER 2000          78010103"
##  [4] "AMC                                                                            AMC                  78020002"
##  [5] "GREMLIN                              03  79  9                    79  9        GREMLIN              78020053"
##  [6] "PACER                                04  89 11                    89 11        PACER                78020103"
##  [7] "PACER WAGON                          07  90 26    91 26                        PACER WAGON          78020153"
##  [8] "CONCORD                              04  88 12    90 11   90 11   83 16        CONCORD              78020203"
##  [9] "CONCORD WAGON                        07  91 30            91 30                CONCORD WAGON        78020253"
## [10] "MATADOR COUPE                        05  97 14    97 14                        MATADOR COUPE        78020303"
```

Ao identificar que se trata de um arquivo colunado, mas que as colunas são separadas por espaços, escolho o read_fwf com o fwf_empty.


```r
dic <- fwf_empty(file)

df <- read_fwf(file, col_positions = dic)
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   X1 = col_character(),
##   X2 = col_character(),
##   X3 = col_double(),
##   X4 = col_double(),
##   X5 = col_double(),
##   X6 = col_double(),
##   X7 = col_double(),
##   X8 = col_double(),
##   X9 = col_double(),
##   X10 = col_double(),
##   X11 = col_character(),
##   X12 = col_double()
## )
```

```r
df
```

```
## # A tibble: 20 x 12
##    X1       X2       X3    X4    X5    X6    X7    X8    X9   X10 X11        X12
##    <chr>    <chr> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <chr>    <dbl>
##  1 ALFA RO~ <NA>     NA    NA    NA    NA    NA    NA    NA    NA ALFA R~ 7.80e7
##  2 ALFETTA  03       81     8    74     7    89     9    NA    NA ALFETTA 7.80e7
##  3 SPIDER ~ 01       NA    NA    NA    NA    NA    NA    NA    NA SPIDER~ 7.80e7
##  4 AMC      <NA>     NA    NA    NA    NA    NA    NA    NA    NA AMC     7.80e7
##  5 GREMLIN  03       79     9    NA    NA    NA    NA    79     9 GREMLIN 7.80e7
##  6 PACER    04       89    11    NA    NA    NA    NA    89    11 PACER   7.80e7
##  7 PACER W~ 07       90    26    91    26    NA    NA    NA    NA PACER ~ 7.80e7
##  8 CONCORD  04       88    12    90    11    90    11    83    16 CONCORD 7.80e7
##  9 CONCORD~ 07       91    30    NA    NA    91    30    NA    NA CONCOR~ 7.80e7
## 10 MATADOR~ 05       97    14    97    14    NA    NA    NA    NA MATADO~ 7.80e7
## 11 MATADOR~ 06      110    20    NA    NA   110    20    NA    NA MATADO~ 7.80e7
## 12 MATADOR~ 09      112    50    NA    NA   112    50    NA    NA MATADO~ 7.80e7
## 13 ASTON M~ <NA>     NA    NA    NA    NA    NA    NA    NA    NA ASTON ~ 7.80e7
## 14 ASTON M~ <NA>     NA    NA    NA    NA    NA    NA    NA    NA ASTON ~ 7.80e7
## 15 AUDI     <NA>     NA    NA    NA    NA    NA    NA    NA    NA AUDI    7.81e7
## 16 FOX      03       84    11    84    11    84    11    NA    NA FOX     7.81e7
## 17 FOX WAG~ 07       83    40    NA    NA    83    40    NA    NA FOX WA~ 7.81e7
## 18 5000     04       90    15    NA    NA    90    15    NA    NA 5000    7.81e7
## 19 AVANTI   <NA>     NA    NA    NA    NA    NA    NA    NA    NA AVANTI  7.81e7
## 20 AVANTI ~ 02       75     8    75     8    NA    NA    NA    NA AVANTI~ 7.81e7
```

2. Importe o banco "challenge.csv" e resolva os problemas com o tipo da coluna.

Ao verificar as primeiras 10 linhas do banco, podemos notar algo estranho


```r
file <- readr_example("challenge.csv")

read_lines(file, n_max = 10)
```

```
##  [1] "x,y"     "404,NA"  "4172,NA" "3004,NA" "787,NA"  "37,NA"   "2332,NA"
##  [8] "2489,NA" "1449,NA" "3665,NA"
```

Parece ser um arquivo csv comum, com duas colunas, mas uma delas parece ter apenas NAs. Se a gente proceder com a importação padrão, chegaremos em


```r
df <- read_csv(file)
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   x = col_double(),
##   y = col_logical()
## )
```

```
## Warning: 1000 parsing failures.
##  row col           expected     actual                                                                     file
## 1001   y 1/0/T/F/TRUE/FALSE 2015-01-16 'C:/Users/vinic/Documents/R/win-library/4.1/readr/extdata/challenge.csv'
## 1002   y 1/0/T/F/TRUE/FALSE 2018-05-18 'C:/Users/vinic/Documents/R/win-library/4.1/readr/extdata/challenge.csv'
## 1003   y 1/0/T/F/TRUE/FALSE 2015-09-05 'C:/Users/vinic/Documents/R/win-library/4.1/readr/extdata/challenge.csv'
## 1004   y 1/0/T/F/TRUE/FALSE 2012-11-28 'C:/Users/vinic/Documents/R/win-library/4.1/readr/extdata/challenge.csv'
## 1005   y 1/0/T/F/TRUE/FALSE 2020-01-13 'C:/Users/vinic/Documents/R/win-library/4.1/readr/extdata/challenge.csv'
## .... ... .................. .......... ........................................................................
## See problems(...) for more details.
```

```r
df
```

```
## # A tibble: 2,000 x 2
##        x y    
##    <dbl> <lgl>
##  1   404 NA   
##  2  4172 NA   
##  3  3004 NA   
##  4   787 NA   
##  5    37 NA   
##  6  2332 NA   
##  7  2489 NA   
##  8  1449 NA   
##  9  3665 NA   
## 10  3863 NA   
## # ... with 1,990 more rows
```


No console de vocês, deve aparecer que foram importadas as colunas x como double e y como logical. Mas uma chuva de "parsing failures", indicando que expected = 1/0/T/F/TRUE/FALSE, actual = 2015-01-16.

Na verdade, ao tentar adivinhar o tipo de colunas, o readr lê as primeiras 1000 observações em busca de um padrão. Você pode resolver esse problema:


```r
# 1. Aumentando o número de observações utilizadas para adivinhar as colunas
df <- read_csv(file, guess_max = 1001)
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   x = col_double(),
##   y = col_date(format = "")
## )
```

```r
# A específicação da coluna Y agora é <date>
df
```

```
## # A tibble: 2,000 x 2
##        x y         
##    <dbl> <date>    
##  1   404 NA        
##  2  4172 NA        
##  3  3004 NA        
##  4   787 NA        
##  5    37 NA        
##  6  2332 NA        
##  7  2489 NA        
##  8  1449 NA        
##  9  3665 NA        
## 10  3863 NA        
## # ... with 1,990 more rows
```

```r
# 2. escolhendo diretamente o tipo de coluna antes da importação
tipos <- cols(
  y = col_date()
)

df <- read_csv(file, col_types = tipos)

# Mesmo resultado
df
```

```
## # A tibble: 2,000 x 2
##        x y         
##    <dbl> <date>    
##  1   404 NA        
##  2  4172 NA        
##  3  3004 NA        
##  4   787 NA        
##  5    37 NA        
##  6  2332 NA        
##  7  2489 NA        
##  8  1449 NA        
##  9  3665 NA        
## 10  3863 NA        
## # ... with 1,990 more rows
```

3. Com o banco sala_aula dado a seguir, transforme-o para que ele contenha as variáveis nome, avaliação e nota.


```r
sala_aula <- tribble(
 ~name,    ~teste1,  ~teste2,  ~prova1,
 "Billy",  "<NA>",   "D"   ,   "C",
 "Suzy",   "F",      "<NA>",   "<NA>",
 "Lionel", "B",      "C"   ,   "B",
 "Jenny",  "A",      "A"   ,   "B"
)
```

É sempre bom começar planejando o banco que queremos construir. Queremos um banco que tenha 3 variáveis: o nome, o tipo de prova aplicada e a nota de cada aluno. Para isso, precisamos colocar os nomes das colunas teste1, teste2 e prova1 numa variável e os valores das células em outra. Vamos chamar essas colunas de "avaliação" e "nota", elas formam um par. 

Agora vamos chamar `pivot_wider` e especificar esses argumentos.


```r
sala_aula %>% pivot_longer(
  # Primeiro, identificamos as colunas que serão modificadas
  cols = c(teste1, teste2, prova1),
  # Agora, indicamos os nomes das colunas que receberão
  # os nomes das colunas transformadas
  names_to = "avaliacao",
  # os valores das células
  values_to = "nota"
)
```

```
## # A tibble: 12 x 3
##    name   avaliacao nota 
##    <chr>  <chr>     <chr>
##  1 Billy  teste1    <NA> 
##  2 Billy  teste2    D    
##  3 Billy  prova1    C    
##  4 Suzy   teste1    F    
##  5 Suzy   teste2    <NA> 
##  6 Suzy   prova1    <NA> 
##  7 Lionel teste1    B    
##  8 Lionel teste2    C    
##  9 Lionel prova1    B    
## 10 Jenny  teste1    A    
## 11 Jenny  teste2    A    
## 12 Jenny  prova1    B
```

4. Transforme o banco `relig_income` para que ele contenha as colunas religião, renda e frequência.


```r
relig_income
```

```
## # A tibble: 18 x 11
##    religion `<$10k` `$10-20k` `$20-30k` `$30-40k` `$40-50k` `$50-75k` `$75-100k`
##    <chr>      <dbl>     <dbl>     <dbl>     <dbl>     <dbl>     <dbl>      <dbl>
##  1 Agnostic      27        34        60        81        76       137        122
##  2 Atheist       12        27        37        52        35        70         73
##  3 Buddhist      27        21        30        34        33        58         62
##  4 Catholic     418       617       732       670       638      1116        949
##  5 Don’t k~      15        14        15        11        10        35         21
##  6 Evangel~     575       869      1064       982       881      1486        949
##  7 Hindu          1         9         7         9        11        34         47
##  8 Histori~     228       244       236       238       197       223        131
##  9 Jehovah~      20        27        24        24        21        30         15
## 10 Jewish        19        19        25        25        30        95         69
## 11 Mainlin~     289       495       619       655       651      1107        939
## 12 Mormon        29        40        48        51        56       112         85
## 13 Muslim         6         7         9        10         9        23         16
## 14 Orthodox      13        17        23        32        32        47         38
## 15 Other C~       9         7        11        13        13        14         18
## 16 Other F~      20        33        40        46        49        63         46
## 17 Other W~       5         2         3         4         2         7          3
## 18 Unaffil~     217       299       374       365       341       528        407
## # ... with 3 more variables: $100-150k <dbl>, >150k <dbl>,
## #   Don't know/refused <dbl>
```

O banco relig_income parece ter uma organização em que temos 2 variáveis, mas uma delas está numa coluna "religion" e a outra está em 10 colunas, "income". Queremos um banco que tenha 3 colunas: a religião, o nível de renda, e o número de pessoas em cada combinação das duas primeiras.

Como no exerício anterior, vamos chamar pivot_longer e especificar


```r
relig_income %>% pivot_longer(
  
  # As colunas a serem modificadas, notem o uso de ':' para selecionar várias
  # colunas em sequência
  cols = `<$10k`:`Don't know/refused`,
  
  # Variável recebe os nomes da antiga coluna
  names_to = "nivel_renda",
  
  # Variável recebe os valores das células
  values_to = "contagem"
  
)
```

```
## # A tibble: 180 x 3
##    religion nivel_renda        contagem
##    <chr>    <chr>                 <dbl>
##  1 Agnostic <$10k                    27
##  2 Agnostic $10-20k                  34
##  3 Agnostic $20-30k                  60
##  4 Agnostic $30-40k                  81
##  5 Agnostic $40-50k                  76
##  6 Agnostic $50-75k                 137
##  7 Agnostic $75-100k                122
##  8 Agnostic $100-150k               109
##  9 Agnostic >150k                    84
## 10 Agnostic Don't know/refused       96
## # ... with 170 more rows
```

5. Transforme o banco `billboard` para que ele contenha apenas uma coluna "semana" e uma coluna com a posição da música no ranking.

> Dica, você pode selecionar várias colunas usando o atalho wk1:wk76


```r
billboard
```

```
## # A tibble: 317 x 79
##    artist   track   date.entered   wk1   wk2   wk3   wk4   wk5   wk6   wk7   wk8
##    <chr>    <chr>   <date>       <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
##  1 2 Pac    Baby D~ 2000-02-26      87    82    72    77    87    94    99    NA
##  2 2Ge+her  The Ha~ 2000-09-02      91    87    92    NA    NA    NA    NA    NA
##  3 3 Doors~ Krypto~ 2000-04-08      81    70    68    67    66    57    54    53
##  4 3 Doors~ Loser   2000-10-21      76    76    72    69    67    65    55    59
##  5 504 Boyz Wobble~ 2000-04-15      57    34    25    17    17    31    36    49
##  6 98^0     Give M~ 2000-08-19      51    39    34    26    26    19     2     2
##  7 A*Teens  Dancin~ 2000-07-08      97    97    96    95   100    NA    NA    NA
##  8 Aaliyah  I Don'~ 2000-01-29      84    62    51    41    38    35    35    38
##  9 Aaliyah  Try Ag~ 2000-03-18      59    53    38    28    21    18    16    14
## 10 Adams, ~ Open M~ 2000-08-26      76    76    74    69    68    67    61    58
## # ... with 307 more rows, and 68 more variables: wk9 <dbl>, wk10 <dbl>,
## #   wk11 <dbl>, wk12 <dbl>, wk13 <dbl>, wk14 <dbl>, wk15 <dbl>, wk16 <dbl>,
## #   wk17 <dbl>, wk18 <dbl>, wk19 <dbl>, wk20 <dbl>, wk21 <dbl>, wk22 <dbl>,
## #   wk23 <dbl>, wk24 <dbl>, wk25 <dbl>, wk26 <dbl>, wk27 <dbl>, wk28 <dbl>,
## #   wk29 <dbl>, wk30 <dbl>, wk31 <dbl>, wk32 <dbl>, wk33 <dbl>, wk34 <dbl>,
## #   wk35 <dbl>, wk36 <dbl>, wk37 <dbl>, wk38 <dbl>, wk39 <dbl>, wk40 <dbl>,
## #   wk41 <dbl>, wk42 <dbl>, wk43 <dbl>, wk44 <dbl>, wk45 <dbl>, wk46 <dbl>,
## #   wk47 <dbl>, wk48 <dbl>, wk49 <dbl>, wk50 <dbl>, wk51 <dbl>, wk52 <dbl>,
## #   wk53 <dbl>, wk54 <dbl>, wk55 <dbl>, wk56 <dbl>, wk57 <dbl>, wk58 <dbl>,
## #   wk59 <dbl>, wk60 <dbl>, wk61 <dbl>, wk62 <dbl>, wk63 <dbl>, wk64 <dbl>,
## #   wk65 <dbl>, wk66 <lgl>, wk67 <lgl>, wk68 <lgl>, wk69 <lgl>, wk70 <lgl>,
## #   wk71 <lgl>, wk72 <lgl>, wk73 <lgl>, wk74 <lgl>, wk75 <lgl>, wk76 <lgl>
```

Da mesma forma como fizemos nos anteriores, queremos transformar as várias wk1:wk76 em um par de colunas, uma que me diga a semana e a outra que me diga em que posição no ranking a música estava.


```r
billboard %>% pivot_longer(
  # Colunas que serão transformadas
  cols = wk1:wk76,
  # Nome da variável que receberá os nomes das colunas
  names_to = "semana",
  # Nome da variável que receberá os valores das células
  values_to = "posicao_rank",
  # Nesse caso, uso o argumento opcional para eliminar os NAs
  values_drop_na = TRUE
  # Experimente mudar este argumento para FALSE e veja o resultado
  # Quando uma música não está mais nas paradas, ela recebe NA. Acho
  # justificado excluir os NAs nesse caso.
)
```

```
## # A tibble: 5,307 x 5
##    artist  track                   date.entered semana posicao_rank
##    <chr>   <chr>                   <date>       <chr>         <dbl>
##  1 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk1              87
##  2 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk2              82
##  3 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk3              72
##  4 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk4              77
##  5 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk5              87
##  6 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk6              94
##  7 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk7              99
##  8 2Ge+her The Hardest Part Of ... 2000-09-02   wk1              91
##  9 2Ge+her The Hardest Part Of ... 2000-09-02   wk2              87
## 10 2Ge+her The Hardest Part Of ... 2000-09-02   wk3              92
## # ... with 5,297 more rows
```

6. Experimente fazer o caminho inverso dos exercícios 3 a 5, devolvendo os datasets ao seu formato original. O que você observou?

Vou começar enxugando os códigos anteriores para criar os resultados que produzimos e salvá-los em objetos.


```r
sala_aula_long <- 
  sala_aula %>% pivot_longer(
    cols = c(teste1, teste2, prova1),
    names_to = "avaliacao",
    values_to = "nota"
  )

relig_income_long <- 
  relig_income %>% pivot_longer(
    cols = `<$10k`:`Don't know/refused`,
    names_to = "nivel_renda",
    values_to = "contagem"
  )

billboard_long <- 
  billboard %>% pivot_longer(
    cols = wk1:wk76,
    names_to = "semana",
    values_to = "posicao_rank",
    values_drop_na = TRUE
  )

sala_aula_long
```

```
## # A tibble: 12 x 3
##    name   avaliacao nota 
##    <chr>  <chr>     <chr>
##  1 Billy  teste1    <NA> 
##  2 Billy  teste2    D    
##  3 Billy  prova1    C    
##  4 Suzy   teste1    F    
##  5 Suzy   teste2    <NA> 
##  6 Suzy   prova1    <NA> 
##  7 Lionel teste1    B    
##  8 Lionel teste2    C    
##  9 Lionel prova1    B    
## 10 Jenny  teste1    A    
## 11 Jenny  teste2    A    
## 12 Jenny  prova1    B
```

```r
relig_income_long
```

```
## # A tibble: 180 x 3
##    religion nivel_renda        contagem
##    <chr>    <chr>                 <dbl>
##  1 Agnostic <$10k                    27
##  2 Agnostic $10-20k                  34
##  3 Agnostic $20-30k                  60
##  4 Agnostic $30-40k                  81
##  5 Agnostic $40-50k                  76
##  6 Agnostic $50-75k                 137
##  7 Agnostic $75-100k                122
##  8 Agnostic $100-150k               109
##  9 Agnostic >150k                    84
## 10 Agnostic Don't know/refused       96
## # ... with 170 more rows
```

```r
billboard_long
```

```
## # A tibble: 5,307 x 5
##    artist  track                   date.entered semana posicao_rank
##    <chr>   <chr>                   <date>       <chr>         <dbl>
##  1 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk1              87
##  2 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk2              82
##  3 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk3              72
##  4 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk4              77
##  5 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk5              87
##  6 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk6              94
##  7 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk7              99
##  8 2Ge+her The Hardest Part Of ... 2000-09-02   wk1              91
##  9 2Ge+her The Hardest Part Of ... 2000-09-02   wk2              87
## 10 2Ge+her The Hardest Part Of ... 2000-09-02   wk3              92
## # ... with 5,297 more rows
```

O caminho inverso desses bancos de dados, é utilizar `pivot_wider`. Aqui, vamos escolher um par de colunas que contém:

1. O nome das colunas que queremos criar
2. O valor que queremos passar para as células dessas novas colunas

Vamos ver exemplos comentados como no anterior


```r
sala_aula_long %>% pivot_wider(
  
  # Aqui, identificamos colunas que NÃO SERÃO MODIFICADAS
  # É o contrário de pivot_longer. Por padrão, são todas as que não são
  # mencionadas na transformação, mas para deixar bem claro, 
  # vou deixar explícito.
  id_cols = name,
  
  # Variável com os nomes para as novas colunas
  names_from = avaliacao,
  
  # Variável com os valores para as células
  values_from = nota
)
```

```
## # A tibble: 4 x 4
##   name   teste1 teste2 prova1
##   <chr>  <chr>  <chr>  <chr> 
## 1 Billy  <NA>   D      C     
## 2 Suzy   F      <NA>   <NA>  
## 3 Lionel B      C      B     
## 4 Jenny  A      A      B
```

```r
relig_income_long %>% pivot_wider(
  # Colunas não modificadas
  id_cols = religion,
  
  # Variável com os nomes para as novas colunas
  names_from = nivel_renda,
  
  # Variável com os valores para as células
  values_from = contagem
)
```

```
## # A tibble: 18 x 11
##    religion `<$10k` `$10-20k` `$20-30k` `$30-40k` `$40-50k` `$50-75k` `$75-100k`
##    <chr>      <dbl>     <dbl>     <dbl>     <dbl>     <dbl>     <dbl>      <dbl>
##  1 Agnostic      27        34        60        81        76       137        122
##  2 Atheist       12        27        37        52        35        70         73
##  3 Buddhist      27        21        30        34        33        58         62
##  4 Catholic     418       617       732       670       638      1116        949
##  5 Don’t k~      15        14        15        11        10        35         21
##  6 Evangel~     575       869      1064       982       881      1486        949
##  7 Hindu          1         9         7         9        11        34         47
##  8 Histori~     228       244       236       238       197       223        131
##  9 Jehovah~      20        27        24        24        21        30         15
## 10 Jewish        19        19        25        25        30        95         69
## 11 Mainlin~     289       495       619       655       651      1107        939
## 12 Mormon        29        40        48        51        56       112         85
## 13 Muslim         6         7         9        10         9        23         16
## 14 Orthodox      13        17        23        32        32        47         38
## 15 Other C~       9         7        11        13        13        14         18
## 16 Other F~      20        33        40        46        49        63         46
## 17 Other W~       5         2         3         4         2         7          3
## 18 Unaffil~     217       299       374       365       341       528        407
## # ... with 3 more variables: $100-150k <dbl>, >150k <dbl>,
## #   Don't know/refused <dbl>
```

```r
billboard_long %>% pivot_wider(
  # Colunas não modificadas
  id_cols = c(artist, track, date.entered),
  
  # Variável com os nomes para as novas colunas
  names_from = semana,
  
  # Variável com os valores para as células
  values_from = posicao_rank
)
```

```
## # A tibble: 317 x 68
##    artist   track   date.entered   wk1   wk2   wk3   wk4   wk5   wk6   wk7   wk8
##    <chr>    <chr>   <date>       <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
##  1 2 Pac    Baby D~ 2000-02-26      87    82    72    77    87    94    99    NA
##  2 2Ge+her  The Ha~ 2000-09-02      91    87    92    NA    NA    NA    NA    NA
##  3 3 Doors~ Krypto~ 2000-04-08      81    70    68    67    66    57    54    53
##  4 3 Doors~ Loser   2000-10-21      76    76    72    69    67    65    55    59
##  5 504 Boyz Wobble~ 2000-04-15      57    34    25    17    17    31    36    49
##  6 98^0     Give M~ 2000-08-19      51    39    34    26    26    19     2     2
##  7 A*Teens  Dancin~ 2000-07-08      97    97    96    95   100    NA    NA    NA
##  8 Aaliyah  I Don'~ 2000-01-29      84    62    51    41    38    35    35    38
##  9 Aaliyah  Try Ag~ 2000-03-18      59    53    38    28    21    18    16    14
## 10 Adams, ~ Open M~ 2000-08-26      76    76    74    69    68    67    61    58
## # ... with 307 more rows, and 57 more variables: wk9 <dbl>, wk10 <dbl>,
## #   wk11 <dbl>, wk12 <dbl>, wk13 <dbl>, wk14 <dbl>, wk15 <dbl>, wk16 <dbl>,
## #   wk17 <dbl>, wk18 <dbl>, wk19 <dbl>, wk20 <dbl>, wk21 <dbl>, wk22 <dbl>,
## #   wk23 <dbl>, wk24 <dbl>, wk25 <dbl>, wk26 <dbl>, wk27 <dbl>, wk28 <dbl>,
## #   wk29 <dbl>, wk30 <dbl>, wk31 <dbl>, wk32 <dbl>, wk33 <dbl>, wk34 <dbl>,
## #   wk35 <dbl>, wk36 <dbl>, wk37 <dbl>, wk38 <dbl>, wk39 <dbl>, wk40 <dbl>,
## #   wk41 <dbl>, wk42 <dbl>, wk43 <dbl>, wk44 <dbl>, wk45 <dbl>, wk46 <dbl>,
## #   wk47 <dbl>, wk48 <dbl>, wk49 <dbl>, wk50 <dbl>, wk51 <dbl>, wk52 <dbl>,
## #   wk53 <dbl>, wk54 <dbl>, wk55 <dbl>, wk56 <dbl>, wk57 <dbl>, wk58 <dbl>,
## #   wk59 <dbl>, wk60 <dbl>, wk61 <dbl>, wk62 <dbl>, wk63 <dbl>, wk64 <dbl>,
## #   wk65 <dbl>
```

Tanto `pivot_longer` quanto `pivot_wider` tem mais argumentos para lidar com situações complexas, como quando você precisa aplicar transformações em variáveis antes de reformatar o banco ou precisa utilizar múltiplas colunas, mas eu deixo isso para vocês descobrirem por conta própria quando estiverem confortáveis com a sintaxe básica.

7. O que os argumentos `extra` e `fill` em separate fazem? Utilize o exemplo a seguir como guia.


```r
tibble(x = c("a,b,c", "d,e,f,g", "h,i,j")) %>% 
  separate(x, c("um", "dois", "tres"))
```

```
## Warning: Expected 3 pieces. Additional pieces discarded in 1 rows [2].
```

```
## # A tibble: 3 x 3
##   um    dois  tres 
##   <chr> <chr> <chr>
## 1 a     b     c    
## 2 d     e     f    
## 3 h     i     j
```

```r
tibble(x = c("a,b,c", "d,e", "f,g,i")) %>% 
  separate(x, c("um", "dois", "tres"))
```

```
## Warning: Expected 3 pieces. Missing pieces filled with `NA` in 1 rows [2].
```

```
## # A tibble: 3 x 3
##   um    dois  tres 
##   <chr> <chr> <chr>
## 1 a     b     c    
## 2 d     e     <NA> 
## 3 f     g     i
```

Por padrão, `separate` espera que todas as colunas sendo separadas tenham o mesmo comprimento. Por exemplo, no primeiro caso, indicamos que vamos criar três novas colunas, chamadas de "um", "dois" e "tres". Mas os vetores tem tamanhos diferentes. Um deles tem 4 letras ao invés de 3. No segundo exemplo, um deles tem duas letras ao invés de três. Esse tipo de situação é bastante comum quando lidamos com erros de digitação. Então, o que fazer com o elemento que está sobrando ou faltando?


```r
# Sobrando
tibble(x = c("a,b,c", "d,e,f,g", "h,i,j")) %>% 
  separate(x, c("um", "dois", "tres"), extra = "warn") # avise que ocorreu (padrão)
```

```
## Warning: Expected 3 pieces. Additional pieces discarded in 1 rows [2].
```

```
## # A tibble: 3 x 3
##   um    dois  tres 
##   <chr> <chr> <chr>
## 1 a     b     c    
## 2 d     e     f    
## 3 h     i     j
```

```r
tibble(x = c("a,b,c", "d,e,f,g", "h,i,j")) %>% 
  separate(x, c("um", "dois", "tres"), extra = "drop") # descarte o que sobrou
```

```
## # A tibble: 3 x 3
##   um    dois  tres 
##   <chr> <chr> <chr>
## 1 a     b     c    
## 2 d     e     f    
## 3 h     i     j
```

```r
tibble(x = c("a,b,c", "d,e,f,g", "h,i,j")) %>% 
  separate(x, c("um", "dois", "tres"), extra = "merge") # junte com o último
```

```
## # A tibble: 3 x 3
##   um    dois  tres 
##   <chr> <chr> <chr>
## 1 a     b     c    
## 2 d     e     f,g  
## 3 h     i     j
```

```r
# Note especialmente no último caso o que ocorreu com as colunas.

# Faltando
tibble(x = c("a,b,c", "d,e", "f,g,i")) %>% 
  separate(x, c("um", "dois", "tres"), fill = "warn") # avise e preencha a direita
```

```
## Warning: Expected 3 pieces. Missing pieces filled with `NA` in 1 rows [2].
```

```
## # A tibble: 3 x 3
##   um    dois  tres 
##   <chr> <chr> <chr>
## 1 a     b     c    
## 2 d     e     <NA> 
## 3 f     g     i
```

```r
tibble(x = c("a,b,c", "d,e", "f,g,i")) %>% 
  separate(x, c("um", "dois", "tres"), fill = "left") # preencha a esquerda
```

```
## # A tibble: 3 x 3
##   um    dois  tres 
##   <chr> <chr> <chr>
## 1 a     b     c    
## 2 <NA>  d     e    
## 3 f     g     i
```

```r
tibble(x = c("a,b,c", "d,e", "f,g,i")) %>% 
  separate(x, c("um", "dois", "tres"), fill = "right") # preencha a direta
```

```
## # A tibble: 3 x 3
##   um    dois  tres 
##   <chr> <chr> <chr>
## 1 a     b     c    
## 2 d     e     <NA> 
## 3 f     g     i
```

```r
# Note na sua saída do R como ficou a tibble e onde foram colocados NAs
# em cada caso
```

8. Tanto `unite` como `separate` possuem um argumento `remove`. Pra que ele serve e quando você o utilizaria no valor `FALSE`?

Acho que a melhor forma de compreender isso é utilizar um banco de dados. Vamos pegar aquele da população retirado da Wikipédia.


```r
populacao <- tribble(
  ~Rank, ~Country, ~Population,	~'% of world', ~Day, ~Month, ~Year, ~Source,
  1L,         "China",    1411778724, "17.9%",  "1", "Nov", "2020",       "Seventh Census on 2020",
  2L,         "India",    1377123716, "17.5%", "19", "May", "2021", "National population clock[3]",
  3L, "United States",     331695937, "4.22%", "19", "May", "2021", "National population clock[4]",
  4L,     "Indonesia",     271350000, "3.45%", "31", "Dec", "2020",  "National annual estimate[5]",
  5L,      "Pakistan",     225200000, "2.86%",  "1", "Jul", "2021",             "UN projection[2]",
  6L,        "Brazil",     213154869, "2.71%", "19", "May", "2021", "National population clock[6]",
  7L,       "Nigeria",     211401000, "2.69%",  "1", "Jul", "2021",             "UN projection[2]",
  8L,    "Bangladesh",     170689832, "2.17%", "19", "May", "2021", "National population clock[7]",
  9L,        "Russia",     146171015, "1.86%",  "1", "Jan", "2021",  "National annual estimate[8]",
  10L,       "Mexico",     126014024, "1.60%",  "2", "Mar", "2020",        "2020 census result[9]"
)
populacao
```

```
## # A tibble: 10 x 8
##     Rank Country     Population `% of world` Day   Month Year  Source           
##    <int> <chr>            <dbl> <chr>        <chr> <chr> <chr> <chr>            
##  1     1 China       1411778724 17.9%        1     Nov   2020  Seventh Census o~
##  2     2 India       1377123716 17.5%        19    May   2021  National populat~
##  3     3 United Sta~  331695937 4.22%        19    May   2021  National populat~
##  4     4 Indonesia    271350000 3.45%        31    Dec   2020  National annual ~
##  5     5 Pakistan     225200000 2.86%        1     Jul   2021  UN projection[2] 
##  6     6 Brazil       213154869 2.71%        19    May   2021  National populat~
##  7     7 Nigeria      211401000 2.69%        1     Jul   2021  UN projection[2] 
##  8     8 Bangladesh   170689832 2.17%        19    May   2021  National populat~
##  9     9 Russia       146171015 1.86%        1     Jan   2021  National annual ~
## 10    10 Mexico       126014024 1.60%        2     Mar   2020  2020 census resu~
```

Vamos ver dois exemplos, um com `unite` e outro com `separate` para exemplificar o que `remove` faz.


```r
# Unir as colunas Day, Month, Year, remove = TRUE
populacao %>% unite(Data, Day, Month, Year, remove = TRUE) # padrão
```

```
## # A tibble: 10 x 6
##     Rank Country      Population `% of world` Data       Source                 
##    <int> <chr>             <dbl> <chr>        <chr>      <chr>                  
##  1     1 China        1411778724 17.9%        1_Nov_2020 Seventh Census on 2020 
##  2     2 India        1377123716 17.5%        19_May_20~ National population cl~
##  3     3 United Stat~  331695937 4.22%        19_May_20~ National population cl~
##  4     4 Indonesia     271350000 3.45%        31_Dec_20~ National annual estima~
##  5     5 Pakistan      225200000 2.86%        1_Jul_2021 UN projection[2]       
##  6     6 Brazil        213154869 2.71%        19_May_20~ National population cl~
##  7     7 Nigeria       211401000 2.69%        1_Jul_2021 UN projection[2]       
##  8     8 Bangladesh    170689832 2.17%        19_May_20~ National population cl~
##  9     9 Russia        146171015 1.86%        1_Jan_2021 National annual estima~
## 10    10 Mexico        126014024 1.60%        2_Mar_2020 2020 census result[9]
```

```r
# Unir as colunas Day, Month, Year, remove = FALSE
populacao %>% unite(Data, Day, Month, Year, remove = FALSE)
```

```
## # A tibble: 10 x 9
##     Rank Country   Population `% of world` Data    Day   Month Year  Source     
##    <int> <chr>          <dbl> <chr>        <chr>   <chr> <chr> <chr> <chr>      
##  1     1 China     1411778724 17.9%        1_Nov_~ 1     Nov   2020  Seventh Ce~
##  2     2 India     1377123716 17.5%        19_May~ 19    May   2021  National p~
##  3     3 United S~  331695937 4.22%        19_May~ 19    May   2021  National p~
##  4     4 Indonesia  271350000 3.45%        31_Dec~ 31    Dec   2020  National a~
##  5     5 Pakistan   225200000 2.86%        1_Jul_~ 1     Jul   2021  UN project~
##  6     6 Brazil     213154869 2.71%        19_May~ 19    May   2021  National p~
##  7     7 Nigeria    211401000 2.69%        1_Jul_~ 1     Jul   2021  UN project~
##  8     8 Banglade~  170689832 2.17%        19_May~ 19    May   2021  National p~
##  9     9 Russia     146171015 1.86%        1_Jan_~ 1     Jan   2021  National a~
## 10    10 Mexico     126014024 1.60%        2_Mar_~ 2     Mar   2020  2020 censu~
```

```r
# Vejam o que aconteceu com as colunas nos dois bancos.
```

Agora com `separate`: Separar a coluna `year` em século e ano, apenas como exemplo


```r
# remove = TRUE, padrão
populacao %>% separate(Year, c("seculo", "ano"), sep = 2, remove = TRUE)
```

```
## # A tibble: 10 x 9
##     Rank Country   Population `% of world` Day   Month seculo ano   Source      
##    <int> <chr>          <dbl> <chr>        <chr> <chr> <chr>  <chr> <chr>       
##  1     1 China     1411778724 17.9%        1     Nov   20     20    Seventh Cen~
##  2     2 India     1377123716 17.5%        19    May   20     21    National po~
##  3     3 United S~  331695937 4.22%        19    May   20     21    National po~
##  4     4 Indonesia  271350000 3.45%        31    Dec   20     20    National an~
##  5     5 Pakistan   225200000 2.86%        1     Jul   20     21    UN projecti~
##  6     6 Brazil     213154869 2.71%        19    May   20     21    National po~
##  7     7 Nigeria    211401000 2.69%        1     Jul   20     21    UN projecti~
##  8     8 Banglade~  170689832 2.17%        19    May   20     21    National po~
##  9     9 Russia     146171015 1.86%        1     Jan   20     21    National an~
## 10    10 Mexico     126014024 1.60%        2     Mar   20     20    2020 census~
```

```r
# remove = FALSE
populacao %>% separate(Year, c("seculo", "ano"), sep = 2, remove = FALSE)
```

```
## # A tibble: 10 x 10
##     Rank Country  Population `% of world` Day   Month Year  seculo ano   Source 
##    <int> <chr>         <dbl> <chr>        <chr> <chr> <chr> <chr>  <chr> <chr>  
##  1     1 China    1411778724 17.9%        1     Nov   2020  20     20    Sevent~
##  2     2 India    1377123716 17.5%        19    May   2021  20     21    Nation~
##  3     3 United ~  331695937 4.22%        19    May   2021  20     21    Nation~
##  4     4 Indones~  271350000 3.45%        31    Dec   2020  20     20    Nation~
##  5     5 Pakistan  225200000 2.86%        1     Jul   2021  20     21    UN pro~
##  6     6 Brazil    213154869 2.71%        19    May   2021  20     21    Nation~
##  7     7 Nigeria   211401000 2.69%        1     Jul   2021  20     21    UN pro~
##  8     8 Banglad~  170689832 2.17%        19    May   2021  20     21    Nation~
##  9     9 Russia    146171015 1.86%        1     Jan   2021  20     21    Nation~
## 10    10 Mexico    126014024 1.60%        2     Mar   2020  20     20    2020 c~
```

```r
# Vejam o que aconteceu com as colunas nos dois bancos.
```

Eu gosto de utilizar esse argumento quando eu tenho dúvida sobre o resultado e quero fazer inspeção visual para detectar eventuais problemas na separação ou junção. Uma vez que estou satisfeito com o resultado, em geral eu uso o `remove=TRUE`. Vocês tem que decidir se precisam manter as colunas originais ou se a coluna transformada é o suficiente.

9. Compare o argumento `values_fill` em `pivot_wider` e `fill` em `complete`. Qual é a diferença?

A resposta curta é simples: em `pivot_wider`, podemos ter aqueles missings "implícitos" que não apareciam no nosso banco longo e, durante a transformação, eles viram `NA`s nas colunas. Ó argumento `values_fill` indica um valor para ser preenchido no lugar de `NA`. 

Em `complete`, temos uma situação similar. O que fazer quando for encontrada uma combinação de valores no banco longo que é um "missing implícito"? Você pode especificar um valor padrão para preenchê-lo.

São funções similares, mas uma funciona sem reformatar o banco e a outra durante o processo de reformatação. Veja um exemplo abaixo com aquela `tibble` das ações.


```r
acoes <- tibble(
  ano   = c(2015, 2015, 2015, 2015, 2016, 2016, 2016),
  qdr   = c(   1,    2,    3,    4,    2,    3,    4),
  lucro = c(1.88, 0.59, 0.35,   NA, 0.92, 0.17, 2.66)
)

acoes
```

```
## # A tibble: 7 x 3
##     ano   qdr lucro
##   <dbl> <dbl> <dbl>
## 1  2015     1  1.88
## 2  2015     2  0.59
## 3  2015     3  0.35
## 4  2015     4 NA   
## 5  2016     2  0.92
## 6  2016     3  0.17
## 7  2016     4  2.66
```

```r
# Vamos supor que o NA implícito significa que a empresa teve 
# lucro = 0 naquele quadrimestre.

# pivot_wider, values_fill
acoes %>% pivot_wider(
  id_cols = ano,
  names_from = qdr,
  values_from = lucro,
  values_fill = 0
)
```

```
## # A tibble: 2 x 5
##     ano   `1`   `2`   `3`   `4`
##   <dbl> <dbl> <dbl> <dbl> <dbl>
## 1  2015  1.88  0.59  0.35 NA   
## 2  2016  0     0.92  0.17  2.66
```

```r
# complete, fill
acoes %>% complete(ano, qdr, fill = list(lucro = 0))
```

```
## # A tibble: 8 x 3
##     ano   qdr lucro
##   <dbl> <dbl> <dbl>
## 1  2015     1  1.88
## 2  2015     2  0.59
## 3  2015     3  0.35
## 4  2015     4  0   
## 5  2016     1  0   
## 6  2016     2  0.92
## 7  2016     3  0.17
## 8  2016     4  2.66
```

Note o resultado. E note também que `values_fill` em `pivot_wider` é um pouco mais criterioso na hora de fazer as transformações.

## `stringr`, `forcats` e `dplyr`


```r
library(nycflights13)
```


1. Encontre os vôos que:

  1. Atrasaram mais de duas horas
    

```r
flights %>% filter(dep_delay > 120)
```

```
## # A tibble: 9,723 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     1      848           1835       853     1001           1950
##  2  2013     1     1      957            733       144     1056            853
##  3  2013     1     1     1114            900       134     1447           1222
##  4  2013     1     1     1540           1338       122     2020           1825
##  5  2013     1     1     1815           1325       290     2120           1542
##  6  2013     1     1     1842           1422       260     1958           1535
##  7  2013     1     1     1856           1645       131     2212           2005
##  8  2013     1     1     1934           1725       129     2126           1855
##  9  2013     1     1     1938           1703       155     2109           1823
## 10  2013     1     1     1942           1705       157     2124           1830
## # ... with 9,713 more rows, and 11 more variables: arr_delay <dbl>,
## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```
    
    
  2. Com destino a Houston (`IAH` ou `HOU`)
    

```r
flights %>% filter(dest %in% c("IAH", "HOU"))
```

```
## # A tibble: 9,313 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     1      517            515         2      830            819
##  2  2013     1     1      533            529         4      850            830
##  3  2013     1     1      623            627        -4      933            932
##  4  2013     1     1      728            732        -4     1041           1038
##  5  2013     1     1      739            739         0     1104           1038
##  6  2013     1     1      908            908         0     1228           1219
##  7  2013     1     1     1028           1026         2     1350           1339
##  8  2013     1     1     1044           1045        -1     1352           1351
##  9  2013     1     1     1114            900       134     1447           1222
## 10  2013     1     1     1205           1200         5     1503           1505
## # ... with 9,303 more rows, and 11 more variables: arr_delay <dbl>,
## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```
  
  3. Operados pela United, American ou Delta


```r
unique(flights$carrier)
```

```
##  [1] "UA" "AA" "B6" "DL" "EV" "MQ" "US" "WN" "VX" "FL" "AS" "9E" "F9" "HA" "YV"
## [16] "OO"
```

```r
flights %>% filter(carrier %in% c("UA", "AA", "DL"))
```

```
## # A tibble: 139,504 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     1      517            515         2      830            819
##  2  2013     1     1      533            529         4      850            830
##  3  2013     1     1      542            540         2      923            850
##  4  2013     1     1      554            600        -6      812            837
##  5  2013     1     1      554            558        -4      740            728
##  6  2013     1     1      558            600        -2      753            745
##  7  2013     1     1      558            600        -2      924            917
##  8  2013     1     1      558            600        -2      923            937
##  9  2013     1     1      559            600        -1      941            910
## 10  2013     1     1      559            600        -1      854            902
## # ... with 139,494 more rows, and 11 more variables: arr_delay <dbl>,
## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```
    
  4. Decolaram entre julho e setembro
    

```r
flights %>% filter(between(month, 7, 9))
```

```
## # A tibble: 86,326 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     7     1        1           2029       212      236           2359
##  2  2013     7     1        2           2359         3      344            344
##  3  2013     7     1       29           2245       104      151              1
##  4  2013     7     1       43           2130       193      322             14
##  5  2013     7     1       44           2150       174      300            100
##  6  2013     7     1       46           2051       235      304           2358
##  7  2013     7     1       48           2001       287      308           2305
##  8  2013     7     1       58           2155       183      335             43
##  9  2013     7     1      100           2146       194      327             30
## 10  2013     7     1      100           2245       135      337            135
## # ... with 86,316 more rows, and 11 more variables: arr_delay <dbl>,
## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```
    
  5. Chegaram com mais de duas horas de atraso, mas não decolaram com atraso
    

```r
flights %>% filter(arr_delay > 120, dep_delay <= 0)
```

```
## # A tibble: 29 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1    27     1419           1420        -1     1754           1550
##  2  2013    10     7     1350           1350         0     1736           1526
##  3  2013    10     7     1357           1359        -2     1858           1654
##  4  2013    10    16      657            700        -3     1258           1056
##  5  2013    11     1      658            700        -2     1329           1015
##  6  2013     3    18     1844           1847        -3       39           2219
##  7  2013     4    17     1635           1640        -5     2049           1845
##  8  2013     4    18      558            600        -2     1149            850
##  9  2013     4    18      655            700        -5     1213            950
## 10  2013     5    22     1827           1830        -3     2217           2010
## # ... with 19 more rows, and 11 more variables: arr_delay <dbl>, carrier <chr>,
## #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
## #   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```
    
  6. Atrasaram mais de uma hora para decolar, mas recuperaram mais de 30 minutos durante o voo
    

```r
flights %>% filter(dep_delay > 60, dep_delay - arr_delay >= 30)
```

```
## # A tibble: 2,046 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     1     1716           1545        91     2140           2039
##  2  2013     1     1     2205           1720       285       46           2040
##  3  2013     1     1     2326           2130       116      131             18
##  4  2013     1     3     1503           1221       162     1803           1555
##  5  2013     1     3     1821           1530       171     2131           1910
##  6  2013     1     3     1839           1700        99     2056           1950
##  7  2013     1     3     1850           1745        65     2148           2120
##  8  2013     1     3     1923           1815        68     2036           1958
##  9  2013     1     3     1941           1759       102     2246           2139
## 10  2013     1     3     1950           1845        65     2228           2227
## # ... with 2,036 more rows, and 11 more variables: arr_delay <dbl>,
## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```
    
  7. Decolaram entre a meia-noite e 6 da manhã (inclusive)
    

```r
flights %>% filter(between(hour, 0, 5) | (hour == 6 & minute == 0))
```

```
## # A tibble: 8,970 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     1      517            515         2      830            819
##  2  2013     1     1      533            529         4      850            830
##  3  2013     1     1      542            540         2      923            850
##  4  2013     1     1      544            545        -1     1004           1022
##  5  2013     1     1      554            600        -6      812            837
##  6  2013     1     1      554            558        -4      740            728
##  7  2013     1     1      555            600        -5      913            854
##  8  2013     1     1      557            600        -3      709            723
##  9  2013     1     1      557            600        -3      838            846
## 10  2013     1     1      558            600        -2      753            745
## # ... with 8,960 more rows, and 11 more variables: arr_delay <dbl>,
## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```
    
2. Reordene suas colunas para encontrar os voos mais rápidos (maior velocidade de voo).


```r
flights %>% 
  select(air_time, distance) %>% 
  mutate(speed = distance/air_time) %>% 
  arrange(desc(speed))
```

```
## # A tibble: 336,776 x 3
##    air_time distance speed
##       <dbl>    <dbl> <dbl>
##  1       65      762 11.7 
##  2       93     1008 10.8 
##  3       55      594 10.8 
##  4       70      748 10.7 
##  5      105     1035  9.86
##  6      170     1598  9.4 
##  7      172     1598  9.29
##  8      175     1623  9.27
##  9      173     1598  9.24
## 10      173     1598  9.24
## # ... with 336,766 more rows
```

3. Teste várias maneiras diferentes de selecionar as variáveis `dep_time`, `dep_delay`, `arr_time` e `arr_delay` usando as várias helper functions de `select`.


```r
flights %>% select(dep_time, dep_delay, arr_time, arr_delay)
```

```
## # A tibble: 336,776 x 4
##    dep_time dep_delay arr_time arr_delay
##       <int>     <dbl>    <int>     <dbl>
##  1      517         2      830        11
##  2      533         4      850        20
##  3      542         2      923        33
##  4      544        -1     1004       -18
##  5      554        -6      812       -25
##  6      554        -4      740        12
##  7      555        -5      913        19
##  8      557        -3      709       -14
##  9      557        -3      838        -8
## 10      558        -2      753         8
## # ... with 336,766 more rows
```

```r
flights %>% select(starts_with("dep"), starts_with("arr"))
```

```
## # A tibble: 336,776 x 4
##    dep_time dep_delay arr_time arr_delay
##       <int>     <dbl>    <int>     <dbl>
##  1      517         2      830        11
##  2      533         4      850        20
##  3      542         2      923        33
##  4      544        -1     1004       -18
##  5      554        -6      812       -25
##  6      554        -4      740        12
##  7      555        -5      913        19
##  8      557        -3      709       -14
##  9      557        -3      838        -8
## 10      558        -2      753         8
## # ... with 336,766 more rows
```

```r
flights %>% select(starts_with(c("dep", "arr")))
```

```
## # A tibble: 336,776 x 4
##    dep_time dep_delay arr_time arr_delay
##       <int>     <dbl>    <int>     <dbl>
##  1      517         2      830        11
##  2      533         4      850        20
##  3      542         2      923        33
##  4      544        -1     1004       -18
##  5      554        -6      812       -25
##  6      554        -4      740        12
##  7      555        -5      913        19
##  8      557        -3      709       -14
##  9      557        -3      838        -8
## 10      558        -2      753         8
## # ... with 336,766 more rows
```

```r
flights %>% select(matches("^arr|^dep"))
```

```
## # A tibble: 336,776 x 4
##    dep_time dep_delay arr_time arr_delay
##       <int>     <dbl>    <int>     <dbl>
##  1      517         2      830        11
##  2      533         4      850        20
##  3      542         2      923        33
##  4      544        -1     1004       -18
##  5      554        -6      812       -25
##  6      554        -4      740        12
##  7      555        -5      913        19
##  8      557        -3      709       -14
##  9      557        -3      838        -8
## 10      558        -2      753         8
## # ... with 336,766 more rows
```

```r
flights %>% select(!starts_with(c("sched", "car")) & contains(c("dep", "arr")))
```

```
## # A tibble: 336,776 x 4
##    dep_time dep_delay arr_time arr_delay
##       <int>     <dbl>    <int>     <dbl>
##  1      517         2      830        11
##  2      533         4      850        20
##  3      542         2      923        33
##  4      544        -1     1004       -18
##  5      554        -6      812       -25
##  6      554        -4      740        12
##  7      555        -5      913        19
##  8      557        -3      709       -14
##  9      557        -3      838        -8
## 10      558        -2      753         8
## # ... with 336,766 more rows
```

```r
flights %>% select(ends_with(c("time", "delay")) & !starts_with(c("sched", "air")))
```

```
## # A tibble: 336,776 x 4
##    dep_time arr_time dep_delay arr_delay
##       <int>    <int>     <dbl>     <dbl>
##  1      517      830         2        11
##  2      533      850         4        20
##  3      542      923         2        33
##  4      544     1004        -1       -18
##  5      554      812        -6       -25
##  6      554      740        -4        12
##  7      555      913        -5        19
##  8      557      709        -3       -14
##  9      557      838        -3        -8
## 10      558      753        -2         8
## # ... with 336,766 more rows
```


4. As variáveis `dep_time` e `sched_dep_time` estão num formato incorreto (veja `?flights`). Converta-as com `mutate` para um valor em minutos passados desde a meia-noite. Dica: utilize `%/%` e `%%`.


```r
flights %>% mutate(
  dep_hour         = dep_time %/% 100,
  dep_minute       = dep_time %% 100,
  sched_dep_hour   = sched_dep_time %/% 100,
  sched_arr_minute = sched_arr_time %% 100
)
```

```
## # A tibble: 336,776 x 23
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     1      517            515         2      830            819
##  2  2013     1     1      533            529         4      850            830
##  3  2013     1     1      542            540         2      923            850
##  4  2013     1     1      544            545        -1     1004           1022
##  5  2013     1     1      554            600        -6      812            837
##  6  2013     1     1      554            558        -4      740            728
##  7  2013     1     1      555            600        -5      913            854
##  8  2013     1     1      557            600        -3      709            723
##  9  2013     1     1      557            600        -3      838            846
## 10  2013     1     1      558            600        -2      753            745
## # ... with 336,766 more rows, and 15 more variables: arr_delay <dbl>,
## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>,
## #   dep_hour <dbl>, dep_minute <dbl>, sched_dep_hour <dbl>,
## #   sched_arr_minute <dbl>
```

```r
# Há uma outra solução com separate!
flights %>% 
  separate(
    col = dep_time,
    into = c("dep_hour", "dep_minute"),
    sep = 1,
    # Esse argumento é importante! Teste com FALSE para ver a diferença
    convert = TRUE) %>% 
  separate(
    col = sched_dep_time,
    into = c("sched_dep_hour", "sched_dep_minute"),
    sep = 1,
    convert = TRUE)
```

```
## # A tibble: 336,776 x 21
##     year month   day dep_hour dep_minute sched_dep_hour sched_dep_minute
##    <int> <int> <int>    <int>      <int>          <int>            <int>
##  1  2013     1     1        5         17              5               15
##  2  2013     1     1        5         33              5               29
##  3  2013     1     1        5         42              5               40
##  4  2013     1     1        5         44              5               45
##  5  2013     1     1        5         54              6                0
##  6  2013     1     1        5         54              5               58
##  7  2013     1     1        5         55              6                0
##  8  2013     1     1        5         57              6                0
##  9  2013     1     1        5         57              6                0
## 10  2013     1     1        5         58              6                0
## # ... with 336,766 more rows, and 14 more variables: dep_delay <dbl>,
## #   arr_time <int>, sched_arr_time <int>, arr_delay <dbl>, carrier <chr>,
## #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
## #   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```

Existe uma outra solução possível para essa questão usando manipulação de strings, com `str_sub` também. Fica como desafio!

Pensando na legibilidade do código e na flexibilidade da abordagem, qual das duas soluções acima você implementaria? `mutate` ou duas `separate`? Reflita.

5. O que o código abaixo está fazendo? Porque mesmo após o código abaixo continuam existindo diferenças entre os valores das variáveis `air_time` e `travel_time`?


```r
flights %>% 
  select(air_time, dep_time, arr_time, dep_delay, arr_delay) %>% 
  mutate(dep_hour = dep_time %/% 100,
         dep_min = dep_time %% 100,
         dep_time2 = dep_hour * 60 + dep_min,
         arr_hour = arr_time %/% 100,
         arr_min = arr_time %% 100,
         arr_time2 = arr_hour * 60 + arr_min,
         travel_time = arr_time2 - dep_time2) %>% 
  select(-dep_hour, -dep_min, -arr_hour, -arr_min)
```

```
## # A tibble: 336,776 x 8
##    air_time dep_time arr_time dep_delay arr_delay dep_time2 arr_time2
##       <dbl>    <int>    <int>     <dbl>     <dbl>     <dbl>     <dbl>
##  1      227      517      830         2        11       317       510
##  2      227      533      850         4        20       333       530
##  3      160      542      923         2        33       342       563
##  4      183      544     1004        -1       -18       344       604
##  5      116      554      812        -6       -25       354       492
##  6      150      554      740        -4        12       354       460
##  7      158      555      913        -5        19       355       553
##  8       53      557      709        -3       -14       357       429
##  9      140      557      838        -3        -8       357       518
## 10      138      558      753        -2         8       358       473
## # ... with 336,766 more rows, and 1 more variable: travel_time <dbl>
```

Essa tem uma resposta mais qualitativa. A primeira parte é parecida com a questão anterior, mas estamos manualmente tentando calcular os tempos de viagem. Acontece que os valores não batem com os tempos de vôo identificados no banco. Isso se deve a pelo menos três questões distintas. 

- Uma delas diz respeito ao registro dos tempos, a definição de `air_time` pode não estar considerando tempos em que o avião está manobrando ou em solo ou mesmo podem existir erros de preenchimento. 
- A segunda diz respeito ao fuso horário distinto entre aeroportos de saída e chegada, que complica o cálculo dos tempos reais, então nosso cálculo está muito cru para identificar isso.
- A última questão são os vôos longos, que começam em um dia e terminam no dia seguinte, que podem prejudicar nosso método de cálculo. Para corrigir alguns desses problemas, você precisaria escrever um código que minimamente levasse essas questões em consideração. Como esse não é o objetivo do curso, eu deixo para quem quiser tentar. [Há uma solução postada aqui](https://jrnold.github.io/r4ds-exercise-solutions/transform.html#exercise-5.5.2).

6. Use o stringr para concatenar as seguintes strings em uma frase


```r
x <- "."
y <- "feliz"
w <- "acordei"
z <- "hoje"

str_c(z, w, y, sep = " ") %>% 
  str_c(x, sep = "") %>% 
  str_to_sentence()
```

```
## [1] "Hoje acordei feliz."
```

7. Corrija as inconsistências nas colunas país, primeiro_nome, segundo_nome e crie uma nova coluna nomes contendo as duas anteriores. No final, ordene o banco em ordem alfabética.


```r
df <- 
  tibble::tribble(
    ~pais,    ~primeiro_nome, ~segundo_nome,
    # -------|----------------|-------------|
    "BRASIL", "ISABELA",       "MARTINS",
    "Brasil", "Eduardo",       "cabellos",
    "brasil", "márcia",         "pinto",
    "bRaSiL", "rogério",        "Marinho",
  )

# Sem dplyr
df$pais <- str_to_title(df$pais)
df$primeiro_nome <-  str_to_title(df$primeiro_nome)
df$segundo_nome <- str_to_title(df$segundo_nome)

df <- df %>% tidyr::unite(nomes, primeiro_nome, segundo_nome, sep = " ")
df[ str_order(df$nomes), ]
```

```
## # A tibble: 4 x 2
##   pais   nomes           
##   <chr>  <chr>           
## 1 Brasil Eduardo Cabellos
## 2 Brasil Isabela Martins 
## 3 Brasil Márcia Pinto    
## 4 Brasil Rogério Marinho
```

```r
# Com dplyr
df <- 
  tibble::tribble(
    ~pais,    ~primeiro_nome, ~segundo_nome,
    # -------|----------------|-------------|
    "BRASIL", "ISABELA",       "MARTINS",
    "Brasil", "Eduardo",       "cabellos",
    "brasil", "márcia",         "pinto",
    "bRaSiL", "rogério",        "Marinho",
  )

df %>% 
  mutate(pais = str_to_title(pais),
         primeiro_nome = str_to_title(primeiro_nome),
         segundo_nome = str_to_title(segundo_nome)) %>% 
  unite(nomes, primeiro_nome, segundo_nome, sep = " ") %>% 
  arrange(str_order(nomes))
```

```
## # A tibble: 4 x 2
##   pais   nomes           
##   <chr>  <chr>           
## 1 Brasil Eduardo Cabellos
## 2 Brasil Isabela Martins 
## 3 Brasil Márcia Pinto    
## 4 Brasil Rogério Marinho
```

8. Transforme a string `c("Seu nome", "Seu sobrenome da mãe", "Seu sobrenome do pai")` na string `"SEU SOBRENOME DO PAI, sua inicial do nome. sua inicial da mãe."`, como numa citação. Veja o exemplo abaixo:


```r
# Transforme
c("Vinícius", "de Souza", "Maia")
```

```
## [1] "Vinícius" "de Souza" "Maia"
```

```r
# Resultado
"MAIA, V. S."
```

```
## [1] "MAIA, V. S."
```

```r
x <- c("Vinícius", "de Souza", "Maia")
x[1] <- str_sub(x[1], 1, 1) %>% str_c(".")
x[2] <- str_sub(x[2], 4, 4) %>% str_c(".")
x[3] <- str_to_upper(x[3])
str_c(c(x[3], x[1], x[2]), collapse = " ")
```

```
## [1] "MAIA V. S."
```


9. DESAFIO: Nos microdados da área de saúde, é comum que a variável idade esteja registrada da seguinte forma: "150", "219", "312", "471". Esses códigos indicam primeiro qual a unidade de medida da idade e segundo o valor desta unidade, 1 = horas, 2 = dias, 3 = meses, 4 = anos. Proponha um código usando `stringr` para transformar o vetor abaixo em um valor numérico.


```r
# Não precisa se preocupar com essa parte
x <- as.character(round(c(
  runif(25, 100, 124),
  runif(25, 201, 230),
  runif(25, 301, 312),
  runif(25, 401, 499)
)))

# Como você transformaria esse vetor em número?
x
```

```
##   [1] "100" "115" "117" "116" "113" "105" "117" "107" "111" "119" "114" "117"
##  [13] "113" "122" "113" "124" "106" "100" "108" "123" "107" "100" "117" "108"
##  [25] "122" "202" "218" "211" "209" "209" "212" "208" "221" "212" "219" "219"
##  [37] "217" "208" "206" "215" "214" "226" "205" "222" "213" "223" "224" "206"
##  [49] "205" "219" "305" "308" "309" "301" "308" "309" "304" "303" "304" "302"
##  [61] "307" "301" "311" "310" "310" "308" "301" "308" "309" "310" "311" "305"
##  [73] "304" "301" "305" "449" "423" "494" "477" "490" "445" "426" "491" "416"
##  [85] "472" "415" "448" "473" "488" "446" "419" "480" "444" "432" "466" "406"
##  [97] "406" "421" "491" "417"
```

```r
# Esse exercício é um pouco mais difícil mesmo!
x %>% str_extract("\\d")
```

```
##   [1] "1" "1" "1" "1" "1" "1" "1" "1" "1" "1" "1" "1" "1" "1" "1" "1" "1" "1"
##  [19] "1" "1" "1" "1" "1" "1" "1" "2" "2" "2" "2" "2" "2" "2" "2" "2" "2" "2"
##  [37] "2" "2" "2" "2" "2" "2" "2" "2" "2" "2" "2" "2" "2" "2" "3" "3" "3" "3"
##  [55] "3" "3" "3" "3" "3" "3" "3" "3" "3" "3" "3" "3" "3" "3" "3" "3" "3" "3"
##  [73] "3" "3" "3" "4" "4" "4" "4" "4" "4" "4" "4" "4" "4" "4" "4" "4" "4" "4"
##  [91] "4" "4" "4" "4" "4" "4" "4" "4" "4" "4"
```

```r
tibble(
  tipo_idade = str_sub(x, 1, 1),
  idade = str_sub(x, 2, 3),
  idade_anos =
    if_else(
      str_detect(tipo_idade, "1"),
      as.numeric(idade) / (24 * 30 * 12),
      if_else(
        str_detect(tipo_idade, "2"),
        as.numeric(idade) / (30 * 12),
        if_else(
          str_detect(tipo_idade, "3"),
          as.numeric(idade) / 12,
          as.numeric(idade)
        )
      )
    )
) %>% 
  print(n = Inf)
```

```
## # A tibble: 100 x 3
##     tipo_idade idade idade_anos
##     <chr>      <chr>      <dbl>
##   1 1          00      0       
##   2 1          15      0.00174 
##   3 1          17      0.00197 
##   4 1          16      0.00185 
##   5 1          13      0.00150 
##   6 1          05      0.000579
##   7 1          17      0.00197 
##   8 1          07      0.000810
##   9 1          11      0.00127 
##  10 1          19      0.00220 
##  11 1          14      0.00162 
##  12 1          17      0.00197 
##  13 1          13      0.00150 
##  14 1          22      0.00255 
##  15 1          13      0.00150 
##  16 1          24      0.00278 
##  17 1          06      0.000694
##  18 1          00      0       
##  19 1          08      0.000926
##  20 1          23      0.00266 
##  21 1          07      0.000810
##  22 1          00      0       
##  23 1          17      0.00197 
##  24 1          08      0.000926
##  25 1          22      0.00255 
##  26 2          02      0.00556 
##  27 2          18      0.05    
##  28 2          11      0.0306  
##  29 2          09      0.025   
##  30 2          09      0.025   
##  31 2          12      0.0333  
##  32 2          08      0.0222  
##  33 2          21      0.0583  
##  34 2          12      0.0333  
##  35 2          19      0.0528  
##  36 2          19      0.0528  
##  37 2          17      0.0472  
##  38 2          08      0.0222  
##  39 2          06      0.0167  
##  40 2          15      0.0417  
##  41 2          14      0.0389  
##  42 2          26      0.0722  
##  43 2          05      0.0139  
##  44 2          22      0.0611  
##  45 2          13      0.0361  
##  46 2          23      0.0639  
##  47 2          24      0.0667  
##  48 2          06      0.0167  
##  49 2          05      0.0139  
##  50 2          19      0.0528  
##  51 3          05      0.417   
##  52 3          08      0.667   
##  53 3          09      0.75    
##  54 3          01      0.0833  
##  55 3          08      0.667   
##  56 3          09      0.75    
##  57 3          04      0.333   
##  58 3          03      0.25    
##  59 3          04      0.333   
##  60 3          02      0.167   
##  61 3          07      0.583   
##  62 3          01      0.0833  
##  63 3          11      0.917   
##  64 3          10      0.833   
##  65 3          10      0.833   
##  66 3          08      0.667   
##  67 3          01      0.0833  
##  68 3          08      0.667   
##  69 3          09      0.75    
##  70 3          10      0.833   
##  71 3          11      0.917   
##  72 3          05      0.417   
##  73 3          04      0.333   
##  74 3          01      0.0833  
##  75 3          05      0.417   
##  76 4          49     49       
##  77 4          23     23       
##  78 4          94     94       
##  79 4          77     77       
##  80 4          90     90       
##  81 4          45     45       
##  82 4          26     26       
##  83 4          91     91       
##  84 4          16     16       
##  85 4          72     72       
##  86 4          15     15       
##  87 4          48     48       
##  88 4          73     73       
##  89 4          88     88       
##  90 4          46     46       
##  91 4          19     19       
##  92 4          80     80       
##  93 4          44     44       
##  94 4          32     32       
##  95 4          66     66       
##  96 4          06      6       
##  97 4          06      6       
##  98 4          21     21       
##  99 4          91     91       
## 100 4          17     17
```

Ao invés de utilizar essas chamadas recursivas de `if_else`, que são muito ruins de ler, como você poderia reescrever a condição usando `case_when`?

10. Explore as contagens da variável `rincome` em `gss_cat`, ela ficaria bem representada num gráfico? De qual tipo?


```r
gss_cat %>% count(rincome)
```

```
## # A tibble: 16 x 2
##    rincome            n
##    <fct>          <int>
##  1 No answer        183
##  2 Don't know       267
##  3 Refused          975
##  4 $25000 or more  7363
##  5 $20000 - 24999  1283
##  6 $15000 - 19999  1048
##  7 $10000 - 14999  1168
##  8 $8000 to 9999    340
##  9 $7000 to 7999    188
## 10 $6000 to 6999    215
## 11 $5000 to 5999    227
## 12 $4000 to 4999    226
## 13 $3000 to 3999    276
## 14 $1000 to 2999    395
## 15 Lt $1000         286
## 16 Not applicable  7043
```

Em geral, contagens de variáveis ficam bem em gráficos de barras ou visualizações equivalentes, em que é possível comparar visualmente as contagens das diversas categorias. Mais sobre isso na aula do `ggplot2`.

11. Qual a religião mais comum em `gss_cat`? Qual o partido (`partyid`) mais popular?


```r
# Religião
gss_cat %>% count(relig) %>% arrange(desc(n))
```

```
## # A tibble: 15 x 2
##    relig                       n
##    <fct>                   <int>
##  1 Protestant              10846
##  2 Catholic                 5124
##  3 None                     3523
##  4 Christian                 689
##  5 Jewish                    388
##  6 Other                     224
##  7 Buddhism                  147
##  8 Inter-nondenominational   109
##  9 Moslem/islam              104
## 10 Orthodox-christian         95
## 11 No answer                  93
## 12 Hinduism                   71
## 13 Other eastern              32
## 14 Native american            23
## 15 Don't know                 15
```

```r
# Partido
gss_cat %>% count(partyid) %>% arrange(desc(n))
```

```
## # A tibble: 10 x 2
##    partyid                n
##    <fct>              <int>
##  1 Independent         4119
##  2 Not str democrat    3690
##  3 Strong democrat     3490
##  4 Not str republican  3032
##  5 Ind,near dem        2499
##  6 Strong republican   2314
##  7 Ind,near rep        1791
##  8 Other party          393
##  9 No answer            154
## 10 Don't know             1
```

12. A que religião se refere a variável `denom`? Você pode descobrir isso fazendo uma tabela de contagens?

Você pode chamar count com várias variáveis para fazer uma tabulação cruzada.


```r
gss_cat %>% count(relig, denom) %>% print(n = Inf)
```

```
## # A tibble: 47 x 3
##    relig                   denom                    n
##    <fct>                   <fct>                <int>
##  1 No answer               No answer               93
##  2 Don't know              Not applicable          15
##  3 Inter-nondenominational Not applicable         109
##  4 Native american         Not applicable          23
##  5 Christian               No answer                2
##  6 Christian               Don't know              11
##  7 Christian               No denomination        452
##  8 Christian               Not applicable         224
##  9 Orthodox-christian      Not applicable          95
## 10 Moslem/islam            Not applicable         104
## 11 Other eastern           Not applicable          32
## 12 Hinduism                Not applicable          71
## 13 Buddhism                Not applicable         147
## 14 Other                   No denomination          7
## 15 Other                   Not applicable         217
## 16 None                    Not applicable        3523
## 17 Jewish                  Not applicable         388
## 18 Catholic                Not applicable        5124
## 19 Protestant              No answer               22
## 20 Protestant              Don't know              41
## 21 Protestant              No denomination       1224
## 22 Protestant              Other                 2534
## 23 Protestant              Episcopal              397
## 24 Protestant              Presbyterian-dk wh     244
## 25 Protestant              Presbyterian, merged    67
## 26 Protestant              Other presbyterian      47
## 27 Protestant              United pres ch in us   110
## 28 Protestant              Presbyterian c in us   104
## 29 Protestant              Lutheran-dk which      267
## 30 Protestant              Evangelical luth       122
## 31 Protestant              Other lutheran          30
## 32 Protestant              Wi evan luth synod      71
## 33 Protestant              Lutheran-mo synod      212
## 34 Protestant              Luth ch in america      71
## 35 Protestant              Am lutheran            146
## 36 Protestant              Methodist-dk which     239
## 37 Protestant              Other methodist         33
## 38 Protestant              United methodist      1067
## 39 Protestant              Afr meth ep zion        32
## 40 Protestant              Afr meth episcopal      77
## 41 Protestant              Baptist-dk which      1457
## 42 Protestant              Other baptists         213
## 43 Protestant              Southern baptist      1536
## 44 Protestant              Nat bapt conv usa       40
## 45 Protestant              Nat bapt conv of am     76
## 46 Protestant              Am bapt ch in usa      130
## 47 Protestant              Am baptist asso        237
```

13. Como você poderia diminuir o número de categorias da variável `rincome` do banco `gss_cat`?

A melhor função para redução de fatores é `fct_collapse`. Veja como ficam a coluna original e a transformada.


```r
gss_cat2 <- 
  gss_cat %>% 
  # Aqui vou salvar em "rincome2" para a gente poder ver as duas
  mutate(rincome2 = fct_collapse(
    rincome,
    "Non-response" = c("No answer", "Don't know", "Refused", "Not applicable"),
    "Até 5k"       = c("$4000 to 4999", "$3000 to 3999", "$1000 to 2999", "Lt $1000"),
    "5k-10k"       = c( "$8000 to 9999", "$7000 to 7999", "$6000 to 6999", "$5000 to 5999"),
    "10k-20k"      = c("$15000 - 19999", "$10000 - 14999"),
    "20k+"         = c("$25000 or more", "$20000 - 24999"))) %>% 
  select(rincome, rincome2)

# E veja as contagens
gss_cat2 %>% count(rincome)
```

```
## # A tibble: 16 x 2
##    rincome            n
##    <fct>          <int>
##  1 No answer        183
##  2 Don't know       267
##  3 Refused          975
##  4 $25000 or more  7363
##  5 $20000 - 24999  1283
##  6 $15000 - 19999  1048
##  7 $10000 - 14999  1168
##  8 $8000 to 9999    340
##  9 $7000 to 7999    188
## 10 $6000 to 6999    215
## 11 $5000 to 5999    227
## 12 $4000 to 4999    226
## 13 $3000 to 3999    276
## 14 $1000 to 2999    395
## 15 Lt $1000         286
## 16 Not applicable  7043
```

```r
gss_cat2 %>% count(rincome2)
```

```
## # A tibble: 5 x 2
##   rincome2         n
##   <fct>        <int>
## 1 Non-response  8468
## 2 20k+          8646
## 3 10k-20k       2216
## 4 5k-10k         970
## 5 Até 5k        1183
```

## `ggplot2`

1. O que tem de errado no código abaixo? Por que os pontos não ficaram azuis?


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, color = "blue"))
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-42-1.png" width="672" />

Os pontos não ficam azuis porque você não está especificando cores! Dentro da função `aes()` você está especificando variáveis para serem mapeadas a uma escala de cores. Portanto, o `ggplot` interpreta "blue" como uma variável sem nome que tem o valor "blue" e mapeia ela para a escala de cores padrão, que é vermelha. Se você quer controlar apenas a "aparência" dos pontos e não está preocupada em mapear nenhuma variável, você pode passar essa estética fora da função `aes()`.


```r
ggplot(data = mpg) + 
  geom_point(
    mapping = aes(x = displ, y = hwy), # aqui acabam os mapeamentos estéticos
    color = "blue", # alteração apenas na aparência do geom
    size = 2, # alteração apenas na aparência do geom
    shape = 6 # alteração apenas na aparência do geom
    ) 
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-43-1.png" width="672" />


2. Utilizando o banco `mpg`, faça o diagrama de dispersão de `displ` por `hwy` e mapeie a cor para `class`, o tamanho para `cyl` e a forma para `manufacturer`. Como esses atributos estéticos se comportam diferente para variáveis categóricas vs contínuas?


```r
ggplot(mpg, aes(
  displ, 
  hwy, 
  color = class, 
  size = cyl,
  shape = manufacturer)) +
  geom_point()
```

```
## Warning: The shape palette can deal with a maximum of 6 discrete values because
## more than 6 becomes difficult to discriminate; you have 15. Consider
## specifying shapes manually if you must have them.
```

```
## Warning: Removed 112 rows containing missing values (geom_point).
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-44-1.png" width="672" />

Ao cumprir as instruções como dadas, logo de cara você recebe um aviso do `ggplot2`. A paleta de "shapes" só recebe por padrão 6 shapes diferentes, porque de acordo com o autor, mais de 6 torna difícil de distinguir. Mas eu sou teimoso.


```r
ggplot(mpg, aes(
  displ, 
  hwy, 
  color = class, 
  size = cyl,
  shape = manufacturer)) +
  geom_point() +
  scale_shape_manual(values = 1:15, guide = "legend")
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-45-1.png" width="672" />

Esse gráfico é mais um exemplo para vocês verem como diferentes escalas se comportam. A variável `cyl` é numérica e ordenada, então faz sentido colocá-la num mapeamento como `size`, já que visualmente é possível indicar que a grandeza aumenta com o tamanho. Classe é uma variável categórica, então ela fica melhor em mapeamentos que ressaltam diferenças entre as categorias, como `colors` ou `shapes`. O pacote também impõe algumas restrições sobre o que é possível mapear. Por exemplo, ele retorna erro se você tenta mapear uma variável discreta para uma escala contínua.


```r
ggplot(mpg, aes(displ, hwy, color = class)) +
  geom_point() +
  scale_color_continuous()
```

```
## Error: Discrete value supplied to continuous scale
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-46-1.png" width="672" />

Experimentem tentar mapear diferentes variáveis no banco `mpg` para as diferentes escalas e vejam os resultados. Em alguns casos, é possível, mas o gráfico é pouco informativo, em outros, você verá mensagens de erro.

3. Utilizando o `diamonds`, crie um diagrama de dispersão que relacione `carat` com `price`. Explore algumas outras variáveis utilizando escalas de cor para ver se você identifica algum padrão. Aplique transformações nas variáveis que você considerar justificadas.

Esse exercício não tem uma resposta correta. O objetivo era que vocês explorassem as transformações estatísticas e as escalas de cores diferentes presentes no `ggplot`, através do argumento `trans`, ou mesmo fazer outras transformações que interessassem vocês nas variáveis. Abaixo um exemplo de transformação de Yeo-Johnson, um tipo de transformação BoxCox que aceita valores negativos e uma das escalas de cor do pacote `viridis`.


```r
ggplot(diamonds, aes(carat, price, color = clarity)) +
  geom_point() +
  scale_x_continuous(trans = scales::yj_trans(p = 2)) +
  scale_color_viridis_d(option = "magma")
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-47-1.png" width="672" />

4. Ainda continuando o exemplo anterior, aplique um `geom_smooth` utilizando várias opções de `method` para as variáveis originais ou transformadas.

Segundo a mesma lógica, o objetivo era explorar as opções de visualização de modelos simples através do argumento `method`. Abaixo um exemplo de `gam`. Uma mudança que fiz foi usar a variável `cut` ao invés de `clarity`, porque o gráfico não-transformado de `clarity` estava muito poluído.


```r
ggplot(diamonds, aes(carat, price, color = cut)) +
  geom_point(alpha = 0.1) + # pontos translúcidos para reduzir a poluição
  geom_smooth(method = "gam", se = FALSE) +
  scale_color_viridis_d(option = "plasma")
```

```
## `geom_smooth()` using formula 'y ~ s(x, bs = "cs")'
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-48-1.png" width="672" />

5. No nosso gráfico de barras usando `stat(prop)` a gente precisou colocar `group = 1`, porque? Qual é a diferença entre esses dois códigos?


```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, y = after_stat(prop)))
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-49-1.png" width="672" />

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = color, y = after_stat(prop)))
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-49-2.png" width="672" />

Acabei explicando isso na aula, devido a uma pergunta, mas para quem perdeu, trata-se do comportamento padrão quando há proporções: cada barra terá sua própria proporção e todas somarão a 100%. O uso de `group = 1` indica à função que as proporções que somam a 100% são o total dos níveis do fator e não cada nível individualmente.


```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, y = after_stat(prop), group = 1))
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-50-1.png" width="672" />

```r
# No caso em que há um "fill", precisamos normalizar as alturas das barras
ggplot(data = diamonds) +
  geom_bar(mapping = aes(
    x = cut,
    y = stat(prop),
    fill = clarity
  ))
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-50-2.png" width="672" />

```r
ggplot(data = diamonds) +
  geom_bar(mapping = aes(
    x = cut,
    y = stat(count)/sum(stat(count)),
    fill = clarity
  ))
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-50-3.png" width="672" />


6. `stat_smooth` é muito parecido com `geom_smooth`, mas há uma diferença sutil. Compare os códigos abaixo.

`geom_smooth` chama `stat_smooth` quando você utiliza a função para calcular as "médias condicionais" que correspondem a linha de tendência desenhada no gráfico. É assim com todos os `geom`s no pacote. Há uma conexão entre o objeto geométrico e uma transformação estatística. Mesmo que seja a transformação `_identity`, que mantém a variável exatamente como ela estava no dado. A grande vantagem de construir um gráfico com `stat_smooth` ao invés de `geom_smooth` é que você pode especificar outro objeto geométrico que não seja o padrão (`geom_line` + `geom_ribbon`). É isso que os gráficos abaixo demonstram.


```r
ggplot(mpg, aes(displ, hwy)) + 
  geom_point() +
  geom_smooth()
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-51-1.png" width="672" />

```r
ggplot(mpg, aes(displ, hwy)) + 
  geom_point() +
  stat_smooth(geom = "step")
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-51-2.png" width="672" />

```r
ggplot(mpg, aes(displ, hwy)) + 
  geom_point() +
  stat_smooth(geom = "linerange")
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-51-3.png" width="672" />

```r
ggplot(mpg, aes(displ, hwy)) + 
  geom_point() +
  stat_smooth(geom = "errorbar")
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-51-4.png" width="672" />

```r
ggplot(mpg, aes(displ, hwy)) + 
  geom_point() +
  stat_smooth(geom = "crossbar")
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-51-5.png" width="672" />

7. Usando o `mpg` e `facet_grid`, crie um scatterplot que contenha `displ` no eixo x, `hwy` no eixo y, `class` na cor, `drv` nas facetas-coluna e `cyl` nas facetas linha.

Esse aqui é para demonstrar o uso de `facet_grid`, que permite especificar fatores de classificação diferentes nas linhas e colunas, diferente de `facet_wrap` mostrado na aula, que só permite especificar uma dimensão.


```r
ggplot(mpg, aes(x = displ, y = hwy, color = class)) +
  geom_point() +
  facet_grid(cyl ~ drv)
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-52-1.png" width="672" />

8. Você acha que os dois gráficos abaixo ficarão diferentes um do outro? Porque? Tente responder antes de rodar o código.


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point() + 
  geom_smooth()
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-53-1.png" width="672" />

```r
ggplot() + 
  geom_point(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_smooth(data = mpg, mapping = aes(x = displ, y = hwy))
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-53-2.png" width="672" />

Mesmo antes de rodar o código, o observador astuto notará que os mapeamentos locais no segundo gráfico são idênticos entre si e aos mapeamentos globais, então os dois gráficos são iguais.

9. Tente recriar o seguinte gráfico

O objetivo dessa era fazer vocês fuçarem um pouco na ajuda para tentar recriar o mais fielmente possível o gráfico final. Não precisava ter acertado, o objetivo era chegar o mais próximo possível.


```r
ggplot(mpg, aes(displ, hwy, color = drv)) +
  geom_point() +
  geom_smooth(method = lm, se = FALSE) +
  labs(x = "Rodovia", y = "Toneladas", color = "Tração") +
  scale_color_brewer(palette = "Set1")
```

```
## `geom_smooth()` using formula 'y ~ x'
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-54-1.png" width="672" />

10. Transforme o gráfico seguir em um gráfico de pizza usando `coord_polar`.


```r
ggplot(diamonds, aes(cut, fill = cut)) +
  geom_bar()
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-55-1.png" width="672" />

Depois de simplesmente especificar `coord_polar`, em geral o gráfico fica meio estranho, não tem aquela cara bonita de pizza. É preciso corrigir os seguintes problemas:

- A largura das barras deve ser igual a proporção das contagens, mas a altura deve ser igual a 1! Portanto, eu inverto as coisas e passo as contagens/proporções para "x" e "y" fica com o valor fixo = 1.


```r
ggplot(diamonds, 
       aes(
         # calculando as proporções do total,
         # também funciona com o padrão stat(count)
         x = stat(count)/sum(stat(count)),
         y = 1, # altura igual a 1
         fill = cut)) + # cores
  geom_bar() +
  coord_polar() + # coordenadas polares
  # opcional: remover aspectos do tema para um visual mais clean
  theme_void()
```

<img src="/courses/tidyverse/solucoes_files/figure-html/unnamed-chunk-56-1.png" width="672" />

Como desafio, tentem adicionar elementos textuais das proporções no gráfico. O problema a ser resolvido é como posicionar o texto num sistema de coordenadas polares. Boa sorte!

Gráficos de pizza são polêmicos na análise de dados porque nossos olhos não captam bem diferenças entre formatos curvos e complexos, então a comparação entre as categorias fica prejudicada se houverem mais de 2 ou 3. Eu sempre dou preferência para barras. Há um tipo de gráfico de pizza melhorzinho chamado "donut plot", em que o meio do círculo é oco, mas eu ainda prefiro as barras.
