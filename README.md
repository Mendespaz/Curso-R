# Curso-R


# 1 Introdução

## 1.1. O R como uma calculadora

No seu nível mais básico, o R funciona como uma calculadora. Podemos usá-lo para fazer operações matemáticas diretas.

```{r}
# Adição
2 + 2 

#Subtração
2 - 2

# Divisão
4 / 2

# Multiplicação
4 * 2 

# Potência
2 ^ 2

```

## 1.2. Guardando informações (Criando Variáveis)

Na maior parte do tempo, não queremos apenas calcular algo e perder o resultado. Queremos guardar essa informação na memória do computador para usar depois.

(Dica: O atalho **Alt** + **-** coloca a setinha diretamente)

```{r}
# Adição
x = 2 + 2
x
# Divisão
y <-  20 / 2
y
# Multiplicação
x * y

```

## 1.3. Tipos de Dados

 O R lida basicamente com três tipos principais de dados no dia a dia:

**Números**: Digitamos diretamente (ex: 20, 8.5).

```{r}
# Criando uma lista com as idades (Números inteiros)
idades <- c(19, 22, 18, 25)

# Criando uma lista com as notas (Números decimais - use sempre PONTO em vez de vírgula)
notas <- c(8.5, 6.0, 9.2, 3.7)
```

**Textos**: Precisam obrigatoriamente estar entre aspas duplas " " ou simples ' '.

```{r}
# Incorreto:
#aluno_exemplo <- Ana

# Correto:
#aluno_exemplo <- "Ana"

# Criando uma lista com o nome dos alunos
alunos <- c("Ana", "Bruno", "Carlos", "Luiz")

```

**Valores Lógicos**: Digitamos diretamente, sempre em letras maiúsculas (ex: TRUE ou FALSE).

Dica: **TAB** complete automaticamente.

```{r}
# Criando uma lista lógica (Verdadeiro ou Falso)
aprovado <- c(TRUE, TRUE, TRUE, FALSE)
```

### 1.3.1 Classes de Dados

```{r}
class(notas)
class(alunos)
class(aprovado)
```

*Alterando Classes*: Podemos forçar a mudança de classe de um objeto.

```{r}
aprovado_num <- as.numeric(aprovado) #
aprovado_num
class(aprovado_num)
```

## 1.4. Trabalhando e Manipulando Vetores

As listas que criamos acima com a função c() são chamadas de vetores. Podemos manipulá-los de várias formas:  Selecionando elementos específicos com colchetes []:

```{r}
alunos[2]
notas[2] 
```

```{r}
table(aprovado) # Conta quantos passaram e quantos reprovaram[cite: 2]
rev(alunos)     # Inverte a ordem da lista (Luiz, Carlos, Bruno, Ana)[cite: 2]
unique(idades)  # Mostra apenas os valores únicos sem repetição[cite: 2]
```

## 1.5. Explorando o Ambiente R

```{r}
ls()
rm("aprovado_num")
```

## 1.6. Automatizando com Lógica

Existem 6 operadores principais no R para comparar valores. Eles sempre vão te responder com TRUE ou FALSE:

== : Exatamente igual a.

!= : Diferente de.

> : Maior que.

\< : Menor que.

> = : Maior ou igual a.

\<= : Menor ou igual a.

```{r}
# Quem tirou EXATAMENTE 6.0?
notas == 6.0

# Quem tirou uma nota DIFERENTE de 6.0?
notas != 6.0

# Quais alunos têm 20 anos ou menos?
idades <= 20

# Perguntando ao R: Quais notas são maiores ou iguais a 6.0?
notas >= 6.0
```

## 1.7. Operadores Lógicos: Juntando perguntas (E / OU)

Muitas vezes, precisamos fazer mais de uma pergunta ao mesmo tempo. Para isso, usamos os operadores lógicos. Os 2 mais importantes no dia a dia são:

& (E): Todas as condições precisam ser verdadeiras.

| (OU): Pelo menos uma condição precisa ser verdadeira.

```{r}
# &: O aluno tem menos de 20 anos e tirou nota maior que 8?
# Só será TRUE se as duas coisas forem verdade.
(idades < 20) & (notas > 8.0)
alunos[(idades < 20) & (notas > 8.0)]

# OU (|): O aluno tem mais de 20 anos OU tirou nota maior que 8?
# Será TRUE se pelo menos UMA das coisas for verdade.
(idades < 20) | (notas > 8.0)
sum((idades < 20) | (notas > 8.0))
```

### 1.8. Outras logicas de comandos

if (se)
else (senão)

```{r}
# Vamos checar a nota de apenas um aluno
# O código dentro das chaves {} só vai rodar se a condição for TRUE
if (notas[2] >= 6.0) {
  print(paste("Parabéns", alunos[2], "você foi aprovado, pode curtir as férias!"))
} else {
  print(paste("Infelizmente", alunos[2], "você reprovou, nos vemos ano que vem."))
}
```

### 1.8.1. Automatizando repetições: O Loop

```{r}
for (i in 1:4) {
  
  if (notas[i] >= 7.0) {
    print(paste("Parabéns", alunos[i], "você foi aprovado, pode curtir as férias."))
  } else {
    print(paste("Infelizmente", alunos[i], "você reprovou, nos vemos ano que vem."))
  }
  
}
```


```{r}
# Criando a lista de situação automaticamente
resultado <- ifelse(notas >= 6.0, "Aprovado", "Reprovado")

# Vendo o resultado
resultado
```

# 2. Trabalhando com dados reais

Até agora, nós criamos nossos próprios dados digitando no R. Mas no mundo real, os dados chegam em arquivos como planilhas do Excel. Vamos aprender a importar, inspecionar e limpar esses dados.

## 2.1. Importando e Inspecionando os Dados

Antes de fazer qualquer conta ou gráfico, a regra número um do analista de dados é **olhar para os dados**. Vamos descobrir em qual pasta o R está trabalhando no momento, carregar o pacote `readxl` e puxar a nossa planilha.

```{r importacao-inspecao}
getwd()
library(readxl)

Dados <- read_excel("Notas.xlsx")
View(Dados)
```
O comando `View()` vai abrir a tabela em uma aba nova, como se fosse o Excel. Ao fazer isso pela primeira vez, notaremos que a primeira linha do arquivo veio bagunçada.

```{r}
Dados <- read_excel("Notas.xlsx", skip = 1, col_names = TRUE)
View(Dados)
colnames(Dados) <- c("aluno", "mat_primeira", "mat_segunda", "mat_terceira")
View(Dados)
summary(Dados)
Dados$mat_primeira <- as.numeric(Dados$mat_primeira)
Dados$mat_segunda <- as.numeric(Dados$mat_segunda)
Dados$mat_terceira <- as.numeric(Dados$mat_terceira)

summary(Dados)
Dados$mat_segunda[which.max(Dados$mat_segunda)] <- max(Dados$mat_segunda)/10
summary(Dados)
```

```{r}
# 1. Extraindo as notas
notas_maria <- as.numeric(Dados[Dados$aluno == "Maria", 2:4])

# 2. Calculando a média
media_maria <- mean(notas_maria, na.rm = TRUE)

# Imprimindo a média
print(paste("A média da Maria foi:", round(media_maria, 2)))

# 3. Plotando o gráfico novamente
plot(notas_maria, 
     type = "b", pch = 19, col = "steelblue", ylim = c(0, 10), xaxt = "n",
     main = "Desempenho da Aluna: Maria", xlab = "Avaliação", ylab = "Nota")
axis(side = 1, at = 1:3, labels = c("Primeira", "Segunda", "Terceira"))

# 4. Adicionando a linha da média PESSOAL dela em verde
abline(h = media_maria, col = "green")

# Adicionando a linha da média de aprovação da escola em vermelho
abline(h = 6, col = "red")
```

```{r}
# Calculando a média de todos os alunos de uma só vez
Dados$Media <- rowMeans(Dados[ , 2:4])

# Olhar as 6 primeiras linhas 
head(Dados)
```


```{r}
# Histograma básico
hist(Dados$Media)


hist(Dados$Media,
     main = "Distribuição das Médias Finais",
     xlab = "Média Final",
     ylab = "Frequência (Nº de alunos)",
     col = "lightgreen",
     border = "white",
     xlim = c(0, 10))

# Adicionando a linha de corte da escola (nota 6.0)
abline(v = 6, col = "red", lwd = 2, lty = 2)
```

## 5.9. Visualizando a Média da Turma (Gráfico de Pontos)

Agora que calculamos a coluna `Media` para todos os alunos, vamos ver como a turma toda se saiu usando a função `plot()`.

Começamos com a versão mais simples que o R pode nos dar. O computador vai ler a coluna e colocar um ponto para cada aluno.


```{r plot-medias-simples}
# Gráfico de pontos básico da Média
plot(Dados$Media)

# Evoluindo o gráfico de pontos
plot(Dados$Media,
     main = "Desempenho Geral: Média Final por aluno",
     xlab = "Número do aluno (Índice da Tabela)",
     ylab = "Média Final",
     pch = 19,               # Usa bolinhas preenchidas
     col = "blue",         # Cor dos pontos
     ylim = c(0, 10))        # Fixa o eixo Y entre 0 e 10

# Adicionando a linha de corte (nota 6)
abline(h = 6, col = "red")
```

# 3. Exercício Prático Final

```{r}
# Se a média for maior ou igual a 6.0, Aprovado. Senão, Reprovado.
Dados$Situacao <- ifelse(Dados$Media >= 6.0, "Aprovado", "Reprovado")

# Visualizando a tabela final completa!
View(Dados)
```

## 3.1. Quantos passaram? O Gráfico de Barras

Agora que temos a nossa coluna `Situacao` com textos ("Aprovado" e "Reprovado"), a primeira coisa que queremos saber é: quantos alunos estão em cada categoria?

Para o R fazer um gráfico disso, primeiro precisamos pedir para ele **contar** as categorias. Fazemos isso com a função `table()` (tabela de contagem).


```{r contagem-simples}
# Pedindo ao R para contar quantos alunos existem em cada situação
table(Dados$Situacao)

# Gráfico de barras
barplot(table(Dados$Situacao))

# Evoluindo o gráfico de barras
barplot(table(Dados$Situacao),
        main = "Situação Final da Turma",
        ylab = "Número de alunos",
        col = c("green", "red"),
        border = "black",
        ylim = c(0,22))
```
