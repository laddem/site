---
date: 2021-05-23
title: purrr
type: book
weight: 50
draft: false
---

O autor dos pacotes do `tidyverse` core é completamente fascinado por gatos, por isso, tantas referências ao bixo nos nomes dos pacotes. "Purr" (em inglês) é o som que os gatos fazem quando sentem prazer. Pra não ficar uma coisa solta no começo da aula, aqui uma foto de gatinho pra vocês:

![O Vizinho da Luciana](/courses/tidyverse/dia4_files/gato-de-buteco.jpeg)

<!-- Inciting incident -->

O assunto da aula de hoje é talvez um pouco mais abstrato do que as aulas anteriores. Vamos falar bastante de funções, loops e programação funcional. São termos que fazem parte do jargão da computação, mas que mesmo usuários veteranos do R como software estatístico para análise de dados podem ter pouca familiaridade. Ao invés de começar definindo o que vamos fazer, vou começar definindo o que não vamos.

- Não vamos revisar a fundo o assunto de iterações. Os livros fazem um bom serviço, é um assunto espinhoso e não basta compreender os conceitos, tem que botar a mão na massa pra ter um entendimento não apenas da teoria, mas também para conseguir resolver os muitos problemas que aparecem quando você está construindo iterações com maior nível de complexidade.

- Não vamos cavar fundo em todos os aspectos da programação funcional ou de todas as funções do purrr. São muitas e temos poucas horas.

Agora o que SIM vamos fazer é revisar muito brevemente a sintaxe de um for loop, ver em que situações a gente o utilizaria e como você pode substituir seus vários for loops por funções no purrr, com exemplos de aplicação quando possível. Se der tempo, vamos entrar um pouco na ideia de utilizar programação funcional para resolver problemas mais genéricos em que precisamos generalizar alguma tarefa.


```r
# Primeiro, nossos pacotes!
library(tidyverse) # o purrr já é carregado automaticamente junto com os outros,
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

```r
# Se quiser carregar apenas o purrr, descomente a linha abaixo
# library(purrr)
```


## Iterações

Do ponto de vista prático, uma iteração é uma repetição uma linha de código de tal forma que apenas uma pequena parte previsível do código muda entre uma repetição e outra. Por exemplo, digamos que você vai calcular uma soma de `x`.


```r
x <- runif(100, 0, 1000)
x
```

```
##   [1] 225.093967 466.946190 883.812438  87.936952 817.297665 959.868374
##   [7] 238.111861 832.283389 517.254072 102.303256 429.861933 994.676597
##  [13]  80.038906  91.034775 315.595919 669.703517 666.710230 425.227435
##  [19] 497.996177 169.809868 677.356086 627.092270  59.883331 766.776678
##  [25] 149.171108 905.846157 544.019980 536.797568 155.848320 622.688184
##  [31] 967.633909 585.653757 699.380320 189.194041 768.424587 571.151547
##  [37] 794.404194 773.359030  59.930340 432.662018 560.213616 676.552325
##  [43] 865.665243 534.298338 184.087156 378.304177 979.344523 455.709585
##  [49] 385.905114 158.846492 508.830850 522.506361 611.013883 926.345360
##  [55] 476.592277 468.897322 559.445858 927.857062 424.459786 848.563421
##  [61] 690.474162 836.675233 226.751909 965.321906  42.961176 578.897402
##  [67] 278.689574  97.974729 847.269263 351.362806 617.868732 228.979437
##  [73] 217.598302 279.653980 602.528267 902.524905 109.103549 874.569871
##  [79]   3.867538 246.845225 971.238817 460.380726 858.708653 273.029658
##  [85]  79.749562 889.292917 775.435723 623.977954 376.979606  17.145635
##  [91] 648.954440 591.458843 806.026611 252.165876  22.365448 657.280677
##  [97] 999.726557 189.202339 267.494455 940.787127
```

Esqueça, por um instante, a função `sum`. O cálculo da soma se dá pela soma de todos os elementos do vetor `x`. Então, é necessário repetir a operação de soma através da acumulação dos valores. Veja que é tudo totalmente previsível, cada nova repetição simplesmente adiciona mais um valor ao vetor original e esse valor pode ser encontrado na próxima posição de `x`. Esse é o típico caso de iteração. No R, a melhor prática é inicializar uma variável antes e salvar os resultados da iteração nela.


```r
# Inicialização
result <- x[[1]]

# Sequência
for (i in 2:length(x)) {
  # Corpo
  result <- result + x[[i]]
}

# Comparando os dois resultados
result
```

```
## [1] 51513.6
```

```r
sum(x)
```

```
## [1] 51513.6
```

Outro exemplo de mesmo tipo é repetição de uma mesma operação em vários vetores/colunas/variáveis comuns. Por exemplo, se eu tenho um `data.frame` com três colunas numéricas e eu gostaria de calcular a soma de cada uma.


```r
# Data.frame
df <- tibble(x = rnorm(100, 50, 25),
             y = rnorm(100, 100, 25),
             z = rnorm(100, 200, 25))

# Inicialização
result <- vector(mode = "double", length = length(df))

# Sequência
for (i in seq_along(df)) {
  result[[i]] <- sum(df[[i]])
}

# Resultado
result
```

```
## [1]  5143.487  9739.920 19452.666
```

Essa é a minha revisão de 5 minutos de iteração em R usando `for` loops. Como já é de costume, tem muito mais. Por exemplo, existe um outro tipo de iterador no R básico chamado `while`, que tem um funcionamento diferente do `for`. Ao invés de você ter um resultado de tamanho previsível, você pode ter um resultado de tamanho desconhecido. Vou deixar o `while` para vocês pesquisarem porque o bom e velho `for` costuma cobrir a maioria dos casos de uso do cientista de dados.

## Programação funcional

Beleza, agora que já dominamos (ou não) o `for` loop, encontramos várias situações em que a gente gostaria de realizar a mesma operação várias vezes, mas, o `for` loop é como uma feijoada: é gostoso, mas é pesado. `For` loops em geral são "verbosos", você precisa escrever bastante para chegar em um determinado resultado e, depois de escrever alguns, você cansa de ter que repetir todos os pedaços dele. E se você pudesse **abstrair** o loop para uma função? Aí você não precisaria escrever toda aquela parafernalha.


```r
soma <- function(x) {
  result <- 0
  for (i in 1:length(x)) {
    result <- result + x[[i]]
  }
  result
}

soma(x)
```

```
## [1] 51513.6
```

Ao fazer isso, eu ganho duas vantagens:

- Manutenção: sempre que eu precisar repetir a operação, eu consigo simplificar muito meu código. Não preciso escrever um for loop para cada soma que eu precisar fazer. Se meus requerimentos mudarem no futuro, eu só preciso mudar um pedaço de código.

- Leitura: o humano que lê um `for` loop vai precisar de um minuto para se familiarizar com a operação e entender o que está sendo iterado, calculado, etc. O humano que lê "soma" sabe que ocorrerá uma soma. Você alinha a **expectativa** com a **execução**.

Outro exemplo.


```r
soma_xyz <- function(x) {
  soma_xyz <- vector(mode = "double", length = nrow(x))
  for (i in seq_along(x[[1]])) {
    soma_xyz[[i]] <- x[[1]][[i]] + x[[2]][[i]] + x[[3]][i]
  }
  soma_xyz
}

df$soma_xyz2 <- soma_xyz(df)
df
```

```
## # A tibble: 100 x 4
##        x     y     z soma_xyz2
##    <dbl> <dbl> <dbl>     <dbl>
##  1  31.0  91.0  182.      304.
##  2  51.9  91.1  188.      331.
##  3  51.3  85.1  229.      365.
##  4  80.0 139.   179.      398.
##  5  80.5  50.1  228.      359.
##  6  59.8 122.   231.      413.
##  7  69.4 135.   242.      446.
##  8  16.5  76.6  234.      327.
##  9  69.2 107.   189.      365.
## 10  51.6  75.2  234.      361.
## # ... with 90 more rows
```

Outra vantagem de ter na mão uma função, é que eu posso me apropriar das ferramentas de programação funcional do R. São as funções da família `apply`, que recebem uma lista de objetos e aplicam uma função em cada um. Basicamente, aquela **abstração** do for loop que estavamos buscando.

Vejam como eu posso recriar o exemplo das somas das colunas do `data.frame` usando a função soma.


```r
sapply(df, soma)
```

```
##         x         y         z soma_xyz2 
##  5143.487  9739.920 19452.666 34336.074
```

Eu sei, o exemplo é muito simples pra ter uma aplicação real. Até porque já vimos como fazer esse tipo de operação com `summarize` lá atrás. Mas talvez, na hora que você estiver realizando alguma operação de repetição, você se lembre dessa possibilidade e ela lhe seja útil.

### o pacote `purrr`

Até o momento, nos limitamos a utilizar funções presentes no `base`, mas o verdadeiro intuito dessa aula é introduzir as facilidade trazidas pelas funções do `tidyverse`. No pacote `purrr`, são importadas funções com diversas funcionalidades que facilitam o trabalho com objetos mais complexos, como listas (pense saída de modelo), iterações envolvendo mais de um argumento (pense escrever diversos objetos em diversos arquivos distintos) e assim sucessivamente. Abaixo, segue uma lista das funções do pacote com uma descrição curta.

- a família `map_()`: similar a família `apply` do R base. Recebe uma lista de objetos de entrada e uma função e devolve uma lista com os resultados.

- `map2_()`: mesma coisa, só que você pode passar duas listas de objetos e uma função que requer dois argumentos variáveis. `pmap()` é a generalização, em que você passa `p` listas e uma função que pede `p` argumentos.

- `imap()`: aplica uma função tanto ao vetor, como a seu nome/índice. É meio louco, mas é muito útil para alguns casos de uso.

- `modify_()`: permite aplicar alterações no objeto estilo o que você faria com uma pipeline de `filter %>% mutate`

- `safely()`: e suas irmãs `possibly()` e `quietly()` ajudam a obter resultados mesmo quando há ocorre um erro na iteração.

- `transpose()`, `flatten_`: manipulam e reformatam listas.

- `invoke_map()`: aplica uma lista de funções a uma (opcional) segunda lista de parâmetros.

- `reduce()` e `accumulate()`: generalizam as operações de sumarização (pense soma acumulada, média, limite, fatorial)

- `pluck()`: pesca elementos de uma lista com uma sintaxe linear e bonitinha.

- [E muito mais coisas](https://purrr.tidyverse.org/reference/index.html)

No interesse de ir direto ao ponto e não tentar ser exaustivo, vamos ver algumas delas em mais detalhe.

#### A função `map`

Essa função é praticamente uma cópia de `lapply`. 


```r
df %>% lapply(sum)
```

```
## $x
## [1] 5143.487
## 
## $y
## [1] 9739.92
## 
## $z
## [1] 19452.67
## 
## $soma_xyz2
## [1] 34336.07
```

```r
df %>% map(sum)
```

```
## $x
## [1] 5143.487
## 
## $y
## [1] 9739.92
## 
## $z
## [1] 19452.67
## 
## $soma_xyz2
## [1] 34336.07
```

Embora existam pequenas diferenças técnicas entre elas, a principal vantagem de `map` é a possibilidade de criar funções anônimas com uma síntaxe enxuta. Pense, por exemplo, em elevar ao quadrado.


```r
df %>% lapply(function(x) x ^ 2)
```

```
## $x
##   [1]   963.798425  2689.631568  2636.289299  6407.686421  6483.401508
##   [6]  3576.407300  4821.200204   271.838261  4794.752257  2663.713324
##  [11]  1269.154807  2810.472272  7733.236266  6633.116709  5544.943339
##  [16]  1276.384278   474.645743   530.431720  3967.496532  3503.986006
##  [21]  4195.830138  6536.191280  2123.966230  1896.230863   320.311269
##  [26]  2713.855093  5381.268996  2205.452174  1047.781254   826.682084
##  [31]  2402.199045  5385.394322  2185.874731  5499.186633  8254.465662
##  [36]  3073.557849  5293.252928  2556.876189  3757.490933  1106.909116
##  [41]  1172.640022  7124.351428   875.948131  1428.390668  1426.113597
##  [46]   644.544767  3285.989441   853.645977     1.357369  2407.567674
##  [51]  5042.688807  5971.962098  4296.292935   124.644964   128.512784
##  [56]  7094.788796  6758.841939  3363.246840   881.306860  5323.336414
##  [61]  3396.128177  6276.881787  1185.356939  2587.379920   838.544288
##  [66]   882.065801  3046.336401  2404.123388  1843.174212  5069.910793
##  [71]  1426.091409   911.092383 12730.733608  6053.560724  3040.006719
##  [76]   658.250295  3402.752115  1192.989288   702.982204   959.240469
##  [81]  2427.061850  2819.283784 10924.963453  2645.464063   561.923775
##  [86] 12385.985694  3377.797601  1343.722757   383.297474   176.408849
##  [91]  4730.214023   955.567165   199.646662  6515.078501  1036.390327
##  [96]  1897.596732  2503.095741   236.412280  5393.150328  7001.526245
## 
## $y
##   [1]  8284.074  8303.088  7248.696 19391.930  2513.737 14851.220 18185.189
##   [8]  5865.938 11469.122  5650.669 10981.344  9473.321  9651.519  4452.080
##  [15]  6911.495  5374.510 11412.073  8851.945 10147.838  8415.336  6150.245
##  [22] 10513.858 11171.249  7657.689  3992.072  5309.097  9101.852  5460.790
##  [29] 10432.513 18788.381  4017.091  4116.322  3185.162 17520.537  3186.135
##  [36] 20143.963 10421.231 13894.456  7658.279 13681.015 15217.759  7052.142
##  [43] 12370.005 18450.451  5580.275 27559.470  9870.916 16777.651 14345.106
##  [50] 12084.487 11485.295  8327.790 26203.364  5477.594  8668.665 13172.393
##  [57] 12254.427  8711.024  9961.140  1268.758  7689.774 15604.880  9159.717
##  [64]  2758.645 10634.015  7599.150 14277.682  9006.049 11517.774 14155.260
##  [71] 10073.476  9203.164  4397.964  9118.102  6600.212 22169.380  7813.854
##  [78]  4461.388 10345.651 18106.926  7387.860  4662.773 20930.433 20457.998
##  [85]  9300.546 10149.361  7864.841  7646.981  7404.943  7709.098  7451.508
##  [92]  6907.973  6798.440  2214.701 12265.324  8910.786  8559.719  5810.114
##  [99]  4509.739 24625.748
## 
## $z
##   [1] 33154.90 35301.39 52433.74 32066.30 52077.27 53418.40 58521.86 54728.30
##   [9] 35690.17 54670.05 30497.10 29415.67 43156.04 32921.73 41698.45 55566.30
##  [17] 43630.23 49193.06 54089.95 38195.18 34639.45 30576.09 49807.56 40702.65
##  [25] 20512.02 30184.61 36949.72 58733.16 26796.23 47936.82 51652.77 33534.37
##  [33] 35970.26 23678.10 48648.95 33974.30 52570.99 30830.01 35836.79 36227.24
##  [41] 35372.95 34173.24 33564.92 31669.90 26779.00 31282.84 48986.23 43579.62
##  [49] 35944.99 29879.19 35826.63 26734.84 28090.00 20681.89 27921.49 40456.99
##  [57] 41507.86 24353.79 46273.41 42284.43 40001.73 41300.76 39745.56 27884.99
##  [65] 34581.60 34593.96 51853.12 50778.22 30045.59 37364.14 40176.84 20330.91
##  [73] 20957.48 43953.10 53487.80 33438.31 33781.46 47470.47 32914.79 33808.00
##  [81] 44550.95 32464.52 44696.09 36404.34 41732.33 17360.01 46148.44 55470.45
##  [89] 33890.15 31730.22 36964.04 47231.90 24258.20 36893.01 42387.00 48762.97
##  [97] 37281.18 43244.78 34961.56 34329.15
## 
## $soma_xyz2
##   [1]  92505.32 109474.68 133567.02 158701.52 128781.37 170397.94 199094.67
##   [8] 106940.59 133412.35 130031.19  99257.32 103590.54 155173.98 108643.62
##  [15] 130900.51 118861.27 113900.96 114860.63 157051.32 119969.06 108448.68
##  [22] 128338.79 140592.54 110757.80  50310.62  89218.94 130309.35 131920.52
##  [29]  88926.02 148046.10 115372.46 102827.80  85760.45 129887.12 135324.67
##  [36] 145687.48 163315.25 118353.26 114322.46 115988.37 119495.42 124780.60
##  [43] 104991.71 123613.33  76235.66 135621.24 142887.30 135058.53  96427.49
##  [50] 110126.02 135027.30 110252.64 156041.87  52435.15  73733.57 160112.69
##  [57] 157328.59  94484.55 118752.53  98729.56 119696.62 165952.07 108569.13
##  [64]  73103.77 101149.61  91728.55 161922.28 136362.02 104710.31 147054.80
##  [71] 114630.97  72201.81 104920.71 146645.60 135168.17 127743.12 109247.74
##  [78] 101895.35  95884.19 122082.54 119915.96  90938.77 212162.11 148428.93
##  [85] 105254.30 118194.15 130772.38 129330.50  83939.26  77959.99 120658.28
##  [92] 109796.45  63671.81 102305.42 121677.47 128724.35 112649.55  89732.36
##  [99] 107303.92 181375.75
```

```r
df %>% map(~ .x ^ 2)
```

```
## $x
##   [1]   963.798425  2689.631568  2636.289299  6407.686421  6483.401508
##   [6]  3576.407300  4821.200204   271.838261  4794.752257  2663.713324
##  [11]  1269.154807  2810.472272  7733.236266  6633.116709  5544.943339
##  [16]  1276.384278   474.645743   530.431720  3967.496532  3503.986006
##  [21]  4195.830138  6536.191280  2123.966230  1896.230863   320.311269
##  [26]  2713.855093  5381.268996  2205.452174  1047.781254   826.682084
##  [31]  2402.199045  5385.394322  2185.874731  5499.186633  8254.465662
##  [36]  3073.557849  5293.252928  2556.876189  3757.490933  1106.909116
##  [41]  1172.640022  7124.351428   875.948131  1428.390668  1426.113597
##  [46]   644.544767  3285.989441   853.645977     1.357369  2407.567674
##  [51]  5042.688807  5971.962098  4296.292935   124.644964   128.512784
##  [56]  7094.788796  6758.841939  3363.246840   881.306860  5323.336414
##  [61]  3396.128177  6276.881787  1185.356939  2587.379920   838.544288
##  [66]   882.065801  3046.336401  2404.123388  1843.174212  5069.910793
##  [71]  1426.091409   911.092383 12730.733608  6053.560724  3040.006719
##  [76]   658.250295  3402.752115  1192.989288   702.982204   959.240469
##  [81]  2427.061850  2819.283784 10924.963453  2645.464063   561.923775
##  [86] 12385.985694  3377.797601  1343.722757   383.297474   176.408849
##  [91]  4730.214023   955.567165   199.646662  6515.078501  1036.390327
##  [96]  1897.596732  2503.095741   236.412280  5393.150328  7001.526245
## 
## $y
##   [1]  8284.074  8303.088  7248.696 19391.930  2513.737 14851.220 18185.189
##   [8]  5865.938 11469.122  5650.669 10981.344  9473.321  9651.519  4452.080
##  [15]  6911.495  5374.510 11412.073  8851.945 10147.838  8415.336  6150.245
##  [22] 10513.858 11171.249  7657.689  3992.072  5309.097  9101.852  5460.790
##  [29] 10432.513 18788.381  4017.091  4116.322  3185.162 17520.537  3186.135
##  [36] 20143.963 10421.231 13894.456  7658.279 13681.015 15217.759  7052.142
##  [43] 12370.005 18450.451  5580.275 27559.470  9870.916 16777.651 14345.106
##  [50] 12084.487 11485.295  8327.790 26203.364  5477.594  8668.665 13172.393
##  [57] 12254.427  8711.024  9961.140  1268.758  7689.774 15604.880  9159.717
##  [64]  2758.645 10634.015  7599.150 14277.682  9006.049 11517.774 14155.260
##  [71] 10073.476  9203.164  4397.964  9118.102  6600.212 22169.380  7813.854
##  [78]  4461.388 10345.651 18106.926  7387.860  4662.773 20930.433 20457.998
##  [85]  9300.546 10149.361  7864.841  7646.981  7404.943  7709.098  7451.508
##  [92]  6907.973  6798.440  2214.701 12265.324  8910.786  8559.719  5810.114
##  [99]  4509.739 24625.748
## 
## $z
##   [1] 33154.90 35301.39 52433.74 32066.30 52077.27 53418.40 58521.86 54728.30
##   [9] 35690.17 54670.05 30497.10 29415.67 43156.04 32921.73 41698.45 55566.30
##  [17] 43630.23 49193.06 54089.95 38195.18 34639.45 30576.09 49807.56 40702.65
##  [25] 20512.02 30184.61 36949.72 58733.16 26796.23 47936.82 51652.77 33534.37
##  [33] 35970.26 23678.10 48648.95 33974.30 52570.99 30830.01 35836.79 36227.24
##  [41] 35372.95 34173.24 33564.92 31669.90 26779.00 31282.84 48986.23 43579.62
##  [49] 35944.99 29879.19 35826.63 26734.84 28090.00 20681.89 27921.49 40456.99
##  [57] 41507.86 24353.79 46273.41 42284.43 40001.73 41300.76 39745.56 27884.99
##  [65] 34581.60 34593.96 51853.12 50778.22 30045.59 37364.14 40176.84 20330.91
##  [73] 20957.48 43953.10 53487.80 33438.31 33781.46 47470.47 32914.79 33808.00
##  [81] 44550.95 32464.52 44696.09 36404.34 41732.33 17360.01 46148.44 55470.45
##  [89] 33890.15 31730.22 36964.04 47231.90 24258.20 36893.01 42387.00 48762.97
##  [97] 37281.18 43244.78 34961.56 34329.15
## 
## $soma_xyz2
##   [1]  92505.32 109474.68 133567.02 158701.52 128781.37 170397.94 199094.67
##   [8] 106940.59 133412.35 130031.19  99257.32 103590.54 155173.98 108643.62
##  [15] 130900.51 118861.27 113900.96 114860.63 157051.32 119969.06 108448.68
##  [22] 128338.79 140592.54 110757.80  50310.62  89218.94 130309.35 131920.52
##  [29]  88926.02 148046.10 115372.46 102827.80  85760.45 129887.12 135324.67
##  [36] 145687.48 163315.25 118353.26 114322.46 115988.37 119495.42 124780.60
##  [43] 104991.71 123613.33  76235.66 135621.24 142887.30 135058.53  96427.49
##  [50] 110126.02 135027.30 110252.64 156041.87  52435.15  73733.57 160112.69
##  [57] 157328.59  94484.55 118752.53  98729.56 119696.62 165952.07 108569.13
##  [64]  73103.77 101149.61  91728.55 161922.28 136362.02 104710.31 147054.80
##  [71] 114630.97  72201.81 104920.71 146645.60 135168.17 127743.12 109247.74
##  [78] 101895.35  95884.19 122082.54 119915.96  90938.77 212162.11 148428.93
##  [85] 105254.30 118194.15 130772.38 129330.50  83939.26  77959.99 120658.28
##  [92] 109796.45  63671.81 102305.42 121677.47 128724.35 112649.55  89732.36
##  [99] 107303.92 181375.75
```

Através do uso de fórmulas `~` é possível especificar funções anônimas simples economizando caracteres e utilizando o autocompletar.

Que tal esta pipeline?


```r
library(nycflights13)

# Médias de todas as variáveis numéricas usando where()
flights %>% 
  select(where(is.numeric)) %>% 
  map(mean)
```

```
## $year
## [1] 2013
## 
## $month
## [1] 6.54851
## 
## $day
## [1] 15.71079
## 
## $dep_time
## [1] NA
## 
## $sched_dep_time
## [1] 1344.255
## 
## $dep_delay
## [1] NA
## 
## $arr_time
## [1] NA
## 
## $sched_arr_time
## [1] 1536.38
## 
## $arr_delay
## [1] NA
## 
## $flight
## [1] 1971.924
## 
## $air_time
## [1] NA
## 
## $distance
## [1] 1039.913
## 
## $hour
## [1] 13.18025
## 
## $minute
## [1] 26.2301
```

Ou esta


```r
# Proporção de missings em todas as variáveis
flights %>% 
  map(is.na) %>% 
  map(mean)
```

```
## $year
## [1] 0
## 
## $month
## [1] 0
## 
## $day
## [1] 0
## 
## $dep_time
## [1] 0.02451184
## 
## $sched_dep_time
## [1] 0
## 
## $dep_delay
## [1] 0.02451184
## 
## $arr_time
## [1] 0.0258718
## 
## $sched_arr_time
## [1] 0
## 
## $arr_delay
## [1] 0.02800081
## 
## $carrier
## [1] 0
## 
## $flight
## [1] 0
## 
## $tailnum
## [1] 0.007458964
## 
## $origin
## [1] 0
## 
## $dest
## [1] 0
## 
## $air_time
## [1] 0.02800081
## 
## $distance
## [1] 0
## 
## $hour
## [1] 0
## 
## $minute
## [1] 0
## 
## $time_hour
## [1] 0
```

```r
# Contagem de missings em todas as variáveis
flights %>% 
  map(is.na) %>% 
  map(sum)
```

```
## $year
## [1] 0
## 
## $month
## [1] 0
## 
## $day
## [1] 0
## 
## $dep_time
## [1] 8255
## 
## $sched_dep_time
## [1] 0
## 
## $dep_delay
## [1] 8255
## 
## $arr_time
## [1] 8713
## 
## $sched_arr_time
## [1] 0
## 
## $arr_delay
## [1] 9430
## 
## $carrier
## [1] 0
## 
## $flight
## [1] 0
## 
## $tailnum
## [1] 2512
## 
## $origin
## [1] 0
## 
## $dest
## [1] 0
## 
## $air_time
## [1] 9430
## 
## $distance
## [1] 0
## 
## $hour
## [1] 0
## 
## $minute
## [1] 0
## 
## $time_hour
## [1] 0
```

A outra grande vantagem do map é ter acesso fácil ao controle do tipo de saída.


```r
# Vetor numérico
flights %>% 
  map(is.na) %>% 
  map_dbl(mean)
```

```
##           year          month            day       dep_time sched_dep_time 
##    0.000000000    0.000000000    0.000000000    0.024511842    0.000000000 
##      dep_delay       arr_time sched_arr_time      arr_delay        carrier 
##    0.024511842    0.025871796    0.000000000    0.028000808    0.000000000 
##         flight        tailnum         origin           dest       air_time 
##    0.000000000    0.007458964    0.000000000    0.000000000    0.028000808 
##       distance           hour         minute      time_hour 
##    0.000000000    0.000000000    0.000000000    0.000000000
```

```r
# Vetor de caracteres
flights %>% 
  map(is.na) %>% 
  map_chr(mean)
```

```
##           year          month            day       dep_time sched_dep_time 
##     "0.000000"     "0.000000"     "0.000000"     "0.024512"     "0.000000" 
##      dep_delay       arr_time sched_arr_time      arr_delay        carrier 
##     "0.024512"     "0.025872"     "0.000000"     "0.028001"     "0.000000" 
##         flight        tailnum         origin           dest       air_time 
##     "0.000000"     "0.007459"     "0.000000"     "0.000000"     "0.028001" 
##       distance           hour         minute      time_hour 
##     "0.000000"     "0.000000"     "0.000000"     "0.000000"
```

Um exemplo um pouco mais elaborado: avaliação inicial de variáveis em um modelo. Em geral, é comum rodar um modelo para cada variável numérica para ver como elas se comportam.


```r
respvar <- "hwy"
predvars <- names(select(mpg, where(is.numeric), -hwy))

tibble(
  names = predvars,
  fit = map(names, ~lm(paste0(respvar, "~", .x), data = mpg)),
  summary = map(fit, summary),
  r2 = map_dbl(summary, "r.squared"),
  FStat = map_df(summary, "fstatistic"),
  coefs = map_df(fit, coef),
)
```

```
## # A tibble: 4 x 6
##   names fit    summary      r2 FStat$value $numdf $dendf coefs$`(Interce~ $displ
##   <chr> <list> <list>    <dbl>       <dbl>  <dbl>  <dbl>            <dbl>  <dbl>
## 1 displ <lm>   <smmry~ 5.87e-1   329.           1    232           35.7    -3.53
## 2 year  <lm>   <smmry~ 4.66e-6     0.00108      1    232           17.7    NA   
## 3 cyl   <lm>   <smmry~ 5.81e-1   321.           1    232           40.0    NA   
## 4 cty   <lm>   <smmry~ 9.14e-1  2459.           1    232            0.892  NA
```

Ok, a saída não está muito bonitinha! Mas com um pouco mais de trabalho, programadores melhores que eu fizeram isso:


```r
models <- 
  tibble(
    names = predvars,
    fit = map(names, ~lm(paste0(respvar, "~", .x), data = mpg)),
    tidied = fit %>% map(broom::tidy),
    glanced = fit %>% map(broom::glance),
    augmented = fit %>% map(broom::augment)
  )

models
```

```
## # A tibble: 4 x 5
##   names fit    tidied           glanced           augmented         
##   <chr> <list> <list>           <list>            <list>            
## 1 displ <lm>   <tibble [2 x 5]> <tibble [1 x 12]> <tibble [234 x 8]>
## 2 year  <lm>   <tibble [2 x 5]> <tibble [1 x 12]> <tibble [234 x 8]>
## 3 cyl   <lm>   <tibble [2 x 5]> <tibble [1 x 12]> <tibble [234 x 8]>
## 4 cty   <lm>   <tibble [2 x 5]> <tibble [1 x 12]> <tibble [234 x 8]>
```

Grande coisa, Vinícius, o output é ilegível! Calma! Lembram da primeira aula em que eu comentei que havia mais funções no `tidyr`?


```r
# Coeficientes
models %>% 
  select(tidied) %>%
  tidyr::unnest(tidied) %>% 
  filter(term != "(Intercept)")
```

```
## # A tibble: 4 x 5
##   term  estimate std.error statistic   p.value
##   <chr>    <dbl>     <dbl>     <dbl>     <dbl>
## 1 displ -3.53       0.195   -18.2    2.04e- 46
## 2 year   0.00285    0.0867    0.0329 9.74e-  1
## 3 cyl   -2.82       0.157   -17.9    1.18e- 45
## 4 cty    1.34       0.0270   49.6    1.87e-125
```

```r
# Estatísticas do modelo
models %>% 
  select(names, glanced) %>% 
  unnest(glanced)
```

```
## # A tibble: 4 x 13
##   names  r.squared adj.r.squared sigma  statistic   p.value    df logLik   AIC
##   <chr>      <dbl>         <dbl> <dbl>      <dbl>     <dbl> <dbl>  <dbl> <dbl>
## 1 displ 0.587            0.585    3.84  329.      2.04e- 46     1  -646. 1297.
## 2 year  0.00000466      -0.00431  5.97    0.00108 9.74e-  1     1  -749. 1504.
## 3 cyl   0.581            0.579    3.87  321.      1.18e- 45     1  -647. 1301.
## 4 cty   0.914            0.913    1.75 2459.      1.87e-125     1  -462.  931.
## # ... with 4 more variables: BIC <dbl>, deviance <dbl>, df.residual <int>,
## #   nobs <int>
```

```r
# Bancos "aumentados" com valores ajustados, resíduos, distâncias de cook, etc.
models %>% 
  filter(names == "displ") %>% 
  select(names, augmented) %>% 
  unnest(augmented)
```

```
## # A tibble: 234 x 9
##    names   hwy displ .fitted .resid    .hat .sigma    .cooksd .std.resid
##    <chr> <int> <dbl>   <dbl>  <dbl>   <dbl>  <dbl>      <dbl>      <dbl>
##  1 displ    29   1.8    29.3 -0.343 0.0115    3.84 0.0000468     -0.0898
##  2 displ    29   1.8    29.3 -0.343 0.0115    3.84 0.0000468     -0.0898
##  3 displ    31   2      28.6  2.36  0.00984   3.84 0.00191        0.619 
##  4 displ    30   2      28.6  1.36  0.00984   3.84 0.000634       0.357 
##  5 displ    26   2.8    25.8  0.188 0.00543   3.84 0.00000660     0.0491
##  6 displ    26   2.8    25.8  0.188 0.00543   3.84 0.00000660     0.0491
##  7 displ    27   3.1    24.8  2.25  0.00463   3.84 0.000802       0.587 
##  8 displ    26   1.8    29.3 -3.34  0.0115    3.84 0.00445       -0.876 
##  9 displ    25   1.8    29.3 -4.34  0.0115    3.83 0.00751       -1.14  
## 10 displ    28   2      28.6 -0.636 0.00984   3.84 0.000138      -0.167 
## # ... with 224 more rows
```

Isso aqui é só uma palhinha de modelagem com o `tidyverse`, mas acho que é uma demonstração muito convincente da aplicabilidade dos exemplos que vimos usando programação funcional.

Outro exemplo mais simples com `map2`: salvar vários arquivos de uma vez só.


```r
df1 <- tibble(x = rnorm(100), y = rnorm(100), z = rnorm(100))
df2 <- tibble(x = rnorm(100), y = rnorm(100), z = rnorm(100))
df3 <- tibble(x = rnorm(100), y = rnorm(100), z = rnorm(100))
df4 <- tibble(x = rnorm(100), y = rnorm(100), z = rnorm(100))

dfs <- list(df1, df2, df3, df4)
dfs
```

```
## [[1]]
## # A tibble: 100 x 3
##          x       y       z
##      <dbl>   <dbl>   <dbl>
##  1 -0.142  -0.924   0.762 
##  2  0.405  -1.33   -0.196 
##  3 -0.0639 -0.399  -0.260 
##  4 -0.0699  0.0876  1.57  
##  5  0.827  -0.335   1.01  
##  6  0.187   0.419  -0.442 
##  7  1.14   -0.224  -0.0744
##  8 -0.514  -1.27    0.896 
##  9  0.645   0.510   1.10  
## 10 -0.810  -0.246   0.589 
## # ... with 90 more rows
## 
## [[2]]
## # A tibble: 100 x 3
##          x      y       z
##      <dbl>  <dbl>   <dbl>
##  1  1.54    0.923 -1.32  
##  2 -0.0135 -0.975 -0.0527
##  3  1.36   -0.369 -0.434 
##  4  2.25   -0.254  0.207 
##  5  1.41   -1.43  -1.19  
##  6  0.357   0.408 -0.183 
##  7  1.32    0.773  0.427 
##  8  0.622   0.379 -1.06  
##  9  1.09   -1.27  -0.699 
## 10  1.66    1.75  -1.14  
## # ... with 90 more rows
## 
## [[3]]
## # A tibble: 100 x 3
##           x       y      z
##       <dbl>   <dbl>  <dbl>
##  1 -0.306    1.11    1.28 
##  2  2.70     1.65    0.579
##  3  0.904    0.558   2.16 
##  4 -0.335    1.92   -0.144
##  5  0.443    0.521   1.07 
##  6  0.744    0.241  -1.57 
##  7 -1.48    -0.941   0.268
##  8 -0.00240  0.304   1.13 
##  9  0.368   -0.987   0.577
## 10 -2.07     0.0147  1.22 
## # ... with 90 more rows
## 
## [[4]]
## # A tibble: 100 x 3
##         x       y      z
##     <dbl>   <dbl>  <dbl>
##  1 -0.753  0.894  -1.85 
##  2  0.960  1.53    1.12 
##  3  2.23   2.12   -0.230
##  4 -1.15  -0.968   0.190
##  5  1.34   1.73    0.459
##  6 -1.44  -1.94    0.200
##  7  0.170 -2.97    0.692
##  8 -0.627  0.702  -0.512
##  9 -1.50  -0.0865 -0.407
## 10  0.633  1.15   -0.273
## # ... with 90 more rows
```

```r
paths <- sprintf("arquivo%s.csv", 1:4)
paths
```

```
## [1] "arquivo1.csv" "arquivo2.csv" "arquivo3.csv" "arquivo4.csv"
```

```r
map2(dfs, paths, ~write_csv(x = .x, file = .y))
```

```
## [[1]]
## # A tibble: 100 x 3
##          x       y       z
##      <dbl>   <dbl>   <dbl>
##  1 -0.142  -0.924   0.762 
##  2  0.405  -1.33   -0.196 
##  3 -0.0639 -0.399  -0.260 
##  4 -0.0699  0.0876  1.57  
##  5  0.827  -0.335   1.01  
##  6  0.187   0.419  -0.442 
##  7  1.14   -0.224  -0.0744
##  8 -0.514  -1.27    0.896 
##  9  0.645   0.510   1.10  
## 10 -0.810  -0.246   0.589 
## # ... with 90 more rows
## 
## [[2]]
## # A tibble: 100 x 3
##          x      y       z
##      <dbl>  <dbl>   <dbl>
##  1  1.54    0.923 -1.32  
##  2 -0.0135 -0.975 -0.0527
##  3  1.36   -0.369 -0.434 
##  4  2.25   -0.254  0.207 
##  5  1.41   -1.43  -1.19  
##  6  0.357   0.408 -0.183 
##  7  1.32    0.773  0.427 
##  8  0.622   0.379 -1.06  
##  9  1.09   -1.27  -0.699 
## 10  1.66    1.75  -1.14  
## # ... with 90 more rows
## 
## [[3]]
## # A tibble: 100 x 3
##           x       y      z
##       <dbl>   <dbl>  <dbl>
##  1 -0.306    1.11    1.28 
##  2  2.70     1.65    0.579
##  3  0.904    0.558   2.16 
##  4 -0.335    1.92   -0.144
##  5  0.443    0.521   1.07 
##  6  0.744    0.241  -1.57 
##  7 -1.48    -0.941   0.268
##  8 -0.00240  0.304   1.13 
##  9  0.368   -0.987   0.577
## 10 -2.07     0.0147  1.22 
## # ... with 90 more rows
## 
## [[4]]
## # A tibble: 100 x 3
##         x       y      z
##     <dbl>   <dbl>  <dbl>
##  1 -0.753  0.894  -1.85 
##  2  0.960  1.53    1.12 
##  3  2.23   2.12   -0.230
##  4 -1.15  -0.968   0.190
##  5  1.34   1.73    0.459
##  6 -1.44  -1.94    0.200
##  7  0.170 -2.97    0.692
##  8 -0.627  0.702  -0.512
##  9 -1.50  -0.0865 -0.407
## 10  0.633  1.15   -0.273
## # ... with 90 more rows
```

E se eu quiser ler um banco de dados que está em vários arquivos diferentes? Vamos usar os que acabamos de criar


```r
# Bum! Uma linha
paths %>% map_dfr(read_csv, .id = "arquivo")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   x = col_double(),
##   y = col_double(),
##   z = col_double()
## )
## 
## 
## -- Column specification --------------------------------------------------------
## cols(
##   x = col_double(),
##   y = col_double(),
##   z = col_double()
## )
## 
## 
## -- Column specification --------------------------------------------------------
## cols(
##   x = col_double(),
##   y = col_double(),
##   z = col_double()
## )
## 
## 
## -- Column specification --------------------------------------------------------
## cols(
##   x = col_double(),
##   y = col_double(),
##   z = col_double()
## )
```

```
## # A tibble: 400 x 4
##    arquivo       x       y       z
##    <chr>     <dbl>   <dbl>   <dbl>
##  1 1       -0.142  -0.924   0.762 
##  2 1        0.405  -1.33   -0.196 
##  3 1       -0.0639 -0.399  -0.260 
##  4 1       -0.0699  0.0876  1.57  
##  5 1        0.827  -0.335   1.01  
##  6 1        0.187   0.419  -0.442 
##  7 1        1.14   -0.224  -0.0744
##  8 1       -0.514  -1.27    0.896 
##  9 1        0.645   0.510   1.10  
## 10 1       -0.810  -0.246   0.589 
## # ... with 390 more rows
```

As possibilidades são inúmeras. A regra é a seguinte: viu uma tarefa que precisa ser repetida muitas vezes? Pense com carinho em usar uma função vetorizada através de `map_`.

#### Predicados

O objetivo de um função predicado é selecionar elementos de uma lista com base em uma função. Em geral, são funções simples que retornam um valor `TRUE` ou `FALSE` com base em alguma característica do objeto, como o tipo.


```r
l <- list(c("a", "b", "c"), c(1, 2, 3))
l %>% str()
```

```
## List of 2
##  $ : chr [1:3] "a" "b" "c"
##  $ : num [1:3] 1 2 3
```

```r
l %>% keep(is.numeric) %>% str()
```

```
## List of 1
##  $ : num [1:3] 1 2 3
```

```r
l %>% keep(is.character) %>% str()
```

```
## List of 1
##  $ : chr [1:3] "a" "b" "c"
```

O uso mais frequente que vejo de `keep` e `discard` é como uma espécie de atalho para `select`. Como `data.frames` são secretamente listas, você pode rapidamente selecionar todas as variáveis do banco que tenham o mesmo tipo.


```r
flights %>% keep(is.numeric)
```

```
## # A tibble: 336,776 x 14
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
## # ... with 336,766 more rows, and 6 more variables: arr_delay <dbl>,
## #   flight <int>, air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>
```

```r
flights %>% discard(is.character)
```

```
## # A tibble: 336,776 x 15
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
## # ... with 336,766 more rows, and 7 more variables: arr_delay <dbl>,
## #   flight <int>, air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>,
## #   time_hour <dttm>
```

Como outros aspectos do `purrr`, a grande vantagem aqui é a generalidade. Essas funções funcionam em qualquer lista de objetos e não apenas em `data.frames`.

#### Lidando com a presença de erros

A próxima dica forte é utilizar os advérbios (chique) `safely()` e companhia para lidar com erros.


```r
x <- list(a = c(1,2,3), b = c("a", "b", "c"), c = c(99, 88, 77))

x %>% map(sum)
```

```
## Error in .Primitive("sum")(..., na.rm = na.rm): 'type' inválido (character) do argumento
```

Não conseguimos nada! A mensagem de erro nem nos informa onde o problema ocorreu. E agora?


```r
x %>% map(safely(sum)) %>% str()
```

```
## List of 3
##  $ a:List of 2
##   ..$ result: num 6
##   ..$ error : NULL
##  $ b:List of 2
##   ..$ result: NULL
##   ..$ error :List of 2
##   .. ..$ message: chr "'type' inválido (character) do argumento"
##   .. ..$ call   : language .Primitive("sum")(..., na.rm = na.rm)
##   .. ..- attr(*, "class")= chr [1:3] "simpleError" "error" "condition"
##  $ c:List of 2
##   ..$ result: num 264
##   ..$ error : NULL
```

```r
x %>% map(possibly(sum, otherwise = NA_real_)) %>% str()
```

```
## List of 3
##  $ a: num 6
##  $ b: num NA
##  $ c: num 264
```

Essas funções permitem alterar a saída de uma função quando ocorrer um erro. `quietly` é parecido, mas ela captura mais a saída do R, o texto em si.


```r
x <- list(1, -1)
x %>% map(quietly(log)) %>% str()
```

```
## List of 2
##  $ :List of 4
##   ..$ result  : num 0
##   ..$ output  : chr ""
##   ..$ warnings: chr(0) 
##   ..$ messages: chr(0) 
##  $ :List of 4
##   ..$ result  : num NaN
##   ..$ output  : chr ""
##   ..$ warnings: chr "NaNs produzidos"
##   ..$ messages: chr(0)
```

#### Chamando listas de funções

Vamos supor que você tem um vetor de números e você quer aplicar um monte de funções diferentes nele, mas você não quer ter que ficar copiando e colando tudo.


```r
numeros <- rnorm(25, 50, 10)
numeros
```

```
##  [1] 42.20048 38.07047 40.98823 66.99693 47.80056 45.23199 58.43128 45.39271
##  [9] 58.51740 57.56044 61.02015 34.67480 57.13596 49.33671 21.79235 48.01163
## [17] 48.88481 55.32602 33.49729 49.72942 34.46436 56.32152 67.10447 53.74201
## [25] 48.43169
```

Nessas situações, você gostaria de chamar várias funções no mesmo objeto ou grupo de objetos, mas você não sabe bem como fazer isso... Então, `invoke_map` vem ao resgate.


```r
funcs <- list("mean", "sd", "IQR", "sum", "length", "range", "min", "max")
args <- list(list(na.rm = T, trim = 0.05),
             list(na.rm=T), 
             list(type = 2),
             list(na.rm = T), 
             list(),
             list(na.rm = T), 
             list(na.rm = T), 
             list(na.rm = T))
funcs %>% str()
```

```
## List of 8
##  $ : chr "mean"
##  $ : chr "sd"
##  $ : chr "IQR"
##  $ : chr "sum"
##  $ : chr "length"
##  $ : chr "range"
##  $ : chr "min"
##  $ : chr "max"
```

```r
args %>% str()
```

```
## List of 8
##  $ :List of 2
##   ..$ na.rm: logi TRUE
##   ..$ trim : num 0.05
##  $ :List of 1
##   ..$ na.rm: logi TRUE
##  $ :List of 1
##   ..$ type: num 2
##  $ :List of 1
##   ..$ na.rm: logi TRUE
##  $ : list()
##  $ :List of 1
##   ..$ na.rm: logi TRUE
##  $ :List of 1
##   ..$ na.rm: logi TRUE
##  $ :List of 1
##   ..$ na.rm: logi TRUE
```

```r
invoke_map(funcs, args, x = numeros) %>% glimpse()
```

```
## List of 8
##  $ : num 49.2
##  $ : num 11
##  $ : num 14.9
##  $ : num 1221
##  $ : int 25
##  $ : num [1:2] 21.8 67.1
##  $ : num 21.8
##  $ : num 67.1
```

Fica mais bonito se você nomear os argumentos, ai o output fica melhorzinho.


```r
names(funcs) <- unlist(funcs)

invoke_map(funcs, args, x = numeros) %>% glimpse()
```

```
## List of 8
##  $ mean  : num 49.2
##  $ sd    : num 11
##  $ IQR   : num 14.9
##  $ sum   : num 1221
##  $ length: int 25
##  $ range : num [1:2] 21.8 67.1
##  $ min   : num 21.8
##  $ max   : num 67.1
```

Claro que esse exemplo aqui talvez não seja útil para vocês, mas espero que quando vocês encontrarem uma situação em que você gostaria de aplicar uma bateria de funções no mesmo objeto ou grupo de objetos, vocês se lembrem dessa possibilidade.

#### `reduce` e `accumulate`

Essas duas funções são desenhadas para fazer aplicações recursivas de funções que recebem dois argumentos (operadores matemáticos, `_join`s, etc. Recursividade em linhas gerais, é você repetir uma operação com o resultado da repetição anterior. Pense em somas ou produtos acumulados, em que o próximo valor é determinado pela aplicação de uma regra sobre o anterior.


```r
accumulate(1:10, `+`)
```

```
##  [1]  1  3  6 10 15 21 28 36 45 55
```

```r
accumulate(1:10, `*`)
```

```
##  [1]       1       2       6      24     120     720    5040   40320  362880
## [10] 3628800
```

A principal diferença entre `accumulate` e `reduce` é que a primeira guarda os resultados intermediários, enquanto a segunda retorna apenas o último.


```r
reduce(1:10, `+`)
```

```
## [1] 55
```

```r
reduce(1:10, `*`)
```

```
## [1] 3628800
```

Uma aplicação bastante prática dessas funções é a produção de um único banco de dados a partir de vários arquivos separados.


```r
# Lembra dos nossos arquivos lá em cima?
paths
```

```
## [1] "arquivo1.csv" "arquivo2.csv" "arquivo3.csv" "arquivo4.csv"
```

Vamos fingir que não temos acesso a `map_dfr` e precisamos importar esses objetos e todos compõem um único banco. Imaginaem que tratam-se de dados por ano.


```r
dfs <- map(paths, read_csv)
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   x = col_double(),
##   y = col_double(),
##   z = col_double()
## )
## 
## 
## -- Column specification --------------------------------------------------------
## cols(
##   x = col_double(),
##   y = col_double(),
##   z = col_double()
## )
## 
## 
## -- Column specification --------------------------------------------------------
## cols(
##   x = col_double(),
##   y = col_double(),
##   z = col_double()
## )
## 
## 
## -- Column specification --------------------------------------------------------
## cols(
##   x = col_double(),
##   y = col_double(),
##   z = col_double()
## )
```

```r
glimpse(dfs)
```

```
## List of 4
##  $ : spec_tbl_df [100 x 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##   ..$ x: num [1:100] -0.1423 0.405 -0.0639 -0.0699 0.8265 ...
##   ..$ y: num [1:100] -0.9244 -1.3257 -0.3995 0.0876 -0.3349 ...
##   ..$ z: num [1:100] 0.762 -0.196 -0.26 1.568 1.009 ...
##   ..- attr(*, "spec")=
##   .. .. cols(
##   .. ..   x = col_double(),
##   .. ..   y = col_double(),
##   .. ..   z = col_double()
##   .. .. )
##  $ : spec_tbl_df [100 x 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##   ..$ x: num [1:100] 1.5431 -0.0135 1.3621 2.2513 1.4092 ...
##   ..$ y: num [1:100] 0.923 -0.975 -0.369 -0.254 -1.433 ...
##   ..$ z: num [1:100] -1.3211 -0.0527 -0.4336 0.2069 -1.1874 ...
##   ..- attr(*, "spec")=
##   .. .. cols(
##   .. ..   x = col_double(),
##   .. ..   y = col_double(),
##   .. ..   z = col_double()
##   .. .. )
##  $ : spec_tbl_df [100 x 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##   ..$ x: num [1:100] -0.306 2.702 0.904 -0.335 0.443 ...
##   ..$ y: num [1:100] 1.109 1.646 0.558 1.918 0.521 ...
##   ..$ z: num [1:100] 1.276 0.579 2.159 -0.144 1.069 ...
##   ..- attr(*, "spec")=
##   .. .. cols(
##   .. ..   x = col_double(),
##   .. ..   y = col_double(),
##   .. ..   z = col_double()
##   .. .. )
##  $ : spec_tbl_df [100 x 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##   ..$ x: num [1:100] -0.753 0.96 2.231 -1.145 1.338 ...
##   ..$ y: num [1:100] 0.894 1.526 2.123 -0.968 1.732 ...
##   ..$ z: num [1:100] -1.849 1.123 -0.23 0.19 0.459 ...
##   ..- attr(*, "spec")=
##   .. .. cols(
##   .. ..   x = col_double(),
##   .. ..   y = col_double(),
##   .. ..   z = col_double()
##   .. .. )
```

```r
new_df <- reduce(dfs, bind_rows)
new_df
```

```
## # A tibble: 400 x 3
##          x       y       z
##      <dbl>   <dbl>   <dbl>
##  1 -0.142  -0.924   0.762 
##  2  0.405  -1.33   -0.196 
##  3 -0.0639 -0.399  -0.260 
##  4 -0.0699  0.0876  1.57  
##  5  0.827  -0.335   1.01  
##  6  0.187   0.419  -0.442 
##  7  1.14   -0.224  -0.0744
##  8 -0.514  -1.27    0.896 
##  9  0.645   0.510   1.10  
## 10 -0.810  -0.246   0.589 
## # ... with 390 more rows
```

O resultado é o mesmo observado em `map_dfr`, mas essa abordagem é **genérica**, ou seja, ela se aplica para qualquer grupo de objetos e para qualquer função que recebe dois argumentos e precisa ser repetida de maneira **recursiva**.


```r
# Apaga os arquivos pra eles não ficarem ai gastando memória a toa
file.remove(paths)
```

```
## [1] TRUE TRUE TRUE TRUE
```

#### `pluck`

Esta função é um atalho para as tesouras duplas do r `[[`. Seu objeto é facilitar a **leitura** de códigos que pescam elementos profundos de uma lista aninhada.


```r
l <- list(
  list(-1, x = 1, y = c(2), z = "a"),
  list(-2, x = 4, y = c(5, 6), z = "b"),
  list(-3, x = 8, y = c(9, 10, 11))
)

glimpse(l)
```

```
## List of 3
##  $ :List of 4
##   ..$  : num -1
##   ..$ x: num 1
##   ..$ y: num 2
##   ..$ z: chr "a"
##  $ :List of 4
##   ..$  : num -2
##   ..$ x: num 4
##   ..$ y: num [1:2] 5 6
##   ..$ z: chr "b"
##  $ :List of 3
##   ..$  : num -3
##   ..$ x: num 8
##   ..$ y: num [1:3] 9 10 11
```

Se você chamar `map_` nessa lista, você pode extrair os elementos delas pelo nome.


```r
map_dbl(l, "x")
```

```
## [1] 1 4 8
```

```r
map(l, "y")
```

```
## [[1]]
## [1] 2
## 
## [[2]]
## [1] 5 6
## 
## [[3]]
## [1]  9 10 11
```

```r
map(l, "z")
```

```
## [[1]]
## [1] "a"
## 
## [[2]]
## [1] "b"
## 
## [[3]]
## NULL
```

Ou pela posição na lista


```r
map_dbl(l, 1)
```

```
## [1] -1 -2 -3
```

```r
map_dbl(l, 2)
```

```
## [1] 1 4 8
```

```r
map(l, 3)
```

```
## [[1]]
## [1] 2
## 
## [[2]]
## [1] 5 6
## 
## [[3]]
## [1]  9 10 11
```

Ou os dois


```r
map_dbl(l, list("x", 1))
```

```
## [1] 1 4 8
```

```r
map(l, list("z", 1))
```

```
## [[1]]
## [1] "a"
## 
## [[2]]
## [1] "b"
## 
## [[3]]
## NULL
```

```r
map(l, list("y", 3))
```

```
## [[1]]
## NULL
## 
## [[2]]
## NULL
## 
## [[3]]
## [1] 11
```

Se um componente não existir, você recebe um erro


```r
map_chr(l, "z")
```

```
## Error: Result 3 must be a single string, not NULL of length 0
```

Mas você pode resolver isso passando um valor padrão


```r
map_chr(l, "z", .default = NA_character_)
```

```
## [1] "a" "b" NA
```

Tudo isso funciona com base na função `pluck`


```r
pluck(l, 1)
```

```
## [[1]]
## [1] -1
## 
## $x
## [1] 1
## 
## $y
## [1] 2
## 
## $z
## [1] "a"
```

```r
pluck(l, 1, 2)
```

```
## [1] 1
```

```r
pluck(l, 2, 3)
```

```
## [1] 5 6
```

```r
pluck(l, 1, "x")
```

```
## [1] 1
```

```r
pluck(l, 2, "z")
```

```
## [1] "b"
```

```r
pluck(l, 3, "y")
```

```
## [1]  9 10 11
```

## Exercícios

1. Utilize uma das funções `map_` para:
    
  1. Calcular a média de cada coluna em `mtcars`.
  2. Determinar o tipo de cada coluna em `flights`.
  3. Computar o número de valores únicos em cada coluna de `iris`.
  4. Gere 10 distribuições aleatórias (`rnorm`) com médias -10, 0, 10 e 100.

2. Como você pode criar um vetor indicando se cada coluna em um `data.frame` é um fator?

3. Usando as funções predicado `keep` e `discard`:
  1. Selecione todas as colunas caractere no banco `flights`.
  2. Descarte os caracteres em `mpg`.
  3. Selecione os fatores ordenados em `diamonds`.
  4. Descarte as variáveis não-numéricas em `iris`

4. Imagine que você tem um diretório cheio de arquivos `.csv` que correspondem a um único banco de dados. Você tem os caminhos de todos eles num vetor com a forma `c(arquivo_1.csv, ..., arquivo_n.csv)`. Como você importaria esses arquivos? Tente fazer duas soluções diferentes.

5. Escreva um código sucinto que implemente vários modelos lineares especificados por você. Salve os resultados numa `tibble` com colunas-lista. Depois, extraia os resultados com `unnest()`. Use o exemplo como guia.


```r
mtcars
```

```
##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
## Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
## Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
## Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
## Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
## Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
## Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
## Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
## Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
## AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
## Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
## Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
## Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
## Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
## Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
## Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
## Ferrari Dino        19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
## Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
## Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
```

```r
modelos <- c("mpg ~ wt",
             "mpg ~ wt + cyl",
             "mpg ~ wt + cyl + drat",
             "mpg ~ wt + cyl + drat + am")
```

