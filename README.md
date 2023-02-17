# Satisfação do Cliente, Santander
## Introdução
O cliente é a parte mais importante de qualquer negócio, a base de qualquer empresa, nenhum negócio existe sem clientes, para manter um bom relacionamento com eles é importante que estejam satisfeitos com os serviços prestados, por isso é necessário identificar clientes insatisfeitos e recuperá-los.
O Santander é o terceiro maior banco privado do setor financeiro do Brasil, e um de seus valores é a **orientaçao ao cliente**, para impedir o churn deles, o banco utiliza da inteligência de dados para prever quem são os clientes mais prováveis a deixar o serviço, daí surge a motivação do desenvolvimento desse projeto, **construir e otimizar um estimador** capaz de identificar com precisão clientes insatisfeitos.
## Sobre o conjunto de dados
Os dados utilizados foram fornecidos pelo próprio Santander a algum tempo atrás para uma competição o Kaggle, você pode acessar a página da competição [aqui](https://www.kaggle.com/competitions/santander-customer-satisfaction/overview), os dados não precisaram de muitos tratamentos pois não havia dados nulos.

![image](https://user-images.githubusercontent.com/115597735/219783250-544db413-d541-4fd4-9a2f-e4069fdd9a36.png)

Devido ao parâmetro inferSchema do spark.read.csv a maioria das colunas já veio no formato correto, com execção do ID, que ficou no formato inteiro e tive que converter para string, por ser a chave primária.

![image](https://user-images.githubusercontent.com/115597735/219783711-a531ed4b-bffb-4bc9-b027-8787bef3b785.png)

## Preparando dados para o modelo
Nessa fase identifiquei que os dados estavam desbalanceados, o desbalanceamento é quando a quantidade de elementos é muito maior que a outra, isso acarretá em um modelo com viés, que aprende melhor sobre a classe majoritária.

![image](https://user-images.githubusercontent.com/115597735/219785342-c35de198-da6a-4452-9898-ee994e51fb0e.png)
![image](https://user-images.githubusercontent.com/115597735/219784238-32a42624-5bc6-4642-b1c3-75cc53286a4e.png)

Existem algumas formas de lidar com dados desbalanceados, sendo elas: **oversampling**, é um técnica onde são criados outros elementos parecidos com os elementos da classe minoritária, até a classe minoritária ser igualada com a majoritária, o **undersampling** é a remoção de elementos da classe majoritária até atingir o balanço, no primeiro caso, criar elementos proporcionais em grandes conjuntos de dados é inviável, e remover elementos gera a perda de informação, por isso usei o **ClassWeight** uma forma de dar mais importância ou peso para a classe minoritária.

Os modelos do spark pedem uma coluna com os valores do peso, para calcular o peso usei a formula: n_amostras/(n_classes*elementos_da_classe)

![image](https://user-images.githubusercontent.com/115597735/219788642-20f7803b-f4c4-469d-a07b-8bc280ea2b93.png)

Depois fiz uma seleção usando funções para criar a coluna 'ColWeight':

![image](https://user-images.githubusercontent.com/115597735/219788791-02ca5fc4-1c03-42f9-a835-60aa0d094684.png)
## Escolha do modelo, Validação Cruzada
Testei o logisticRegression, DecissionTreeClassifier e o RandomForestClassifier, olhando as métricas de acurácia, F1-score, precisão, recall e matrix de confusão o que melhor desempenhou na matrix de confusão foi o LogisticRegression, então fazer a validação cruzada com este modelo, o processamento levou 20 minutos, e as melhores configurações para o LogisticRegression foram:

![image](https://user-images.githubusercontent.com/115597735/219816154-7f8b454c-17a5-4714-beb0-6ac5f4d48c2f.png)

Após criar o modelo final usando o LogisticRegression com esses parâmetros usei o conjunto de dados de teste para prever quais clientes provavelmente estão insatisfeitos.


## Conclusão
A partir dos resultados obtidos pelo modelo, oferer outros tipos de produtos financeiros especificamente para os clientes insatisfeitos, afim de reverter a situação.
