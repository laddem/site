---
date: 2021-05-23
title: purrr
type: book
weight: 50
draft: false
---

O autor dos pacotes do `tidyverse` core é completamente fascinado por gatos, por isso, tantas referências ao bixo nos nomes dos pacotes. "Purr" (em inglês) é o som que os gatos fazem quando sentem prazer. Pra não ficar uma coisa solta no começo da aula, aqui uma foto de gatinho pra vocês:

![Amy, the cat](/courses/tidyverse/dia4_files/amy-cat.jpg)

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
##   [1] 511.21033 511.89922 773.63835  75.66633 722.47241 117.13315 951.66865
##   [8] 105.49553 770.73902 497.39858 324.68612 717.92247 527.07341 585.00887
##  [15] 380.39971  17.15150 957.58816 225.99117 653.19930 276.15361 531.60984
##  [22] 496.43149 804.00073 727.48489  86.62227 951.70058 382.33849 624.78218
##  [29]  83.78090 591.81424 473.27932 777.49061 467.26225 319.52625  14.61723
##  [36] 843.51570 657.64747 509.07023  60.33003 933.35876  91.72044 351.31833
##  [43] 595.22138 148.48688  31.02683 245.96797 491.90911 303.65395 993.94694
##  [50]  14.63012 532.77839 978.14352 985.71426 259.13229 545.54421 835.12723
##  [57] 900.39120 598.04300  85.86639 917.62820 902.74467 883.72252 511.84901
##  [64] 101.90038 346.86150 655.52290 447.19990 755.65847 546.60029 341.40437
##  [71] 550.06610 356.46386 645.91008 690.29805  11.03944 831.26283 821.42773
##  [78] 216.10993 687.41842 536.72430 833.02925 298.43149 470.50705 368.70771
##  [85] 609.66773 738.84025 675.22583 348.49183 453.19584 579.29202 190.77600
##  [92] 943.77276  22.31339 641.31284  28.05916 606.97261 344.78386 874.06792
##  [99] 974.14733 618.56387
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
## [1] 51405.75
```

```r
sum(x)
```

```
## [1] 51405.75
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
## [1]  5175.084 10214.559 19979.241
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
## [1] 51405.75
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
##  1 64.0   84.6  204.      352.
##  2 80.3   81.0  210.      371.
##  3 56.7  124.   210.      391.
##  4 88.0   99.6  222.      409.
##  5  8.80  91.8  220.      320.
##  6 33.6  105.   175.      314.
##  7 63.3   94.3  202.      359.
##  8 44.4   79.0  191.      314.
##  9 59.4  119.   192.      371.
## 10 27.8   45.1  202.      275.
## # ... with 90 more rows
```

Outra vantagem de ter na mão uma função, é que eu posso me apropriar das ferramentas de programação funcional do R. São as funções da família `apply`, que recebem uma lista de objetos e aplicam uma função em cada um. Basicamente, aquela **abstração** do for loop que estavamos buscando.

Vejam como eu posso recriar o exemplo das somas das colunas do `data.frame` usando a função soma.


```r
sapply(df, soma)
```

```
##         x         y         z soma_xyz2 
##  5175.084 10214.559 19979.241 35368.884
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
## [1] 5175.084
## 
## $y
## [1] 10214.56
## 
## $z
## [1] 19979.24
## 
## $soma_xyz2
## [1] 35368.88
```

```r
df %>% map(sum)
```

```
## $x
## [1] 5175.084
## 
## $y
## [1] 10214.56
## 
## $z
## [1] 19979.24
## 
## $soma_xyz2
## [1] 35368.88
```

Embora existam pequenas diferenças técnicas entre elas, a principal vantagem de `map` é a possibilidade de criar funções anônimas com uma síntaxe enxuta. Pense, por exemplo, em elevar ao quadrado.


```r
df %>% lapply(function(x) x ^ 2)
```

```
## $x
##   [1]  4101.73466  6442.94757  3219.96411  7740.50083    77.52392  1127.58641
##   [7]  4003.18277  1970.49014  3530.47096   774.99987  2443.20895  2865.99209
##  [13]  4835.03006  2291.73704  3472.69766  1827.06745  1954.20529  5005.39589
##  [19]   599.55337  6840.21539  2690.90027  4443.48342  8176.61716  1122.35347
##  [25]  6124.43204  4090.38758  1258.12990  2903.93407  1505.60524   816.50506
##  [31]  2762.76527 10206.93299  3383.39474  3929.24440   858.32911   299.32177
##  [37]  6349.05638   465.28473  1911.37533  7033.14246   813.33451  1467.96694
##  [43]  2018.47077  2411.16517  1945.70017  3561.26161  1401.68199  3657.67548
##  [49]  5975.33518  3707.27733    82.02548  8828.42227  6899.83497   938.19812
##  [55]  3413.88464   927.31285  1181.68012  2922.96590  2157.63334  4754.98150
##  [61]  3069.83817  2994.45852    20.95274  3286.19224  5448.84573  4614.42975
##  [67]  5147.05677  3342.69959  2750.92683   916.98522   138.26289  3952.18991
##  [73]  5720.72876  2815.19365  2091.69446  1278.02274    10.68606  1700.03805
##  [79]  3664.47310  5700.26729   155.81705   964.87730  1704.68622  1187.67307
##  [85]  2495.20060  5450.75572   934.23105  4995.71787  3996.69171  3570.77096
##  [91]  3647.71154  8265.66190  8104.97900  2787.03070   105.38002  2139.97839
##  [97]  2188.88388  4027.12053  1516.84960  4629.35075
## 
## $y
##   [1]  7151.412  6565.904 15465.469  9912.371  8422.026 11062.683  8898.726
##   [8]  6233.135 14195.810  2030.401  5565.017 15515.046 11528.588  9451.909
##  [15]  6173.870  4146.633  3164.735 18272.908 24261.140 17448.823 17156.170
##  [22]  4154.849 10407.268 10852.507 12008.867 11661.905  9962.577  5827.709
##  [29]  1917.046 11350.607 12628.009 14620.744  8851.069  7313.565 10635.098
##  [36]  8405.690 11204.093 10624.971  6625.751 17180.077  6758.962 14635.734
##  [43] 15028.864  6287.385  9670.247 27097.833 12444.103  5174.200  1987.106
##  [50]  3968.708 14741.992  7745.702  7529.071 11367.992 11004.793 13263.630
##  [57] 14356.074 15834.530 11392.086 13884.453  8110.941  3459.290 17805.823
##  [64]  9464.322 10653.107 13969.328 16219.520  4533.473  5779.148  7423.940
##  [71] 19330.647 20030.626 10051.328  7654.627  2168.127 17940.152 11978.961
##  [78]  7928.637 15345.071  5966.271 15963.726 14123.782  4730.376 22526.893
##  [85] 13404.375 15081.751 19130.641 27500.443  6542.030 10927.504  3684.683
##  [92]  9811.098 15832.925 17041.849 17629.378  6607.492  5663.422 11701.811
##  [99]  6023.724 21869.526
## 
## $z
##   [1] 41508.55 43955.22 44122.17 49181.30 48234.98 30624.13 40730.31 36356.66
##   [9] 36849.43 40839.24 47870.88 55940.49 56158.85 52453.95 36367.21 33702.16
##  [17] 40651.44 37526.54 33466.91 40098.83 21280.36 31668.90 59987.43 42429.94
##  [25] 47436.91 56383.51 42850.74 33140.52 30740.82 33382.25 39325.68 29803.55
##  [33] 27582.72 36055.88 30141.33 43135.68 49081.05 35531.51 40896.89 40136.05
##  [41] 58039.94 40374.40 20277.93 48318.24 41539.72 53062.55 61468.63 33130.61
##  [49] 41153.57 25438.34 34253.86 50562.05 24140.92 42847.66 38727.46 36720.32
##  [57] 51322.56 51123.33 30686.04 26044.65 35205.08 55941.68 39308.71 34987.88
##  [65] 42918.57 31031.96 52151.91 25617.74 21877.90 43197.98 50814.30 41798.91
##  [73] 58608.88 42227.48 54186.38 59475.44 41981.70 23404.82 40336.76 35826.02
##  [81] 46172.67 58244.33 31089.67 40382.25 57512.31 29234.02 46584.44 42845.56
##  [89] 31071.13 43848.77 44352.73 40918.43 32501.86 25508.47 48716.72 43616.08
##  [97] 44454.40 26486.83 27212.78 35157.45
## 
## $soma_xyz2
##   [1] 124148.61 137606.30 153004.39 167534.34 102528.67  98443.05 129183.67
##   [8]  98605.19 137289.51  75617.30 117526.93 171902.94 171300.16 139966.73
##  [15] 107718.82  84518.14  91254.97 159715.19 131903.56 172263.61 108065.60
##  [22]  95527.45 191287.26 118103.71 164546.97 167607.18 117160.41  97514.41
##  [29]  66521.08 101010.74 131945.81 155695.49 101332.42 114302.78  93658.30
##  [36] 100282.91 165708.24  98060.42 107156.57 172454.54 123655.43 129762.85
##  [43]  96050.56 121250.73 119896.30 206700.80 157546.17  98865.47 105456.51
##  [50]  80303.71  99572.67 165510.17 105760.85 118506.39 129690.11 113734.26
##  [57] 144960.84 164839.64 107819.22 121223.89 110953.64 122540.12 113084.32
##  [64] 116731.92 147608.24 131247.09 182727.87  81340.51  76386.89 105160.94
##  [71] 124394.78 167153.29 174711.06 119745.34 105675.64 171036.97 100877.23
##  [78]  80236.65 148417.75 116977.26 125109.75 153072.34  82018.20 148614.57
##  [85] 164467.96 135141.96 148004.12 196696.37 102638.38 139645.42 110024.06
##  [92] 153859.84 156926.37 117683.63 132321.18 113158.92 110811.31 111811.48
##  [99]  79254.85 162752.56
```

```r
df %>% map(~ .x ^ 2)
```

```
## $x
##   [1]  4101.73466  6442.94757  3219.96411  7740.50083    77.52392  1127.58641
##   [7]  4003.18277  1970.49014  3530.47096   774.99987  2443.20895  2865.99209
##  [13]  4835.03006  2291.73704  3472.69766  1827.06745  1954.20529  5005.39589
##  [19]   599.55337  6840.21539  2690.90027  4443.48342  8176.61716  1122.35347
##  [25]  6124.43204  4090.38758  1258.12990  2903.93407  1505.60524   816.50506
##  [31]  2762.76527 10206.93299  3383.39474  3929.24440   858.32911   299.32177
##  [37]  6349.05638   465.28473  1911.37533  7033.14246   813.33451  1467.96694
##  [43]  2018.47077  2411.16517  1945.70017  3561.26161  1401.68199  3657.67548
##  [49]  5975.33518  3707.27733    82.02548  8828.42227  6899.83497   938.19812
##  [55]  3413.88464   927.31285  1181.68012  2922.96590  2157.63334  4754.98150
##  [61]  3069.83817  2994.45852    20.95274  3286.19224  5448.84573  4614.42975
##  [67]  5147.05677  3342.69959  2750.92683   916.98522   138.26289  3952.18991
##  [73]  5720.72876  2815.19365  2091.69446  1278.02274    10.68606  1700.03805
##  [79]  3664.47310  5700.26729   155.81705   964.87730  1704.68622  1187.67307
##  [85]  2495.20060  5450.75572   934.23105  4995.71787  3996.69171  3570.77096
##  [91]  3647.71154  8265.66190  8104.97900  2787.03070   105.38002  2139.97839
##  [97]  2188.88388  4027.12053  1516.84960  4629.35075
## 
## $y
##   [1]  7151.412  6565.904 15465.469  9912.371  8422.026 11062.683  8898.726
##   [8]  6233.135 14195.810  2030.401  5565.017 15515.046 11528.588  9451.909
##  [15]  6173.870  4146.633  3164.735 18272.908 24261.140 17448.823 17156.170
##  [22]  4154.849 10407.268 10852.507 12008.867 11661.905  9962.577  5827.709
##  [29]  1917.046 11350.607 12628.009 14620.744  8851.069  7313.565 10635.098
##  [36]  8405.690 11204.093 10624.971  6625.751 17180.077  6758.962 14635.734
##  [43] 15028.864  6287.385  9670.247 27097.833 12444.103  5174.200  1987.106
##  [50]  3968.708 14741.992  7745.702  7529.071 11367.992 11004.793 13263.630
##  [57] 14356.074 15834.530 11392.086 13884.453  8110.941  3459.290 17805.823
##  [64]  9464.322 10653.107 13969.328 16219.520  4533.473  5779.148  7423.940
##  [71] 19330.647 20030.626 10051.328  7654.627  2168.127 17940.152 11978.961
##  [78]  7928.637 15345.071  5966.271 15963.726 14123.782  4730.376 22526.893
##  [85] 13404.375 15081.751 19130.641 27500.443  6542.030 10927.504  3684.683
##  [92]  9811.098 15832.925 17041.849 17629.378  6607.492  5663.422 11701.811
##  [99]  6023.724 21869.526
## 
## $z
##   [1] 41508.55 43955.22 44122.17 49181.30 48234.98 30624.13 40730.31 36356.66
##   [9] 36849.43 40839.24 47870.88 55940.49 56158.85 52453.95 36367.21 33702.16
##  [17] 40651.44 37526.54 33466.91 40098.83 21280.36 31668.90 59987.43 42429.94
##  [25] 47436.91 56383.51 42850.74 33140.52 30740.82 33382.25 39325.68 29803.55
##  [33] 27582.72 36055.88 30141.33 43135.68 49081.05 35531.51 40896.89 40136.05
##  [41] 58039.94 40374.40 20277.93 48318.24 41539.72 53062.55 61468.63 33130.61
##  [49] 41153.57 25438.34 34253.86 50562.05 24140.92 42847.66 38727.46 36720.32
##  [57] 51322.56 51123.33 30686.04 26044.65 35205.08 55941.68 39308.71 34987.88
##  [65] 42918.57 31031.96 52151.91 25617.74 21877.90 43197.98 50814.30 41798.91
##  [73] 58608.88 42227.48 54186.38 59475.44 41981.70 23404.82 40336.76 35826.02
##  [81] 46172.67 58244.33 31089.67 40382.25 57512.31 29234.02 46584.44 42845.56
##  [89] 31071.13 43848.77 44352.73 40918.43 32501.86 25508.47 48716.72 43616.08
##  [97] 44454.40 26486.83 27212.78 35157.45
## 
## $soma_xyz2
##   [1] 124148.61 137606.30 153004.39 167534.34 102528.67  98443.05 129183.67
##   [8]  98605.19 137289.51  75617.30 117526.93 171902.94 171300.16 139966.73
##  [15] 107718.82  84518.14  91254.97 159715.19 131903.56 172263.61 108065.60
##  [22]  95527.45 191287.26 118103.71 164546.97 167607.18 117160.41  97514.41
##  [29]  66521.08 101010.74 131945.81 155695.49 101332.42 114302.78  93658.30
##  [36] 100282.91 165708.24  98060.42 107156.57 172454.54 123655.43 129762.85
##  [43]  96050.56 121250.73 119896.30 206700.80 157546.17  98865.47 105456.51
##  [50]  80303.71  99572.67 165510.17 105760.85 118506.39 129690.11 113734.26
##  [57] 144960.84 164839.64 107819.22 121223.89 110953.64 122540.12 113084.32
##  [64] 116731.92 147608.24 131247.09 182727.87  81340.51  76386.89 105160.94
##  [71] 124394.78 167153.29 174711.06 119745.34 105675.64 171036.97 100877.23
##  [78]  80236.65 148417.75 116977.26 125109.75 153072.34  82018.20 148614.57
##  [85] 164467.96 135141.96 148004.12 196696.37 102638.38 139645.42 110024.06
##  [92] 153859.84 156926.37 117683.63 132321.18 113158.92 110811.31 111811.48
##  [99]  79254.85 162752.56
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
##          x      y       z
##      <dbl>  <dbl>   <dbl>
##  1 -0.423   0.647  0.340 
##  2  0.168  -0.654  0.134 
##  3  0.317  -1.52   0.479 
##  4 -0.0876  0.229 -0.579 
##  5  1.08    1.01  -1.05  
##  6  0.0523  0.726 -0.790 
##  7  0.967  -1.01  -0.814 
##  8  0.772   0.262  1.28  
##  9 -1.07   -0.549 -0.759 
## 10  1.19    0.768 -0.0372
## # ... with 90 more rows
## 
## [[2]]
## # A tibble: 100 x 3
##         x       y        z
##     <dbl>   <dbl>    <dbl>
##  1 -0.426  0.427  -0.428  
##  2 -0.532  0.0595 -1.46   
##  3  0.211 -0.125   1.14   
##  4  0.853 -0.376  -0.0773 
##  5  1.45   1.32   -0.248  
##  6 -1.11   0.785  -0.0661 
##  7 -1.03  -0.237  -0.00866
##  8  0.950  0.449   1.25   
##  9 -0.467 -0.0485  0.168  
## 10  0.886 -0.338  -0.376  
## # ... with 90 more rows
## 
## [[3]]
## # A tibble: 100 x 3
##         x       y        z
##     <dbl>   <dbl>    <dbl>
##  1  1.69  -0.524  -0.0254 
##  2 -2.34   2.02    0.459  
##  3 -1.01  -0.0508  0.678  
##  4  0.117 -0.427  -0.240  
##  5 -2.26  -0.598  -0.863  
##  6  0.801  0.397  -0.923  
##  7 -0.484  0.501   0.00797
##  8  1.07   1.49    0.587  
##  9  0.102 -1.25    0.393  
## 10 -1.09  -0.789   1.65   
## # ... with 90 more rows
## 
## [[4]]
## # A tibble: 100 x 3
##         x      y       z
##     <dbl>  <dbl>   <dbl>
##  1  0.563 -0.756  1.34  
##  2  0.150  0.664  0.373 
##  3 -0.762  1.28  -1.12  
##  4  0.888  0.920 -0.0385
##  5 -0.786  1.66  -0.914 
##  6  1.27  -0.454  0.721 
##  7  1.39  -0.567  0.289 
##  8 -1.49   0.489  0.545 
##  9 -0.173  0.780 -1.17  
## 10  0.335 -0.947 -0.959 
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
##          x      y       z
##      <dbl>  <dbl>   <dbl>
##  1 -0.423   0.647  0.340 
##  2  0.168  -0.654  0.134 
##  3  0.317  -1.52   0.479 
##  4 -0.0876  0.229 -0.579 
##  5  1.08    1.01  -1.05  
##  6  0.0523  0.726 -0.790 
##  7  0.967  -1.01  -0.814 
##  8  0.772   0.262  1.28  
##  9 -1.07   -0.549 -0.759 
## 10  1.19    0.768 -0.0372
## # ... with 90 more rows
## 
## [[2]]
## # A tibble: 100 x 3
##         x       y        z
##     <dbl>   <dbl>    <dbl>
##  1 -0.426  0.427  -0.428  
##  2 -0.532  0.0595 -1.46   
##  3  0.211 -0.125   1.14   
##  4  0.853 -0.376  -0.0773 
##  5  1.45   1.32   -0.248  
##  6 -1.11   0.785  -0.0661 
##  7 -1.03  -0.237  -0.00866
##  8  0.950  0.449   1.25   
##  9 -0.467 -0.0485  0.168  
## 10  0.886 -0.338  -0.376  
## # ... with 90 more rows
## 
## [[3]]
## # A tibble: 100 x 3
##         x       y        z
##     <dbl>   <dbl>    <dbl>
##  1  1.69  -0.524  -0.0254 
##  2 -2.34   2.02    0.459  
##  3 -1.01  -0.0508  0.678  
##  4  0.117 -0.427  -0.240  
##  5 -2.26  -0.598  -0.863  
##  6  0.801  0.397  -0.923  
##  7 -0.484  0.501   0.00797
##  8  1.07   1.49    0.587  
##  9  0.102 -1.25    0.393  
## 10 -1.09  -0.789   1.65   
## # ... with 90 more rows
## 
## [[4]]
## # A tibble: 100 x 3
##         x      y       z
##     <dbl>  <dbl>   <dbl>
##  1  0.563 -0.756  1.34  
##  2  0.150  0.664  0.373 
##  3 -0.762  1.28  -1.12  
##  4  0.888  0.920 -0.0385
##  5 -0.786  1.66  -0.914 
##  6  1.27  -0.454  0.721 
##  7  1.39  -0.567  0.289 
##  8 -1.49   0.489  0.545 
##  9 -0.173  0.780 -1.17  
## 10  0.335 -0.947 -0.959 
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
##    arquivo       x      y       z
##    <chr>     <dbl>  <dbl>   <dbl>
##  1 1       -0.423   0.647  0.340 
##  2 1        0.168  -0.654  0.134 
##  3 1        0.317  -1.52   0.479 
##  4 1       -0.0876  0.229 -0.579 
##  5 1        1.08    1.01  -1.05  
##  6 1        0.0523  0.726 -0.790 
##  7 1        0.967  -1.01  -0.814 
##  8 1        0.772   0.262  1.28  
##  9 1       -1.07   -0.549 -0.759 
## 10 1        1.19    0.768 -0.0372
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
##  [1] 40.28363 47.05338 55.52213 67.17898 63.43975 31.32029 26.02457 44.61720
##  [9] 65.55023 49.84583 51.14611 55.58084 49.75603 46.89402 59.27724 55.44557
## [17] 54.67422 60.47497 44.36346 40.90925 53.91280 53.97654 74.11447 60.40166
## [25] 52.34901
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
##  $ : num 52.3
##  $ : num 10.8
##  $ : num 12.4
##  $ : num 1304
##  $ : int 25
##  $ : num [1:2] 26 74.1
##  $ : num 26
##  $ : num 74.1
```

Fica mais bonito se você nomear os argumentos, ai o output fica melhorzinho.


```r
names(funcs) <- unlist(funcs)

invoke_map(funcs, args, x = numeros) %>% glimpse()
```

```
## List of 8
##  $ mean  : num 52.3
##  $ sd    : num 10.8
##  $ IQR   : num 12.4
##  $ sum   : num 1304
##  $ length: int 25
##  $ range : num [1:2] 26 74.1
##  $ min   : num 26
##  $ max   : num 74.1
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
##   ..$ x: num [1:100] -0.4228 0.1677 0.3166 -0.0876 1.0804 ...
##   ..$ y: num [1:100] 0.647 -0.654 -1.521 0.229 1.014 ...
##   ..$ z: num [1:100] 0.34 0.134 0.479 -0.579 -1.048 ...
##   ..- attr(*, "spec")=
##   .. .. cols(
##   .. ..   x = col_double(),
##   .. ..   y = col_double(),
##   .. ..   z = col_double()
##   .. .. )
##  $ : spec_tbl_df [100 x 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##   ..$ x: num [1:100] -0.426 -0.532 0.211 0.853 1.452 ...
##   ..$ y: num [1:100] 0.4268 0.0595 -0.1254 -0.3762 1.3227 ...
##   ..$ z: num [1:100] -0.4284 -1.4608 1.1365 -0.0773 -0.2477 ...
##   ..- attr(*, "spec")=
##   .. .. cols(
##   .. ..   x = col_double(),
##   .. ..   y = col_double(),
##   .. ..   z = col_double()
##   .. .. )
##  $ : spec_tbl_df [100 x 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##   ..$ x: num [1:100] 1.689 -2.343 -1.015 0.117 -2.26 ...
##   ..$ y: num [1:100] -0.524 2.0172 -0.0508 -0.4266 -0.598 ...
##   ..$ z: num [1:100] -0.0254 0.4592 0.6776 -0.2399 -0.8627 ...
##   ..- attr(*, "spec")=
##   .. .. cols(
##   .. ..   x = col_double(),
##   .. ..   y = col_double(),
##   .. ..   z = col_double()
##   .. .. )
##  $ : spec_tbl_df [100 x 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##   ..$ x: num [1:100] 0.563 0.15 -0.762 0.888 -0.786 ...
##   ..$ y: num [1:100] -0.756 0.664 1.281 0.92 1.659 ...
##   ..$ z: num [1:100] 1.3386 0.3727 -1.1177 -0.0385 -0.9144 ...
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
##          x      y       z
##      <dbl>  <dbl>   <dbl>
##  1 -0.423   0.647  0.340 
##  2  0.168  -0.654  0.134 
##  3  0.317  -1.52   0.479 
##  4 -0.0876  0.229 -0.579 
##  5  1.08    1.01  -1.05  
##  6  0.0523  0.726 -0.790 
##  7  0.967  -1.01  -0.814 
##  8  0.772   0.262  1.28  
##  9 -1.07   -0.549 -0.759 
## 10  1.19    0.768 -0.0372
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

