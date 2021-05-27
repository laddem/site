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
##   [1] 481.8565152 889.4467938 839.3919568 995.6545529 434.8893075 548.0684573
##   [7] 483.9695676 690.5513781 151.6914470 872.1020871   0.3946235 702.4974534
##  [13] 394.3226694 138.9176038 252.6494386 958.9286412 780.5854031 670.7747667
##  [19] 946.2900842 114.1950220 830.8356900 898.1239600 299.5932216 374.4803111
##  [25] 296.9716154 607.0112186 435.7656906 611.9967769  94.4200489 398.8551637
##  [31] 689.6964593 702.0807178 496.2329580 544.2922530 229.5011075 204.9134872
##  [37] 119.8225897 515.7763774 676.0634030 853.2036459 390.4024123 245.4408365
##  [43] 136.4117221  53.2958910 123.1613006 239.5618213 441.0227661 266.3004498
##  [49] 501.5086005 151.4288806 230.7690587 913.5274501 217.5684345 186.2564688
##  [55] 575.9362916 544.4018145 135.8508093  38.8186283 928.3766786 303.2186597
##  [61] 641.7115638 104.6880512 648.4473264 983.6191037 175.0366774  38.5587176
##  [67] 626.3049259 475.8893903 379.8211862 420.5922675 388.8584753 230.1794016
##  [73]  29.6346173 531.2614150 684.2571679  82.1317388  22.6144663 273.1581759
##  [79] 373.9245753 128.0558456 692.0181639 546.2022151 134.0667482 875.7218486
##  [85] 261.0591126 242.6213648 220.8787291 953.6133921 324.5823658 809.9891061
##  [91]  29.6787191 706.7882018 819.1550428 642.3830071 196.5823381 404.6445782
##  [97] 485.1826637 482.0555861 104.4058145 916.4104878
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
## [1] 44936.86
```

```r
sum(x)
```

```
## [1] 44936.86
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
## [1]  4782.552 10009.401 20350.742
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
## [1] 44936.86
```

Ao fazer isso, eu ganho duas vantagens:

- Manutenção: sempre que eu precisar repetir a operação, eu consigo simplificar muito meu código. Não preciso escrever um for loop para cada soma que eu precisar fazer. Se meus requerimentos mudarem no futuro, eu só preciso mudar um pedaço de código.

- Leitura: o humano que lê um `for` loop vai precisar de um minuto para se familiarizar com a operação e entender o que está sendo iterado, calculado, etc. O humano que lê "soma" sabe que ocorrerá uma soma. Você alinha a **expectativa** com a **execução**.

Outro exemplo.


```r
soma_xyz <- function(x) {
  soma_xyz <- vector(mode = "double", length = nrow(x))
  for (i in seq_along(df$soma_xyz)) {
    soma_xyz[[i]] <- x[[1]][[i]] + x[[2]][[i]] + x[[3]][i]
  }
  soma_xyz
}

df$soma_xyz2 <- soma_xyz(df)
```

```
## Warning: Unknown or uninitialised column: `soma_xyz`.
```

```r
df
```

```
## # A tibble: 100 x 4
##        x     y     z soma_xyz2
##    <dbl> <dbl> <dbl>     <dbl>
##  1  91.3  90.4  195.         0
##  2  27.6  98.3  245.         0
##  3  37.5  46.6  195.         0
##  4  63.8  90.3  196.         0
##  5  62.6  94.4  247.         0
##  6  27.2 112.   209.         0
##  7  55.4  78.6  217.         0
##  8  50.3 113.   233.         0
##  9  34.3  74.5  235.         0
## 10  68.8  94.4  193.         0
## # ... with 90 more rows
```

Outra vantagem de ter na mão uma função, é que eu posso me apropriar das ferramentas de programação funcional do R. São as funções da família `apply`, que recebem uma lista de objetos e aplicam uma função em cada um. Basicamente, aquela **abstração** do for loop que estavamos buscando.

Vejam como eu posso recriar o exemplo das somas das colunas do `data.frame` usando a função soma.


```r
sapply(df, soma)
```

```
##         x         y         z soma_xyz2 
##  4782.552 10009.401 20350.742     0.000
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

- `reduce()` e `accumulate()``: generalizam as operações de sumarização (pense soma acumulada, média, limite, fatorial)

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
## [1] 4782.552
## 
## $y
## [1] 10009.4
## 
## $z
## [1] 20350.74
## 
## $soma_xyz2
## [1] 0
```

```r
df %>% map(sum)
```

```
## $x
## [1] 4782.552
## 
## $y
## [1] 10009.4
## 
## $z
## [1] 20350.74
## 
## $soma_xyz2
## [1] 0
```

Embora existam pequenas diferenças técnicas entre elas, a principal vantagem de `map` é a possibilidade de criar funções anônimas com uma síntaxe enxuta. Pense, por exemplo, em elevar ao quadrado.


```r
df %>% lapply(function(x) x ^ 2)
```

```
## $x
##   [1] 8.340665e+03 7.609719e+02 1.407304e+03 4.072748e+03 3.918333e+03
##   [6] 7.390810e+02 3.066834e+03 2.531999e+03 1.177876e+03 4.727122e+03
##  [11] 3.629937e+03 1.025907e+04 5.184645e+03 8.465455e+02 2.497259e+03
##  [16] 9.040971e+02 6.071589e+03 1.218469e+03 2.060830e+02 2.980012e+03
##  [21] 2.926861e+03 1.737000e+03 7.159171e+02 3.745861e+03 2.286992e+03
##  [26] 1.139618e+03 1.385856e+03 1.473180e+03 7.256814e+02 2.324368e+02
##  [31] 6.280350e+02 2.798749e+03 2.295509e+03 5.492001e+03 1.603682e+03
##  [36] 5.921566e+03 2.535633e+03 2.869353e+03 3.539944e+00 2.774229e+03
##  [41] 6.126133e+03 1.307888e+02 5.432382e+03 1.080891e+03 5.239030e+03
##  [46] 3.527635e+01 3.322854e+02 8.144258e+03 2.140008e+00 8.174575e-03
##  [51] 4.845700e+03 9.943035e+03 3.519638e+01 6.476945e+03 1.017417e+03
##  [56] 2.017825e+03 8.164210e+01 5.076489e+03 1.774837e+03 1.267500e+03
##  [61] 2.547740e+03 1.158167e+03 1.969161e+02 5.625541e+02 3.086190e+03
##  [66] 1.822884e+03 2.031231e+03 5.032518e+03 1.697567e+03 1.640038e+03
##  [71] 1.956268e+03 5.095183e+03 3.193288e+03 1.282227e+03 8.508813e+02
##  [76] 4.323554e+03 4.485009e+03 2.598271e+01 4.245055e+03 8.807886e+03
##  [81] 5.279595e+02 1.690810e+03 1.494979e+03 8.323780e+03 2.970652e+03
##  [86] 7.973331e+03 4.298083e+03 1.490143e+03 7.355707e+03 6.888136e+02
##  [91] 4.577101e+03 8.548181e+03 8.335626e+02 8.041585e+02 7.036801e+03
##  [96] 1.956278e+01 7.482131e+03 2.508999e+03 6.958052e+03 3.651068e+03
## 
## $y
##   [1]  8175.802  9655.427  2173.739  8162.567  8918.293 12647.369  6174.668
##   [8] 12853.036  5548.858  8907.042  3598.469 19733.664  9939.235 12768.554
##  [15] 14729.183  9798.012  6904.748  5388.004  3211.408 11746.864  4284.553
##  [22]  7587.151  3573.968 12485.685 12581.851  8549.839 13242.744  6221.696
##  [29] 23138.896 15254.354  5126.643  9314.245  7027.650  7238.021  9298.434
##  [36]  9590.037 16230.142 21024.441 10360.863 20232.282  5968.434 11773.480
##  [43]  9352.594  6487.212 14268.116 22835.777  6249.220 14092.213  7756.533
##  [50] 16457.960  6619.314  6553.217 19060.770  7766.440  8047.052  6098.922
##  [57] 10806.751 11915.118  9127.615  6985.857 11945.977  7995.177  6593.833
##  [64] 20428.728 16515.877 15453.485 16747.701  5506.670 29666.589 12339.310
##  [71]  8591.397  9430.074  8469.947 15936.118  6924.588 11339.666  7310.403
##  [78]  9806.804  4779.000 10068.194  2533.711  3112.751  5741.073 17578.210
##  [85]  3095.715 32905.527  4820.790  6854.842 17001.563  6286.864 15818.163
##  [92]  8620.866 10772.858 15410.029 11357.589 21756.877 11183.605  5818.452
##  [99] 17986.063  5486.993
## 
## $z
##   [1] 38166.20 59835.67 38148.02 38429.29 60772.90 43500.93 47008.10 54156.79
##   [9] 55144.65 37181.51 43479.82 34076.33 32159.97 43002.82 20928.52 44749.75
##  [17] 34836.89 61083.71 34735.53 29604.31 39366.31 46551.25 45708.43 42917.59
##  [25] 24308.75 31554.57 41343.99 48125.42 33339.10 31703.37 52953.02 40069.31
##  [33] 46482.75 40190.46 49221.78 36963.49 26005.78 44222.12 36592.87 48526.54
##  [41] 54375.20 46506.28 14958.35 36557.16 36944.87 58310.41 36741.60 41539.45
##  [49] 32371.12 41014.68 44839.06 35222.26 45268.79 29866.27 49296.33 42969.19
##  [57] 34748.62 39555.17 57497.89 48875.87 47479.51 24623.18 61368.62 45975.52
##  [65] 52805.84 54308.65 40488.29 30223.55 44914.23 44364.25 32447.04 23491.32
##  [73] 44225.58 41328.19 36727.72 55035.62 75682.59 44429.90 42944.31 47937.10
##  [81] 61786.95 50779.65 51986.37 37411.78 41597.63 36160.43 32692.37 45780.12
##  [89] 44958.06 31920.22 40491.99 40352.93 48159.41 38318.30 41328.63 30426.50
##  [97] 42943.59 33370.48 42763.44 32189.52
## 
## $soma_xyz2
##   [1] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [38] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [75] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
```

```r
df %>% map(~ .x ^ 2)
```

```
## $x
##   [1] 8.340665e+03 7.609719e+02 1.407304e+03 4.072748e+03 3.918333e+03
##   [6] 7.390810e+02 3.066834e+03 2.531999e+03 1.177876e+03 4.727122e+03
##  [11] 3.629937e+03 1.025907e+04 5.184645e+03 8.465455e+02 2.497259e+03
##  [16] 9.040971e+02 6.071589e+03 1.218469e+03 2.060830e+02 2.980012e+03
##  [21] 2.926861e+03 1.737000e+03 7.159171e+02 3.745861e+03 2.286992e+03
##  [26] 1.139618e+03 1.385856e+03 1.473180e+03 7.256814e+02 2.324368e+02
##  [31] 6.280350e+02 2.798749e+03 2.295509e+03 5.492001e+03 1.603682e+03
##  [36] 5.921566e+03 2.535633e+03 2.869353e+03 3.539944e+00 2.774229e+03
##  [41] 6.126133e+03 1.307888e+02 5.432382e+03 1.080891e+03 5.239030e+03
##  [46] 3.527635e+01 3.322854e+02 8.144258e+03 2.140008e+00 8.174575e-03
##  [51] 4.845700e+03 9.943035e+03 3.519638e+01 6.476945e+03 1.017417e+03
##  [56] 2.017825e+03 8.164210e+01 5.076489e+03 1.774837e+03 1.267500e+03
##  [61] 2.547740e+03 1.158167e+03 1.969161e+02 5.625541e+02 3.086190e+03
##  [66] 1.822884e+03 2.031231e+03 5.032518e+03 1.697567e+03 1.640038e+03
##  [71] 1.956268e+03 5.095183e+03 3.193288e+03 1.282227e+03 8.508813e+02
##  [76] 4.323554e+03 4.485009e+03 2.598271e+01 4.245055e+03 8.807886e+03
##  [81] 5.279595e+02 1.690810e+03 1.494979e+03 8.323780e+03 2.970652e+03
##  [86] 7.973331e+03 4.298083e+03 1.490143e+03 7.355707e+03 6.888136e+02
##  [91] 4.577101e+03 8.548181e+03 8.335626e+02 8.041585e+02 7.036801e+03
##  [96] 1.956278e+01 7.482131e+03 2.508999e+03 6.958052e+03 3.651068e+03
## 
## $y
##   [1]  8175.802  9655.427  2173.739  8162.567  8918.293 12647.369  6174.668
##   [8] 12853.036  5548.858  8907.042  3598.469 19733.664  9939.235 12768.554
##  [15] 14729.183  9798.012  6904.748  5388.004  3211.408 11746.864  4284.553
##  [22]  7587.151  3573.968 12485.685 12581.851  8549.839 13242.744  6221.696
##  [29] 23138.896 15254.354  5126.643  9314.245  7027.650  7238.021  9298.434
##  [36]  9590.037 16230.142 21024.441 10360.863 20232.282  5968.434 11773.480
##  [43]  9352.594  6487.212 14268.116 22835.777  6249.220 14092.213  7756.533
##  [50] 16457.960  6619.314  6553.217 19060.770  7766.440  8047.052  6098.922
##  [57] 10806.751 11915.118  9127.615  6985.857 11945.977  7995.177  6593.833
##  [64] 20428.728 16515.877 15453.485 16747.701  5506.670 29666.589 12339.310
##  [71]  8591.397  9430.074  8469.947 15936.118  6924.588 11339.666  7310.403
##  [78]  9806.804  4779.000 10068.194  2533.711  3112.751  5741.073 17578.210
##  [85]  3095.715 32905.527  4820.790  6854.842 17001.563  6286.864 15818.163
##  [92]  8620.866 10772.858 15410.029 11357.589 21756.877 11183.605  5818.452
##  [99] 17986.063  5486.993
## 
## $z
##   [1] 38166.20 59835.67 38148.02 38429.29 60772.90 43500.93 47008.10 54156.79
##   [9] 55144.65 37181.51 43479.82 34076.33 32159.97 43002.82 20928.52 44749.75
##  [17] 34836.89 61083.71 34735.53 29604.31 39366.31 46551.25 45708.43 42917.59
##  [25] 24308.75 31554.57 41343.99 48125.42 33339.10 31703.37 52953.02 40069.31
##  [33] 46482.75 40190.46 49221.78 36963.49 26005.78 44222.12 36592.87 48526.54
##  [41] 54375.20 46506.28 14958.35 36557.16 36944.87 58310.41 36741.60 41539.45
##  [49] 32371.12 41014.68 44839.06 35222.26 45268.79 29866.27 49296.33 42969.19
##  [57] 34748.62 39555.17 57497.89 48875.87 47479.51 24623.18 61368.62 45975.52
##  [65] 52805.84 54308.65 40488.29 30223.55 44914.23 44364.25 32447.04 23491.32
##  [73] 44225.58 41328.19 36727.72 55035.62 75682.59 44429.90 42944.31 47937.10
##  [81] 61786.95 50779.65 51986.37 37411.78 41597.63 36160.43 32692.37 45780.12
##  [89] 44958.06 31920.22 40491.99 40352.93 48159.41 38318.30 41328.63 30426.50
##  [97] 42943.59 33370.48 42763.44 32189.52
## 
## $soma_xyz2
##   [1] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [38] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [75] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
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
##  1  0.841  2.68    0.0835
##  2  0.554 -0.131   0.526 
##  3 -0.171  0.614   0.898 
##  4  0.861  0.887   0.550 
##  5 -0.512  0.514  -0.191 
##  6  0.593 -0.498  -0.137 
##  7 -1.18  -0.298  -1.14  
##  8 -0.321  0.0182  0.693 
##  9 -0.993 -0.0592  0.507 
## 10 -1.20  -0.625  -0.678 
## # ... with 90 more rows
## 
## [[2]]
## # A tibble: 100 x 3
##         x       y       z
##     <dbl>   <dbl>   <dbl>
##  1  0.101  0.259   0.282 
##  2 -0.370 -0.223  -0.622 
##  3  1.66   0.0762  1.87  
##  4 -0.289  0.111  -2.20  
##  5  0.174 -0.695  -0.0989
##  6 -0.647 -0.709   1.53  
##  7  0.360 -0.966  -1.14  
##  8 -1.36   1.06    0.246 
##  9 -2.22  -1.33   -0.805 
## 10  0.881 -0.920   1.84  
## # ... with 90 more rows
## 
## [[3]]
## # A tibble: 100 x 3
##         x       y       z
##     <dbl>   <dbl>   <dbl>
##  1 -1.45  -2.37   -0.563 
##  2  0.161  0.473   0.0762
##  3  0.546 -0.764   1.05  
##  4  1.68   0.240   1.27  
##  5  1.19  -2.24   -0.211 
##  6 -1.76  -1.44    0.276 
##  7  0.823 -0.562   1.15  
##  8 -0.358  0.159   0.261 
##  9  0.325 -0.250  -0.650 
## 10  1.03  -0.0697  2.19  
## # ... with 90 more rows
## 
## [[4]]
## # A tibble: 100 x 3
##           x       y      z
##       <dbl>   <dbl>  <dbl>
##  1  0.551    1.12   -0.958
##  2  0.00661  1.04    1.38 
##  3  1.00    -1.78    0.405
##  4 -1.99     1.96    0.723
##  5  0.0453  -0.944   1.29 
##  6  1.97     1.16    1.12 
##  7 -0.151   -0.452  -0.134
##  8 -2.06    -0.115  -0.436
##  9  1.00     1.62   -1.11 
## 10 -0.546    0.0693 -0.955
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
##  1  0.841  2.68    0.0835
##  2  0.554 -0.131   0.526 
##  3 -0.171  0.614   0.898 
##  4  0.861  0.887   0.550 
##  5 -0.512  0.514  -0.191 
##  6  0.593 -0.498  -0.137 
##  7 -1.18  -0.298  -1.14  
##  8 -0.321  0.0182  0.693 
##  9 -0.993 -0.0592  0.507 
## 10 -1.20  -0.625  -0.678 
## # ... with 90 more rows
## 
## [[2]]
## # A tibble: 100 x 3
##         x       y       z
##     <dbl>   <dbl>   <dbl>
##  1  0.101  0.259   0.282 
##  2 -0.370 -0.223  -0.622 
##  3  1.66   0.0762  1.87  
##  4 -0.289  0.111  -2.20  
##  5  0.174 -0.695  -0.0989
##  6 -0.647 -0.709   1.53  
##  7  0.360 -0.966  -1.14  
##  8 -1.36   1.06    0.246 
##  9 -2.22  -1.33   -0.805 
## 10  0.881 -0.920   1.84  
## # ... with 90 more rows
## 
## [[3]]
## # A tibble: 100 x 3
##         x       y       z
##     <dbl>   <dbl>   <dbl>
##  1 -1.45  -2.37   -0.563 
##  2  0.161  0.473   0.0762
##  3  0.546 -0.764   1.05  
##  4  1.68   0.240   1.27  
##  5  1.19  -2.24   -0.211 
##  6 -1.76  -1.44    0.276 
##  7  0.823 -0.562   1.15  
##  8 -0.358  0.159   0.261 
##  9  0.325 -0.250  -0.650 
## 10  1.03  -0.0697  2.19  
## # ... with 90 more rows
## 
## [[4]]
## # A tibble: 100 x 3
##           x       y      z
##       <dbl>   <dbl>  <dbl>
##  1  0.551    1.12   -0.958
##  2  0.00661  1.04    1.38 
##  3  1.00    -1.78    0.405
##  4 -1.99     1.96    0.723
##  5  0.0453  -0.944   1.29 
##  6  1.97     1.16    1.12 
##  7 -0.151   -0.452  -0.134
##  8 -2.06    -0.115  -0.436
##  9  1.00     1.62   -1.11 
## 10 -0.546    0.0693 -0.955
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
##  1 1        0.841  2.68    0.0835
##  2 1        0.554 -0.131   0.526 
##  3 1       -0.171  0.614   0.898 
##  4 1        0.861  0.887   0.550 
##  5 1       -0.512  0.514  -0.191 
##  6 1        0.593 -0.498  -0.137 
##  7 1       -1.18  -0.298  -1.14  
##  8 1       -0.321  0.0182  0.693 
##  9 1       -0.993 -0.0592  0.507 
## 10 1       -1.20  -0.625  -0.678 
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
##  [1] 43.17877 48.76450 51.29309 40.21730 50.91338 49.95989 54.53964 44.23765
##  [9] 56.14614 37.00182 55.60017 44.17833 33.21468 54.57325 60.78264 39.49895
## [17] 40.34316 24.39014 68.37701 53.10101 62.45857 73.02202 49.60368 48.17334
## [25] 57.83333
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
##  $ : num 49.7
##  $ : num 10.9
##  $ : num 12.4
##  $ : num 1241
##  $ : int 25
##  $ : num [1:2] 24.4 73
##  $ : num 24.4
##  $ : num 73
```

Fica mais bonito se você nomear os argumentos, ai o output fica melhorzinho.


```r
names(funcs) <- unlist(funcs)

invoke_map(funcs, args, x = numeros) %>% glimpse()
```

```
## List of 8
##  $ mean  : num 49.7
##  $ sd    : num 10.9
##  $ IQR   : num 12.4
##  $ sum   : num 1241
##  $ length: int 25
##  $ range : num [1:2] 24.4 73
##  $ min   : num 24.4
##  $ max   : num 73
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
##   ..$ x: num [1:100] 0.841 0.554 -0.171 0.861 -0.512 ...
##   ..$ y: num [1:100] 2.679 -0.131 0.614 0.887 0.514 ...
##   ..$ z: num [1:100] 0.0835 0.5261 0.8976 0.5499 -0.1911 ...
##   ..- attr(*, "spec")=
##   .. .. cols(
##   .. ..   x = col_double(),
##   .. ..   y = col_double(),
##   .. ..   z = col_double()
##   .. .. )
##  $ : spec_tbl_df [100 x 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##   ..$ x: num [1:100] 0.101 -0.37 1.658 -0.289 0.174 ...
##   ..$ y: num [1:100] 0.2594 -0.2229 0.0762 0.1111 -0.6952 ...
##   ..$ z: num [1:100] 0.2824 -0.6223 1.8727 -2.201 -0.0989 ...
##   ..- attr(*, "spec")=
##   .. .. cols(
##   .. ..   x = col_double(),
##   .. ..   y = col_double(),
##   .. ..   z = col_double()
##   .. .. )
##  $ : spec_tbl_df [100 x 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##   ..$ x: num [1:100] -1.453 0.161 0.546 1.678 1.188 ...
##   ..$ y: num [1:100] -2.369 0.473 -0.764 0.24 -2.241 ...
##   ..$ z: num [1:100] -0.5626 0.0762 1.0544 1.2732 -0.2112 ...
##   ..- attr(*, "spec")=
##   .. .. cols(
##   .. ..   x = col_double(),
##   .. ..   y = col_double(),
##   .. ..   z = col_double()
##   .. .. )
##  $ : spec_tbl_df [100 x 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##   ..$ x: num [1:100] 0.55069 0.00661 1.00478 -1.99002 0.04531 ...
##   ..$ y: num [1:100] 1.12 1.044 -1.779 1.963 -0.944 ...
##   ..$ z: num [1:100] -0.958 1.379 0.405 0.723 1.286 ...
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
##  1  0.841  2.68    0.0835
##  2  0.554 -0.131   0.526 
##  3 -0.171  0.614   0.898 
##  4  0.861  0.887   0.550 
##  5 -0.512  0.514  -0.191 
##  6  0.593 -0.498  -0.137 
##  7 -1.18  -0.298  -1.14  
##  8 -0.321  0.0182  0.693 
##  9 -0.993 -0.0592  0.507 
## 10 -1.20  -0.625  -0.678 
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

