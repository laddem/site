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

## Estatística descritiva em R

### Somas

#### Exercício

### na.rm

#### Exercício

### Contagens

#### Exercício

### Médias


#### Média "trimmed"

#### Exercício

### Medianas

#### Exercício

### Amplitude

<!-- Incluir também min() e max() além de range() -->

#### Exercício

### Percentis

#### Exercício

### Distância interquartil

#### Exercício

### Variância

#### Exercício

### Desvio padrão

#### Exercício

### Desvio absoluto da mediana

#### Exercício

### Sumário

#### Exercício

### Estatísticas por grupo

<!-- by() e aggregate() -->

#### Exercício

### Escores e testes

<!-- p-valores, t-valores, f-valores e seus respectivos testes -->

#### Exercício

### Correlações

#### Exercício

### complete.obs

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
