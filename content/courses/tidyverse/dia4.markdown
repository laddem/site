---
date: 2021-05-23
title: purrr
type: book
weight: 50
draft: false
---

O autor dos pacotes do `tidyverse` core é completamente fascinado por gatos, por isso, tantas referências ao bicho nos nomes dos pacotes. "Purr" (em inglês) é o som que os gatos fazem quando sentem prazer. Para não ficar uma coisa solta no começo da aula, aqui uma foto de gatinho pra vocês:

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
##   [1] 932.45060 854.73036 322.68529 473.32061 787.93718 737.81215 985.23288
##   [8] 556.12284 427.43083 202.31560 556.68959 471.12877 325.80404 864.23383
##  [15] 821.50368 217.90992 828.55693 208.16253 101.32865 497.02844 162.14640
##  [22] 705.92322 541.78227 878.39618 999.20586 779.41229 235.10385 716.48081
##  [29] 316.74047 600.86867 223.53952 776.60319 462.10780 883.16957 709.43722
##  [36] 389.57969 964.70028 376.61982 799.59570 144.71691 924.66184 588.25282
##  [43] 116.61194 608.07781 134.93851 350.80481 851.81603 712.44217  72.07885
##  [50] 921.23962  29.30420 803.04197 704.33304 529.43881 620.59352  68.71324
##  [57] 621.06605 369.86052 978.34026  71.94507 954.05547 933.73772 747.56437
##  [64] 467.99461 708.76668  68.69263 904.94534 444.09781 274.69749 977.41118
##  [71] 251.36856 702.56858  40.30280 940.35432 117.62457 442.16959  15.79249
##  [78] 102.77577  99.87290 712.89847 962.80337  46.97794 930.34112 326.48483
##  [85] 439.90790 965.39917 322.10377 970.62833 663.42152 263.12762 759.84708
##  [92]   5.94325 431.82749 460.66645 458.93459  74.09223 315.73897 686.44889
##  [99] 861.14429 498.09425
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
## [1] 53267.7
```

```r
sum(x)
```

```
## [1] 53267.7
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
## [1]  4763.273 10257.077 20251.471
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
## [1] 53267.7
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
##  1  20.2 139.   193.      352.
##  2  56.3  91.9  199.      347.
##  3  28.2 125.   221.      375.
##  4  77.4  68.6  213.      359.
##  5  76.9  48.6  169.      294.
##  6  26.1  95.7  236.      358.
##  7  55.3  68.5  198.      322.
##  8  81.9  98.0  265.      445.
##  9  58.5  71.3  111.      240.
## 10  31.5 108.   231.      371.
## # ... with 90 more rows
```

Outra vantagem de ter na mão uma função, é que eu posso me apropriar das ferramentas de programação funcional do R. São as funções da família `apply`, que recebem uma lista de objetos e aplicam uma função em cada um. Basicamente, aquela **abstração** do for loop que estavamos buscando.

Vejam como eu posso recriar o exemplo das somas das colunas do `data.frame` usando a função soma.


```r
sapply(df, soma)
```

```
##         x         y         z soma_xyz2 
##  4763.273 10257.077 20251.471 35271.821
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
## [1] 4763.273
## 
## $y
## [1] 10257.08
## 
## $z
## [1] 20251.47
## 
## $soma_xyz2
## [1] 35271.82
```

```r
df %>% map(sum)
```

```
## $x
## [1] 4763.273
## 
## $y
## [1] 10257.08
## 
## $z
## [1] 20251.47
## 
## $soma_xyz2
## [1] 35271.82
```

Embora existam pequenas diferenças técnicas entre elas, a principal vantagem de `map` é a possibilidade de criar funções anônimas com uma síntaxe enxuta. Pense, por exemplo, em elevar ao quadrado.


```r
df %>% lapply(function(x) x ^ 2)
```

```
## $x
##   [1] 4.098851e+02 3.166900e+03 7.965187e+02 5.990114e+03 5.919986e+03
##   [6] 6.806833e+02 3.058849e+03 6.710794e+03 3.418073e+03 9.911043e+02
##  [11] 7.262557e+03 1.229650e+03 4.614959e+03 1.334587e+03 4.376548e+03
##  [16] 1.683557e+00 3.678986e+03 7.303855e+02 2.281344e+03 6.020090e+03
##  [21] 1.616306e+03 1.022110e+03 2.669522e+03 6.254209e+02 2.947705e+03
##  [26] 1.228063e+03 2.983676e+03 5.213567e+03 9.712738e+02 1.091035e+03
##  [31] 9.744005e+03 3.788507e+03 1.518321e+03 3.973071e+00 4.709316e+03
##  [36] 8.881056e+03 4.026554e+03 6.044031e+03 4.619299e+03 3.993292e+03
##  [41] 1.099183e+03 3.245529e+03 2.409330e+03 6.679147e+03 3.316934e+03
##  [46] 6.501946e+03 1.978189e+03 4.707893e+03 9.689042e+02 1.957993e+03
##  [51] 2.873459e+03 2.794197e+03 5.046830e+03 1.074281e+03 2.117916e+03
##  [56] 3.384339e+03 2.103909e+03 4.102475e+02 9.532060e+02 6.740582e+03
##  [61] 7.270433e+02 3.579561e+03 7.520174e+03 2.640835e+03 1.262544e+03
##  [66] 2.075985e+03 1.424682e+03 2.603697e+02 8.431157e+02 1.063360e+03
##  [71] 3.582397e+02 3.114648e+02 9.796727e+02 2.202274e+03 5.074693e+03
##  [76] 4.770465e+03 3.896971e+03 3.910142e+03 5.819862e+01 3.020810e+03
##  [81] 7.376594e+03 1.009338e+03 1.488763e+03 1.447512e-03 2.358162e+03
##  [86] 1.749782e+03 6.096146e+02 2.326065e-01 7.560180e+02 4.230610e+03
##  [91] 3.870858e+03 7.034365e+03 2.282774e+00 2.048389e+03 1.783539e+03
##  [96] 3.326398e+02 3.721681e+03 3.599671e+03 9.388893e+02 1.781901e+02
## 
## $y
##   [1] 19262.498  8440.949 15706.307  4699.395  2365.636  9158.381  4692.247
##   [8]  9608.843  5090.449 11770.936 18153.544 17477.696  5395.348 10194.023
##  [15] 16405.544  7442.935  6077.889 11838.011  1518.600 14098.638  8195.648
##  [22] 20491.093 16726.949  1902.552 16036.617  3346.542  7980.461 17444.534
##  [29] 10690.553 15542.924 19621.167  8133.165 17494.853 16943.489  9692.481
##  [36] 18518.499  8078.944  8584.160  8936.495  2016.352 16465.394  7015.704
##  [43] 15703.783 28859.615 14385.671 12426.197 22442.690  4366.540  7877.715
##  [50] 15996.190 13474.364  6688.742  7970.944  8001.087  9704.368  8127.663
##  [57] 12784.580 15311.353 13045.664  5888.471  4923.967 13759.427 10456.918
##  [64] 17088.958  7803.571  4756.058  7695.688 15125.565 18005.310 13821.906
##  [71] 12273.549  3906.617 15237.079  9925.422 14122.162  8628.404 15950.013
##  [78] 20258.130 13275.272 10398.823 14999.890  5641.321  5964.132 17029.926
##  [85] 12111.979 10285.968  9533.723 18723.109 12764.953 18973.842 13175.134
##  [92]  2003.455 11236.108  3245.825  9936.697  7681.693  3721.840  6701.296
##  [99] 13070.664  7377.707
## 
## $z
##   [1] 37183.52 39628.72 49034.03 45434.22 28479.23 55896.08 39203.50 70104.90
##   [9] 12246.82 53206.94 37583.26 38015.74 48362.58 52578.69 30413.38 48118.08
##  [17] 35491.71 49080.87 28324.10 25013.11 62625.54 36067.94 33375.13 30367.84
##  [25] 43104.49 47884.11 51146.67 33091.00 45992.31 37133.20 33173.89 39156.23
##  [33] 51787.03 43173.31 38775.45 31521.51 50210.34 47023.23 27656.30 36609.76
##  [41] 35897.11 41197.37 28894.09 52887.36 32805.70 37407.91 52157.29 41996.41
##  [49] 34322.69 33124.52 31077.55 21019.93 46411.13 32895.26 55085.26 47956.83
##  [57] 73376.10 31954.32 29485.71 49999.43 52964.14 37318.59 38305.11 44738.32
##  [65] 38052.40 51223.73 43747.69 55565.40 57276.59 63332.76 42843.69 56657.40
##  [73] 38516.17 43591.16 59826.20 46073.10 47388.84 43874.46 43295.12 37138.21
##  [81] 55577.27 33570.50 44585.57 39053.49 27161.43 29699.13 39081.64 30360.08
##  [89] 33522.29 49180.78 49246.60 39041.14 38814.45 35312.49 38863.55 31376.48
##  [97] 32465.36 38521.36 52356.80 34374.93
## 
## $soma_xyz2
##   [1] 123809.19 120561.38 140612.83 128953.53  86634.35 128316.46 103558.83
##   [8] 197773.60  57829.20 137375.52 171246.79 131222.06 130538.74 134540.69
##  [15] 135890.98  94204.98 106934.16 127713.53  65040.45 125657.67 145148.87
##  [22] 133249.22 132269.50  58995.74 150967.02  97167.74 136983.45 149144.35
##  [29] 121813.96 122781.46 177177.27 122230.11 159042.55 112866.29 132488.50
##  [36] 166353.87 142441.87 149956.75 108109.63  89660.17 123156.64 118130.16
##  [43] 118599.15 231919.06 128634.52 148624.71 178646.03 115344.55  93115.03
##  [50] 124416.11 119696.66  78191.49 141190.79  92170.25 143818.48 134923.31
##  [57] 184743.12 104168.42 100365.85 146262.60 107108.34 137129.66 147990.19
##  [64] 154943.12 101722.96 116181.28 111976.92 140508.90 162042.95 161472.01
##  [71] 113367.20 101238.16 123196.54 126266.34 188935.87 141830.72 165168.16
##  [78] 171664.79 109509.35 122254.62 197233.20  84158.80 106906.26 107636.66
##  [85] 104602.18  99593.59 102414.04  96467.13 104696.77 180247.90 159133.14
##  [92] 106419.09  90904.34  84185.55 114957.18  80099.05  91321.16 114330.07
##  [99] 139714.77  81024.00
```

```r
df %>% map(~ .x ^ 2)
```

```
## $x
##   [1] 4.098851e+02 3.166900e+03 7.965187e+02 5.990114e+03 5.919986e+03
##   [6] 6.806833e+02 3.058849e+03 6.710794e+03 3.418073e+03 9.911043e+02
##  [11] 7.262557e+03 1.229650e+03 4.614959e+03 1.334587e+03 4.376548e+03
##  [16] 1.683557e+00 3.678986e+03 7.303855e+02 2.281344e+03 6.020090e+03
##  [21] 1.616306e+03 1.022110e+03 2.669522e+03 6.254209e+02 2.947705e+03
##  [26] 1.228063e+03 2.983676e+03 5.213567e+03 9.712738e+02 1.091035e+03
##  [31] 9.744005e+03 3.788507e+03 1.518321e+03 3.973071e+00 4.709316e+03
##  [36] 8.881056e+03 4.026554e+03 6.044031e+03 4.619299e+03 3.993292e+03
##  [41] 1.099183e+03 3.245529e+03 2.409330e+03 6.679147e+03 3.316934e+03
##  [46] 6.501946e+03 1.978189e+03 4.707893e+03 9.689042e+02 1.957993e+03
##  [51] 2.873459e+03 2.794197e+03 5.046830e+03 1.074281e+03 2.117916e+03
##  [56] 3.384339e+03 2.103909e+03 4.102475e+02 9.532060e+02 6.740582e+03
##  [61] 7.270433e+02 3.579561e+03 7.520174e+03 2.640835e+03 1.262544e+03
##  [66] 2.075985e+03 1.424682e+03 2.603697e+02 8.431157e+02 1.063360e+03
##  [71] 3.582397e+02 3.114648e+02 9.796727e+02 2.202274e+03 5.074693e+03
##  [76] 4.770465e+03 3.896971e+03 3.910142e+03 5.819862e+01 3.020810e+03
##  [81] 7.376594e+03 1.009338e+03 1.488763e+03 1.447512e-03 2.358162e+03
##  [86] 1.749782e+03 6.096146e+02 2.326065e-01 7.560180e+02 4.230610e+03
##  [91] 3.870858e+03 7.034365e+03 2.282774e+00 2.048389e+03 1.783539e+03
##  [96] 3.326398e+02 3.721681e+03 3.599671e+03 9.388893e+02 1.781901e+02
## 
## $y
##   [1] 19262.498  8440.949 15706.307  4699.395  2365.636  9158.381  4692.247
##   [8]  9608.843  5090.449 11770.936 18153.544 17477.696  5395.348 10194.023
##  [15] 16405.544  7442.935  6077.889 11838.011  1518.600 14098.638  8195.648
##  [22] 20491.093 16726.949  1902.552 16036.617  3346.542  7980.461 17444.534
##  [29] 10690.553 15542.924 19621.167  8133.165 17494.853 16943.489  9692.481
##  [36] 18518.499  8078.944  8584.160  8936.495  2016.352 16465.394  7015.704
##  [43] 15703.783 28859.615 14385.671 12426.197 22442.690  4366.540  7877.715
##  [50] 15996.190 13474.364  6688.742  7970.944  8001.087  9704.368  8127.663
##  [57] 12784.580 15311.353 13045.664  5888.471  4923.967 13759.427 10456.918
##  [64] 17088.958  7803.571  4756.058  7695.688 15125.565 18005.310 13821.906
##  [71] 12273.549  3906.617 15237.079  9925.422 14122.162  8628.404 15950.013
##  [78] 20258.130 13275.272 10398.823 14999.890  5641.321  5964.132 17029.926
##  [85] 12111.979 10285.968  9533.723 18723.109 12764.953 18973.842 13175.134
##  [92]  2003.455 11236.108  3245.825  9936.697  7681.693  3721.840  6701.296
##  [99] 13070.664  7377.707
## 
## $z
##   [1] 37183.52 39628.72 49034.03 45434.22 28479.23 55896.08 39203.50 70104.90
##   [9] 12246.82 53206.94 37583.26 38015.74 48362.58 52578.69 30413.38 48118.08
##  [17] 35491.71 49080.87 28324.10 25013.11 62625.54 36067.94 33375.13 30367.84
##  [25] 43104.49 47884.11 51146.67 33091.00 45992.31 37133.20 33173.89 39156.23
##  [33] 51787.03 43173.31 38775.45 31521.51 50210.34 47023.23 27656.30 36609.76
##  [41] 35897.11 41197.37 28894.09 52887.36 32805.70 37407.91 52157.29 41996.41
##  [49] 34322.69 33124.52 31077.55 21019.93 46411.13 32895.26 55085.26 47956.83
##  [57] 73376.10 31954.32 29485.71 49999.43 52964.14 37318.59 38305.11 44738.32
##  [65] 38052.40 51223.73 43747.69 55565.40 57276.59 63332.76 42843.69 56657.40
##  [73] 38516.17 43591.16 59826.20 46073.10 47388.84 43874.46 43295.12 37138.21
##  [81] 55577.27 33570.50 44585.57 39053.49 27161.43 29699.13 39081.64 30360.08
##  [89] 33522.29 49180.78 49246.60 39041.14 38814.45 35312.49 38863.55 31376.48
##  [97] 32465.36 38521.36 52356.80 34374.93
## 
## $soma_xyz2
##   [1] 123809.19 120561.38 140612.83 128953.53  86634.35 128316.46 103558.83
##   [8] 197773.60  57829.20 137375.52 171246.79 131222.06 130538.74 134540.69
##  [15] 135890.98  94204.98 106934.16 127713.53  65040.45 125657.67 145148.87
##  [22] 133249.22 132269.50  58995.74 150967.02  97167.74 136983.45 149144.35
##  [29] 121813.96 122781.46 177177.27 122230.11 159042.55 112866.29 132488.50
##  [36] 166353.87 142441.87 149956.75 108109.63  89660.17 123156.64 118130.16
##  [43] 118599.15 231919.06 128634.52 148624.71 178646.03 115344.55  93115.03
##  [50] 124416.11 119696.66  78191.49 141190.79  92170.25 143818.48 134923.31
##  [57] 184743.12 104168.42 100365.85 146262.60 107108.34 137129.66 147990.19
##  [64] 154943.12 101722.96 116181.28 111976.92 140508.90 162042.95 161472.01
##  [71] 113367.20 101238.16 123196.54 126266.34 188935.87 141830.72 165168.16
##  [78] 171664.79 109509.35 122254.62 197233.20  84158.80 106906.26 107636.66
##  [85] 104602.18  99593.59 102414.04  96467.13 104696.77 180247.90 159133.14
##  [92] 106419.09  90904.34  84185.55 114957.18  80099.05  91321.16 114330.07
##  [99] 139714.77  81024.00
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
##         x       y       z
##     <dbl>   <dbl>   <dbl>
##  1 -1.59   0.296   0.738 
##  2 -0.430 -0.0696  0.554 
##  3 -1.66  -0.0623 -1.25  
##  4 -0.595  0.353  -0.704 
##  5  0.669 -0.510   0.399 
##  6  0.829  1.08   -0.0918
##  7 -0.372  0.0488 -0.225 
##  8 -0.777  1.47    0.698 
##  9  0.510 -1.02    0.970 
## 10 -0.511  1.29    0.374 
## # ... with 90 more rows
## 
## [[2]]
## # A tibble: 100 x 3
##          x      y       z
##      <dbl>  <dbl>   <dbl>
##  1  0.0224  0.746 -1.29  
##  2  1.51   -0.462  0.743 
##  3 -0.0367  0.502  1.01  
##  4 -0.575   0.653  1.47  
##  5 -0.879  -0.591 -1.54  
##  6  0.168  -0.173 -0.0793
##  7 -0.747   1.75   0.962 
##  8 -0.748   2.08  -0.282 
##  9 -0.526  -1.08   0.486 
## 10  1.56    1.72   0.706 
## # ... with 90 more rows
## 
## [[3]]
## # A tibble: 100 x 3
##         x      y       z
##     <dbl>  <dbl>   <dbl>
##  1 -0.694 -0.633  0.469 
##  2  0.669  0.956  1.88  
##  3 -0.874  0.997 -0.415 
##  4 -1.99   0.517 -0.0228
##  5  0.308 -2.41   0.601 
##  6 -0.273 -1.70  -0.891 
##  7 -0.769 -0.531 -1.12  
##  8 -1.67  -0.520  0.0485
##  9 -0.837  0.254  0.675 
## 10 -0.627 -0.734 -0.830 
## # ... with 90 more rows
## 
## [[4]]
## # A tibble: 100 x 3
##          x       y      z
##      <dbl>   <dbl>  <dbl>
##  1  0.376  -0.0913 -0.312
##  2 -0.109   1.05   -0.229
##  3  0.181  -0.826  -0.658
##  4 -0.534  -0.498  -1.53 
##  5 -0.0549  2.03    0.809
##  6  0.677   1.04    0.750
##  7  0.0816 -1.15    1.80 
##  8 -0.710   1.26   -0.769
##  9  0.165  -0.375  -0.788
## 10  0.456   0.759   0.163
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
##         x       y       z
##     <dbl>   <dbl>   <dbl>
##  1 -1.59   0.296   0.738 
##  2 -0.430 -0.0696  0.554 
##  3 -1.66  -0.0623 -1.25  
##  4 -0.595  0.353  -0.704 
##  5  0.669 -0.510   0.399 
##  6  0.829  1.08   -0.0918
##  7 -0.372  0.0488 -0.225 
##  8 -0.777  1.47    0.698 
##  9  0.510 -1.02    0.970 
## 10 -0.511  1.29    0.374 
## # ... with 90 more rows
## 
## [[2]]
## # A tibble: 100 x 3
##          x      y       z
##      <dbl>  <dbl>   <dbl>
##  1  0.0224  0.746 -1.29  
##  2  1.51   -0.462  0.743 
##  3 -0.0367  0.502  1.01  
##  4 -0.575   0.653  1.47  
##  5 -0.879  -0.591 -1.54  
##  6  0.168  -0.173 -0.0793
##  7 -0.747   1.75   0.962 
##  8 -0.748   2.08  -0.282 
##  9 -0.526  -1.08   0.486 
## 10  1.56    1.72   0.706 
## # ... with 90 more rows
## 
## [[3]]
## # A tibble: 100 x 3
##         x      y       z
##     <dbl>  <dbl>   <dbl>
##  1 -0.694 -0.633  0.469 
##  2  0.669  0.956  1.88  
##  3 -0.874  0.997 -0.415 
##  4 -1.99   0.517 -0.0228
##  5  0.308 -2.41   0.601 
##  6 -0.273 -1.70  -0.891 
##  7 -0.769 -0.531 -1.12  
##  8 -1.67  -0.520  0.0485
##  9 -0.837  0.254  0.675 
## 10 -0.627 -0.734 -0.830 
## # ... with 90 more rows
## 
## [[4]]
## # A tibble: 100 x 3
##          x       y      z
##      <dbl>   <dbl>  <dbl>
##  1  0.376  -0.0913 -0.312
##  2 -0.109   1.05   -0.229
##  3  0.181  -0.826  -0.658
##  4 -0.534  -0.498  -1.53 
##  5 -0.0549  2.03    0.809
##  6  0.677   1.04    0.750
##  7  0.0816 -1.15    1.80 
##  8 -0.710   1.26   -0.769
##  9  0.165  -0.375  -0.788
## 10  0.456   0.759   0.163
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
##    arquivo      x       y       z
##    <chr>    <dbl>   <dbl>   <dbl>
##  1 1       -1.59   0.296   0.738 
##  2 1       -0.430 -0.0696  0.554 
##  3 1       -1.66  -0.0623 -1.25  
##  4 1       -0.595  0.353  -0.704 
##  5 1        0.669 -0.510   0.399 
##  6 1        0.829  1.08   -0.0918
##  7 1       -0.372  0.0488 -0.225 
##  8 1       -0.777  1.47    0.698 
##  9 1        0.510 -1.02    0.970 
## 10 1       -0.511  1.29    0.374 
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
##  [1] 44.79172 47.60495 48.61908 46.47576 33.53751 53.53056 59.71062 45.06651
##  [9] 87.43203 42.82188 42.27556 51.03986 46.29749 48.02671 68.12173 63.99874
## [17] 54.37657 47.13603 50.91887 50.39004 70.29964 33.01856 55.71406 61.57012
## [25] 53.94121
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
##  $ : num 51.6
##  $ : num 11.7
##  $ : num 9.42
##  $ : num 1307
##  $ : int 25
##  $ : num [1:2] 33 87.4
##  $ : num 33
##  $ : num 87.4
```

Fica mais bonito se você nomear os argumentos, ai o output fica melhorzinho.


```r
names(funcs) <- unlist(funcs)

invoke_map(funcs, args, x = numeros) %>% glimpse()
```

```
## List of 8
##  $ mean  : num 51.6
##  $ sd    : num 11.7
##  $ IQR   : num 9.42
##  $ sum   : num 1307
##  $ length: int 25
##  $ range : num [1:2] 33 87.4
##  $ min   : num 33
##  $ max   : num 87.4
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
##   ..$ x: num [1:100] -1.594 -0.43 -1.66 -0.595 0.669 ...
##   ..$ y: num [1:100] 0.2958 -0.0696 -0.0623 0.3526 -0.51 ...
##   ..$ z: num [1:100] 0.738 0.554 -1.252 -0.704 0.399 ...
##   ..- attr(*, "spec")=
##   .. .. cols(
##   .. ..   x = col_double(),
##   .. ..   y = col_double(),
##   .. ..   z = col_double()
##   .. .. )
##  $ : spec_tbl_df [100 x 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##   ..$ x: num [1:100] 0.0224 1.5069 -0.0367 -0.575 -0.8793 ...
##   ..$ y: num [1:100] 0.746 -0.462 0.502 0.653 -0.591 ...
##   ..$ z: num [1:100] -1.287 0.743 1.006 1.47 -1.536 ...
##   ..- attr(*, "spec")=
##   .. .. cols(
##   .. ..   x = col_double(),
##   .. ..   y = col_double(),
##   .. ..   z = col_double()
##   .. .. )
##  $ : spec_tbl_df [100 x 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##   ..$ x: num [1:100] -0.694 0.669 -0.874 -1.991 0.308 ...
##   ..$ y: num [1:100] -0.633 0.956 0.997 0.517 -2.412 ...
##   ..$ z: num [1:100] 0.4692 1.879 -0.4147 -0.0228 0.6012 ...
##   ..- attr(*, "spec")=
##   .. .. cols(
##   .. ..   x = col_double(),
##   .. ..   y = col_double(),
##   .. ..   z = col_double()
##   .. .. )
##  $ : spec_tbl_df [100 x 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##   ..$ x: num [1:100] 0.3761 -0.1088 0.1815 -0.5345 -0.0549 ...
##   ..$ y: num [1:100] -0.0913 1.0474 -0.8255 -0.4983 2.0343 ...
##   ..$ z: num [1:100] -0.312 -0.229 -0.658 -1.526 0.809 ...
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
##         x       y       z
##     <dbl>   <dbl>   <dbl>
##  1 -1.59   0.296   0.738 
##  2 -0.430 -0.0696  0.554 
##  3 -1.66  -0.0623 -1.25  
##  4 -0.595  0.353  -0.704 
##  5  0.669 -0.510   0.399 
##  6  0.829  1.08   -0.0918
##  7 -0.372  0.0488 -0.225 
##  8 -0.777  1.47    0.698 
##  9  0.510 -1.02    0.970 
## 10 -0.511  1.29    0.374 
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

