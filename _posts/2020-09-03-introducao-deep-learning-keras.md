---
title: "Introdução à Deep Learning usando Keras"
date: 2020-09-03
tags: [deep learning, python, keras]
#header:
#  image: "/images/salary/manchete.png"
excerpt: "Compreendendo o processo de se utilizar redes neurais para solucionar problemas de regressão com Keras."
mathjax: "true"
---

Sempre acompanhei palestras ou aulas que buscavam explicar algum trabalho relacionado à Deep Learning com um olhar admirado, mas ao mesmo tempo sem entender muita coisa. Aproveitando que o DataCamp disponibilizou uma semana gratuita para todos os seus cursos, resolvi finalmente me desafiar e aprender mais a respeito. Seguindo a track [Deep Learning in Python](https://learn.datacamp.com/skill-tracks/deep-learning-in-python?version=1), comecei então pelo curso ["Introduction to Deep Learning in Python"](https://learn.datacamp.com/courses/introduction-to-deep-learning-in-python). Confesso que me surpreendi com a fluidez do material e tento então através desta publicação transmitir um pouco do que estou aprendendo.

Nesta publicação tentarei então compartilhar uma ideia de **como construir modelos de Deep Learning com Keras**. Keras é uma biblioteca de rede neural de código aberto escrita em Pytho, que iremos utilizar. O modelo de Deep Learning pode então ser dividido em quatro passos:

### 1. Especificação da Arquitetura

Neste passo, você deve então específicar a arquitetura da rede neural, respondendo algumas perguntas como:

- Quantas camadas a rede possuirá?
- Quantos nós existirão em cada camada?
- Qual função de ativação será utilizada em cada camada?

Respondendo estas perguntas, você consegue começar a imaginar o formato da sua rede neural, como seria a sua disposição ideal. A Figura 1 representa um exemplo de arquitetura, onde a rede possui duas camadas ocultas, uma camada de entrada e uma de saída. Além disso, são definidos quatro nós para as camadas ocultas, três para a camada de entrada e apenas um para a camada de saída.

<center>
<img src="https://cdn-images-1.medium.com/max/1000/1*livHOtvW8PSptrSb7OXpyA.jpeg" style="width:75%">

<b>Figura 1.</b> Exemplo de Rede Neural que será utilizada como exemplo. 
</center>

Para especificar o modelo da Figura 1 utilizando Keras, você pode utilizar o seguinte código:

```python
import pandas as pd
from keras.layers import Dense
from keras.models import Sequential

#Obtendo os dados de entrada
data = pd.read_csv('dados_entrada.csv')
predictors = data.drop(['resultado'], axis=1).as_matrix()
target = data['resultado'] 

#Utilizando o número de colunas para delimitar os nós de entrada (3 nós)
num_cols = predictors.shape[1]

#Iniciando uma rede neural sequencial
model = Sequential()

#Instanciando a primeira camada oculta com 4 nós a partir dos 3 nós da entrada
model.add(Dense(4, activation='relu', input_shape=(n_cols, )))

#Instanciando a segunda camada oculta com 4 nós
model.add(Dense(4, activation='relu', input_shape=(n_cols, )))

#Instanciando a camada de saída com apenas 1 nó
model.add(Dense(1))
```

### 2. Compilação

Agora, você pode compilar o modelo, configurando a mesma para a otimização. Neste passo você deve especificar a função de perda e alguns detalhes sobre como a otimização da rede neural irá funcionar.  

Os métodos de compilação possuem então dois argumentos importantes para você escolher:

  1. Qual otimizador será utilizado? R: Neste caso, apresentarei o "Adam"
  2. Qual será a função de perda? R: Neste caso, apresentarei o erro quadrático médio, por se tratar de um exemplo de regressão (um nó de saída).

O otimizador está diretamente relacionado com a taxa de aprendizado da rede neural. Continuando o código em Python apresentado como exemplo, irei compilar o modelo da seguinte maneira:

```python
#Compilando o modelo proposto para a rede neural 
model.compile(optimizer='adam', loss='mean_squared_error', metrics=['accuracy'])
```

### 3. Fit (Encaixe)

Após compilar o modelo com a arquitetura proposta, você pode então encaixar o modelo. Neste passo então você literalmente encaixa a rede neural formulada com os dados de entrada. É aqui que você irá definir e aplicar os ciclos de backpropagation e como será realizada a otimização dos pesos do modelo com os seus dados.

Continuando o código em Python apresentado como exemplo, irei fazer o fit do modelo da seguinte maneira:

```python
#Encaixando a rede neural com os dados de entrada e o resultado esperado
model.fit(predictors, target, validation_split=0.3)
```

Após realizar o fit do seu modelo, é recomendado salvar o mesmo. Para salvar, utilize o seguinte:

```python
#Salvando o modelo para uso posterior
model.save('model.h5')
```

### 4. Predição

E por fim, com a sua rede neural já treinada com os dados de entrada, você poderá utilizar o modelo para realizar certas predições. Com o modelo salvo, devemos então recarregá-lo e utilizá-lo posteriormente. Isto pode ser feito através do seguinte código:

```python
#Carregando o modelo criado anteriormente e os dados a serem preditos
the_model = load_model('model.h5')
data_predict = pd.read_csv('dados_predicao.csv')

predictions = the_model.predict(data_predict)
```

_____________________________

### Conclusão 

E é isso! Com estas pouquissímas linhas de código você apreendeu a criar uma rede neural que consegue estimar uma predição na saída, conforme os dados de entrada. Claro que esta rede é extremamente pequena, mas a ideia aqui era apenas fornecer de maneira simples o que muitas vezes é desesperador ao se ouvir falar pela primeira vez.