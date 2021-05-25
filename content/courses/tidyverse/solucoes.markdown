---
title: "Soluções"
# subtitle: ""
# summary: ""
# authors: []
# tags: []
# categories: []
date: 2021-05-25T11:30:29-03:00
lastmod: 2021-05-25T11:30:29-03:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
# image:
#   caption: ""
#   focal_point: ""
#   preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
# projects: []
---

## Soluções dos exercícios

### `readr`, `tibble`, `tidyr`


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
