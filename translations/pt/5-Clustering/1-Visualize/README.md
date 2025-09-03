<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "0ab69b161efd7a41d325ee28b29415d7",
  "translation_date": "2025-09-03T17:08:49+00:00",
  "source_file": "5-Clustering/1-Visualize/README.md",
  "language_code": "pt"
}
-->
# Introdução à clusterização

A clusterização é um tipo de [Aprendizagem Não Supervisionada](https://wikipedia.org/wiki/Aprendizagem_n%C3%A3o_supervisionada) que parte do pressuposto de que um conjunto de dados não está rotulado ou que suas entradas não estão associadas a saídas predefinidas. Utiliza vários algoritmos para analisar dados não rotulados e fornecer agrupamentos com base nos padrões que identifica nos dados.

[![No One Like You por PSquare](https://img.youtube.com/vi/ty2advRiWJM/0.jpg)](https://youtu.be/ty2advRiWJM "No One Like You por PSquare")

> 🎥 Clique na imagem acima para assistir ao vídeo. Enquanto estuda aprendizagem de máquina com clusterização, aproveite algumas faixas de Dance Hall nigeriano - esta é uma música muito bem avaliada de 2014 por PSquare.

## [Questionário pré-aula](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/27/)

### Introdução

[A clusterização](https://link.springer.com/referenceworkentry/10.1007%2F978-0-387-30164-8_124) é muito útil para exploração de dados. Vamos ver se ela pode ajudar a descobrir tendências e padrões na forma como o público nigeriano consome música.

✅ Reserve um momento para pensar sobre os usos da clusterização. Na vida real, a clusterização acontece sempre que você tem uma pilha de roupa suja e precisa separar as roupas dos membros da sua família 🧦👕👖🩲. Na ciência de dados, a clusterização ocorre ao tentar analisar as preferências de um usuário ou determinar as características de qualquer conjunto de dados não rotulado. De certa forma, a clusterização ajuda a dar sentido ao caos, como uma gaveta de meias.

[![Introdução à ML](https://img.youtube.com/vi/esmzYhuFnds/0.jpg)](https://youtu.be/esmzYhuFnds "Introdução à Clusterização")

> 🎥 Clique na imagem acima para assistir ao vídeo: John Guttag do MIT apresenta a clusterização.

No ambiente profissional, a clusterização pode ser usada para determinar coisas como segmentação de mercado, identificando quais faixas etárias compram quais itens, por exemplo. Outro uso seria a detecção de anomalias, talvez para identificar fraudes em um conjunto de dados de transações com cartões de crédito. Ou você pode usar a clusterização para identificar tumores em um lote de exames médicos.

✅ Pense por um momento sobre como você pode ter encontrado clusterização 'na prática', em um ambiente bancário, de comércio eletrônico ou empresarial.

> 🎓 Curiosamente, a análise de clusters teve origem nos campos da Antropologia e Psicologia na década de 1930. Consegue imaginar como ela pode ter sido usada?

Alternativamente, você poderia usá-la para agrupar resultados de pesquisa - por links de compras, imagens ou avaliações, por exemplo. A clusterização é útil quando você tem um grande conjunto de dados que deseja reduzir e sobre o qual deseja realizar uma análise mais detalhada, sendo assim uma técnica que pode ser usada para aprender sobre os dados antes de construir outros modelos.

✅ Depois de organizar seus dados em clusters, você atribui a eles um Id de cluster, e essa técnica pode ser útil para preservar a privacidade de um conjunto de dados; você pode se referir a um ponto de dados pelo seu Id de cluster, em vez de usar dados identificáveis mais reveladores. Consegue pensar em outros motivos para usar um Id de cluster em vez de outros elementos do cluster para identificá-lo?

Aprofunde seu entendimento sobre técnicas de clusterização neste [módulo de aprendizagem](https://docs.microsoft.com/learn/modules/train-evaluate-cluster-models?WT.mc_id=academic-77952-leestott).

## Começando com clusterização

[O Scikit-learn oferece uma ampla gama](https://scikit-learn.org/stable/modules/clustering.html) de métodos para realizar clusterização. O tipo que você escolhe dependerá do seu caso de uso. De acordo com a documentação, cada método tem diferentes benefícios. Aqui está uma tabela simplificada dos métodos suportados pelo Scikit-learn e seus casos de uso apropriados:

| Nome do método                | Caso de uso                                                           |
| :---------------------------- | :-------------------------------------------------------------------- |
| K-Means                       | propósito geral, indutivo                                             |
| Propagação de afinidade       | muitos clusters desiguais, indutivo                                  |
| Mean-shift                    | muitos clusters desiguais, indutivo                                  |
| Clusterização espectral       | poucos clusters iguais, transdutivo                                  |
| Clusterização hierárquica Ward | muitos clusters restritos, transdutivo                              |
| Clusterização aglomerativa    | muitos clusters restritos, distâncias não euclidianas, transdutivo   |
| DBSCAN                        | geometria não plana, clusters desiguais, transdutivo                 |
| OPTICS                        | geometria não plana, clusters desiguais com densidade variável, transdutivo |
| Misturas Gaussianas           | geometria plana, indutivo                                            |
| BIRCH                         | grande conjunto de dados com outliers, indutivo                      |

> 🎓 Como criamos clusters tem muito a ver com a forma como agrupamos os pontos de dados. Vamos explorar alguns termos:
>
> 🎓 ['Transdutivo' vs. 'indutivo'](https://wikipedia.org/wiki/Transduction_(machine_learning))
> 
> Inferência transdutiva é derivada de casos de treinamento observados que mapeiam para casos de teste específicos. Inferência indutiva é derivada de casos de treinamento que mapeiam para regras gerais que só então são aplicadas aos casos de teste.
> 
> Um exemplo: Imagine que você tem um conjunto de dados parcialmente rotulado. Alguns itens são 'discos', outros 'CDs', e alguns estão em branco. Sua tarefa é fornecer rótulos para os itens em branco. Se você escolher uma abordagem indutiva, treinaria um modelo procurando por 'discos' e 'CDs', e aplicaria esses rótulos aos dados não rotulados. Essa abordagem terá dificuldade em classificar itens que na verdade são 'cassetes'. Uma abordagem transdutiva, por outro lado, lida com esses dados desconhecidos de forma mais eficaz, agrupando itens semelhantes e aplicando um rótulo ao grupo. Nesse caso, os clusters podem refletir 'coisas musicais redondas' e 'coisas musicais quadradas'.
> 
> 🎓 ['Geometria não plana' vs. 'plana'](https://datascience.stackexchange.com/questions/52260/terminology-flat-geometry-in-the-context-of-clustering)
> 
> Derivado da terminologia matemática, geometria não plana vs. plana refere-se à medida de distâncias entre pontos por métodos geométricos 'planos' ([Euclidianos](https://wikipedia.org/wiki/Geometria_euclidiana)) ou 'não planos' (não Euclidianos).
>
>'Plana' neste contexto refere-se à geometria Euclidiana (partes da qual são ensinadas como 'geometria plana'), e 'não plana' refere-se à geometria não Euclidiana. O que a geometria tem a ver com aprendizagem de máquina? Bem, como dois campos enraizados na matemática, deve haver uma maneira comum de medir distâncias entre pontos em clusters, e isso pode ser feito de forma 'plana' ou 'não plana', dependendo da natureza dos dados. [Distâncias Euclidianas](https://wikipedia.org/wiki/Dist%C3%A2ncia_euclidiana) são medidas como o comprimento de um segmento de linha entre dois pontos. [Distâncias não Euclidianas](https://wikipedia.org/wiki/Geometria_n%C3%A3o_euclidiana) são medidas ao longo de uma curva. Se seus dados, visualizados, parecem não existir em um plano, você pode precisar usar um algoritmo especializado para lidar com eles.
>
![Infográfico Geometria Plana vs. Não Plana](../../../../translated_images/flat-nonflat.d1c8c6e2a96110c1d57fa0b72913f6aab3c245478524d25baf7f4a18efcde224.pt.png)
> Infográfico por [Dasani Madipalli](https://twitter.com/dasani_decoded)
> 
> 🎓 ['Distâncias'](https://web.stanford.edu/class/cs345a/slides/12-clustering.pdf)
> 
> Clusters são definidos por sua matriz de distâncias, ou seja, as distâncias entre pontos. Essa distância pode ser medida de algumas maneiras. Clusters Euclidianos são definidos pela média dos valores dos pontos e contêm um 'centroide' ou ponto central. As distâncias são medidas pela distância até esse centroide. Distâncias não Euclidianas referem-se a 'clustroids', o ponto mais próximo de outros pontos. Clustroids, por sua vez, podem ser definidos de várias maneiras.
> 
> 🎓 ['Restrito'](https://wikipedia.org/wiki/Constrained_clustering)
> 
> [Clusterização Restrita](https://web.cs.ucdavis.edu/~davidson/Publications/ICDMTutorial.pdf) introduz aprendizagem 'semi-supervisionada' neste método não supervisionado. As relações entre pontos são marcadas como 'não pode vincular' ou 'deve vincular', então algumas regras são impostas ao conjunto de dados.
>
>Um exemplo: Se um algoritmo é liberado em um lote de dados não rotulados ou semi-rotulados, os clusters que ele produz podem ser de baixa qualidade. No exemplo acima, os clusters podem agrupar 'coisas musicais redondas', 'coisas musicais quadradas', 'coisas triangulares' e 'biscoitos'. Se forem dadas algumas restrições ou regras para seguir ("o item deve ser feito de plástico", "o item precisa ser capaz de produzir música"), isso pode ajudar a 'restringir' o algoritmo para fazer escolhas melhores.
> 
> 🎓 'Densidade'
> 
> Dados que são 'ruidosos' são considerados 'densos'. As distâncias entre pontos em cada um de seus clusters podem, ao serem examinadas, ser mais ou menos densas, ou 'aglomeradas', e assim esses dados precisam ser analisados com o método de clusterização apropriado. [Este artigo](https://www.kdnuggets.com/2020/02/understanding-density-based-clustering.html) demonstra a diferença entre usar K-Means clustering vs. algoritmos HDBSCAN para explorar um conjunto de dados ruidoso com densidade de cluster desigual.

## Algoritmos de clusterização

Existem mais de 100 algoritmos de clusterização, e seu uso depende da natureza dos dados em questão. Vamos discutir alguns dos principais:

- **Clusterização hierárquica**. Se um objeto é classificado pela sua proximidade a um objeto próximo, em vez de um mais distante, os clusters são formados com base na distância de seus membros para outros objetos. A clusterização aglomerativa do Scikit-learn é hierárquica.

   ![Infográfico Clusterização Hierárquica](../../../../translated_images/hierarchical.bf59403aa43c8c47493bfdf1cc25230f26e45f4e38a3d62e8769cd324129ac15.pt.png)
   > Infográfico por [Dasani Madipalli](https://twitter.com/dasani_decoded)

- **Clusterização por centroide**. Este algoritmo popular exige a escolha de 'k', ou o número de clusters a serem formados, após o qual o algoritmo determina o ponto central de um cluster e reúne dados ao redor desse ponto. [K-means clustering](https://wikipedia.org/wiki/K-means_clustering) é uma versão popular de clusterização por centroide. O centro é determinado pela média mais próxima, daí o nome. A distância quadrada do cluster é minimizada.

   ![Infográfico Clusterização por Centroide](../../../../translated_images/centroid.097fde836cf6c9187d0b2033e9f94441829f9d86f4f0b1604dd4b3d1931aee34.pt.png)
   > Infográfico por [Dasani Madipalli](https://twitter.com/dasani_decoded)

- **Clusterização baseada em distribuição**. Baseada em modelagem estatística, a clusterização baseada em distribuição centra-se em determinar a probabilidade de um ponto de dados pertencer a um cluster e atribuí-lo de acordo. Métodos de mistura Gaussiana pertencem a este tipo.

- **Clusterização baseada em densidade**. Pontos de dados são atribuídos a clusters com base na sua densidade, ou no agrupamento ao redor uns dos outros. Pontos de dados distantes do grupo são considerados outliers ou ruído. DBSCAN, Mean-shift e OPTICS pertencem a este tipo de clusterização.

- **Clusterização baseada em grade**. Para conjuntos de dados multidimensionais, uma grade é criada e os dados são divididos entre as células da grade, criando assim clusters.

## Exercício - agrupe seus dados

A clusterização como técnica é muito auxiliada por uma boa visualização, então vamos começar visualizando nossos dados musicais. Este exercício nos ajudará a decidir qual dos métodos de clusterização devemos usar de forma mais eficaz para a natureza desses dados.

1. Abra o arquivo [_notebook.ipynb_](https://github.com/microsoft/ML-For-Beginners/blob/main/5-Clustering/1-Visualize/notebook.ipynb) nesta pasta.

1. Importe o pacote `Seaborn` para uma boa visualização de dados.

    ```python
    !pip install seaborn
    ```

1. Adicione os dados das músicas a partir de [_nigerian-songs.csv_](https://github.com/microsoft/ML-For-Beginners/blob/main/5-Clustering/data/nigerian-songs.csv). Carregue um dataframe com alguns dados sobre as músicas. Prepare-se para explorar esses dados importando as bibliotecas e exibindo os dados:

    ```python
    import matplotlib.pyplot as plt
    import pandas as pd
    
    df = pd.read_csv("../data/nigerian-songs.csv")
    df.head()
    ```

    Verifique as primeiras linhas de dados:

    |     | nome                     | álbum                        | artista             | género_principal_artista | data_lançamento | duração | popularidade | dançabilidade | acústica | energia | instrumentalidade | vivacidade | volume | discurso | tempo   | assinatura_tempo |
    | --- | ------------------------ | ---------------------------- | ------------------- | ------------------------- | ---------------- | ------- | ------------ | ------------- | -------- | ------- | ------------------ | ---------- | ------ | -------- | ------- | ----------------- |
    | 0   | Sparky                   | Mandy & The Jungle           | Cruel Santino       | r&b alternativo          | 2019             | 144000  | 48           | 0.666         | 0.851    | 0.42    | 0.534              | 0.11       | -6.699 | 0.0829   | 133.015 | 5                 |
    | 1   | shuga rush               | EVERYTHING YOU HEARD IS TRUE | Odunsi (The Engine) | afropop                  | 2020             | 89488   | 30           | 0.71          | 0.0822   | 0.683   | 0.000169           | 0.101      | -5.64  | 0.36     | 129.993 | 3                 |
| 2   | LITT!                    | LITT!                        | AYLØ                | indie r&b        | 2018         | 207758 | 40         | 0.836        | 0.272        | 0.564  | 0.000537         | 0.11     | -7.127   | 0.0424      | 130.005 | 4              |
| 3   | Confident / Feeling Cool | Enjoy Your Life              | Lady Donli          | nigerian pop     | 2019         | 175135 | 14         | 0.894        | 0.798        | 0.611  | 0.000187         | 0.0964   | -4.961   | 0.113       | 111.087 | 4              |
| 4   | wanted you               | rare.                        | Odunsi (The Engine) | afropop          | 2018         | 152049 | 25         | 0.702        | 0.116        | 0.833  | 0.91             | 0.348    | -6.044   | 0.0447      | 105.115 | 4              |

1. Obtenha algumas informações sobre o dataframe, chamando `info()`:

    ```python
    df.info()
    ```

   O resultado será semelhante a:

    ```output
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 530 entries, 0 to 529
    Data columns (total 16 columns):
     #   Column            Non-Null Count  Dtype  
    ---  ------            --------------  -----  
     0   name              530 non-null    object 
     1   album             530 non-null    object 
     2   artist            530 non-null    object 
     3   artist_top_genre  530 non-null    object 
     4   release_date      530 non-null    int64  
     5   length            530 non-null    int64  
     6   popularity        530 non-null    int64  
     7   danceability      530 non-null    float64
     8   acousticness      530 non-null    float64
     9   energy            530 non-null    float64
     10  instrumentalness  530 non-null    float64
     11  liveness          530 non-null    float64
     12  loudness          530 non-null    float64
     13  speechiness       530 non-null    float64
     14  tempo             530 non-null    float64
     15  time_signature    530 non-null    int64  
    dtypes: float64(8), int64(4), object(4)
    memory usage: 66.4+ KB
    ```

1. Verifique novamente se há valores nulos, chamando `isnull()` e verificando se a soma é 0:

    ```python
    df.isnull().sum()
    ```

    Tudo parece bem:

    ```output
    name                0
    album               0
    artist              0
    artist_top_genre    0
    release_date        0
    length              0
    popularity          0
    danceability        0
    acousticness        0
    energy              0
    instrumentalness    0
    liveness            0
    loudness            0
    speechiness         0
    tempo               0
    time_signature      0
    dtype: int64
    ```

1. Descreva os dados:

    ```python
    df.describe()
    ```

    |       | release_date | length      | popularity | danceability | acousticness | energy   | instrumentalness | liveness | loudness  | speechiness | tempo      | time_signature |
    | ----- | ------------ | ----------- | ---------- | ------------ | ------------ | -------- | ---------------- | -------- | --------- | ----------- | ---------- | -------------- |
    | count | 530          | 530         | 530        | 530          | 530          | 530      | 530              | 530      | 530       | 530         | 530        | 530            |
    | mean  | 2015.390566  | 222298.1698 | 17.507547  | 0.741619     | 0.265412     | 0.760623 | 0.016305         | 0.147308 | -4.953011 | 0.130748    | 116.487864 | 3.986792       |
    | std   | 3.131688     | 39696.82226 | 18.992212  | 0.117522     | 0.208342     | 0.148533 | 0.090321         | 0.123588 | 2.464186  | 0.092939    | 23.518601  | 0.333701       |
    | min   | 1998         | 89488       | 0          | 0.255        | 0.000665     | 0.111    | 0                | 0.0283   | -19.362   | 0.0278      | 61.695     | 3              |
    | 25%   | 2014         | 199305      | 0          | 0.681        | 0.089525     | 0.669    | 0                | 0.07565  | -6.29875  | 0.0591      | 102.96125  | 4              |
    | 50%   | 2016         | 218509      | 13         | 0.761        | 0.2205       | 0.7845   | 0.000004         | 0.1035   | -4.5585   | 0.09795     | 112.7145   | 4              |
    | 75%   | 2017         | 242098.5    | 31         | 0.8295       | 0.403        | 0.87575  | 0.000234         | 0.164    | -3.331    | 0.177       | 125.03925  | 4              |
    | max   | 2020         | 511738      | 73         | 0.966        | 0.954        | 0.995    | 0.91             | 0.811    | 0.582     | 0.514       | 206.007    | 5              |

> 🤔 Se estamos a trabalhar com clustering, um método não supervisionado que não requer dados rotulados, por que estamos a mostrar estes dados com rótulos? Na fase de exploração de dados, eles são úteis, mas não são necessários para que os algoritmos de clustering funcionem. Poderíamos simplesmente remover os cabeçalhos das colunas e referir-nos aos dados pelo número da coluna.

Observe os valores gerais dos dados. Note que a popularidade pode ser '0', o que indica músicas que não têm classificação. Vamos remover esses valores em breve.

1. Use um gráfico de barras para descobrir os géneros mais populares:

    ```python
    import seaborn as sns
    
    top = df['artist_top_genre'].value_counts()
    plt.figure(figsize=(10,7))
    sns.barplot(x=top[:5].index,y=top[:5].values)
    plt.xticks(rotation=45)
    plt.title('Top genres',color = 'blue')
    ```

    ![most popular](../../../../translated_images/popular.9c48d84b3386705f98bf44e26e9655bee9eb7c849d73be65195e37895bfedb5d.pt.png)

✅ Se quiser ver mais valores principais, altere o top `[:5]` para um valor maior ou remova-o para ver todos.

Note que, quando o género principal é descrito como 'Missing', isso significa que o Spotify não o classificou, então vamos eliminá-lo.

1. Elimine os dados em falta, filtrando-os:

    ```python
    df = df[df['artist_top_genre'] != 'Missing']
    top = df['artist_top_genre'].value_counts()
    plt.figure(figsize=(10,7))
    sns.barplot(x=top.index,y=top.values)
    plt.xticks(rotation=45)
    plt.title('Top genres',color = 'blue')
    ```

    Agora verifique novamente os géneros:

    ![most popular](../../../../translated_images/all-genres.1d56ef06cefbfcd61183023834ed3cb891a5ee638a3ba5c924b3151bf80208d7.pt.png)

1. De longe, os três géneros principais dominam este conjunto de dados. Vamos concentrar-nos em `afro dancehall`, `afropop` e `nigerian pop`, e adicionalmente filtrar o conjunto de dados para remover qualquer valor de popularidade igual a 0 (o que significa que não foi classificado com uma popularidade no conjunto de dados e pode ser considerado ruído para os nossos propósitos):

    ```python
    df = df[(df['artist_top_genre'] == 'afro dancehall') | (df['artist_top_genre'] == 'afropop') | (df['artist_top_genre'] == 'nigerian pop')]
    df = df[(df['popularity'] > 0)]
    top = df['artist_top_genre'].value_counts()
    plt.figure(figsize=(10,7))
    sns.barplot(x=top.index,y=top.values)
    plt.xticks(rotation=45)
    plt.title('Top genres',color = 'blue')
    ```

1. Faça um teste rápido para ver se os dados têm alguma correlação particularmente forte:

    ```python
    corrmat = df.corr(numeric_only=True)
    f, ax = plt.subplots(figsize=(12, 9))
    sns.heatmap(corrmat, vmax=.8, square=True)
    ```

    ![correlations](../../../../translated_images/correlation.a9356bb798f5eea51f47185968e1ebac5c078c92fce9931e28ccf0d7fab71c2b.pt.png)

    A única correlação forte é entre `energy` e `loudness`, o que não é muito surpreendente, dado que música alta geralmente é bastante energética. Fora isso, as correlações são relativamente fracas. Será interessante ver o que um algoritmo de clustering pode fazer com estes dados.

    > 🎓 Note que correlação não implica causalidade! Temos prova de correlação, mas não prova de causalidade. Um [site divertido](https://tylervigen.com/spurious-correlations) tem alguns visuais que enfatizam este ponto.

Há alguma convergência neste conjunto de dados em torno da popularidade percebida de uma música e sua capacidade de dança? Um FacetGrid mostra que há círculos concêntricos que se alinham, independentemente do género. Poderia ser que os gostos nigerianos convergem a um certo nível de capacidade de dança para este género?  

✅ Experimente diferentes pontos de dados (energia, loudness, speechiness) e mais ou diferentes géneros musicais. O que consegue descobrir? Veja a tabela `df.describe()` para observar a distribuição geral dos pontos de dados.

### Exercício - distribuição de dados

Estes três géneros são significativamente diferentes na perceção da sua capacidade de dança, com base na popularidade?

1. Examine a distribuição de dados dos nossos três géneros principais para popularidade e capacidade de dança ao longo de um eixo x e y dado.

    ```python
    sns.set_theme(style="ticks")
    
    g = sns.jointplot(
        data=df,
        x="popularity", y="danceability", hue="artist_top_genre",
        kind="kde",
    )
    ```

    Pode descobrir círculos concêntricos em torno de um ponto geral de convergência, mostrando a distribuição dos pontos.

    > 🎓 Note que este exemplo usa um gráfico KDE (Kernel Density Estimate) que representa os dados usando uma curva de densidade de probabilidade contínua. Isso permite interpretar os dados ao trabalhar com múltiplas distribuições.

    Em geral, os três géneros alinham-se vagamente em termos de popularidade e capacidade de dança. Determinar clusters nestes dados vagamente alinhados será um desafio:

    ![distribution](../../../../translated_images/distribution.9be11df42356ca958dc8e06e87865e09d77cab78f94fe4fea8a1e6796c64dc4b.pt.png)

1. Crie um gráfico de dispersão:

    ```python
    sns.FacetGrid(df, hue="artist_top_genre", height=5) \
       .map(plt.scatter, "popularity", "danceability") \
       .add_legend()
    ```

    Um gráfico de dispersão dos mesmos eixos mostra um padrão semelhante de convergência.

    ![Facetgrid](../../../../translated_images/facetgrid.9b2e65ce707eba1f983b7cdfed5d952e60f385947afa3011df6e3cc7d200eb5b.pt.png)

Em geral, para clustering, pode usar gráficos de dispersão para mostrar clusters de dados, por isso dominar este tipo de visualização é muito útil. Na próxima lição, vamos pegar neste conjunto de dados filtrado e usar clustering k-means para descobrir grupos nestes dados que parecem sobrepor-se de formas interessantes.

---

## 🚀Desafio

Em preparação para a próxima lição, crie um gráfico sobre os vários algoritmos de clustering que pode descobrir e usar num ambiente de produção. Que tipos de problemas o clustering está a tentar resolver?

## [Questionário pós-aula](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/28/)

## Revisão & Autoestudo

Antes de aplicar algoritmos de clustering, como aprendemos, é uma boa ideia entender a natureza do seu conjunto de dados. Leia mais sobre este tópico [aqui](https://www.kdnuggets.com/2019/10/right-clustering-algorithm.html)

[Este artigo útil](https://www.freecodecamp.org/news/8-clustering-algorithms-in-machine-learning-that-all-data-scientists-should-know/) explica os diferentes comportamentos de vários algoritmos de clustering, dados diferentes formatos de dados.

## Tarefa

[Pesquise outras visualizações para clustering](assignment.md)

---

**Aviso Legal**:  
Este documento foi traduzido utilizando o serviço de tradução por IA [Co-op Translator](https://github.com/Azure/co-op-translator). Embora nos esforcemos para garantir a precisão, é importante ter em conta que traduções automáticas podem conter erros ou imprecisões. O documento original na sua língua nativa deve ser considerado a fonte autoritária. Para informações críticas, recomenda-se a tradução profissional realizada por humanos. Não nos responsabilizamos por quaisquer mal-entendidos ou interpretações incorretas decorrentes da utilização desta tradução.