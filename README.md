# Integração do Azure Machine Learning Classic com o Power BI

## Criando um modelo de machine learning no Azure Machine Learning Studio (Classic)

1. Acesse o [portal do Azure ML Studio (Classic)](https://studio.azureml.net/).
2. Faça seu cadastro com uma conta Microsoft (pode ser seu Hotmail, Outlook, etc, ou domínio organizacional). Utilize a opção de Free Workspace
    1. ![image](https://user-images.githubusercontent.com/43036486/110165074-19d5a200-7dd1-11eb-8a18-7bcb6af4f241.png)
    2. ![image](https://user-images.githubusercontent.com/43036486/110165148-425d9c00-7dd1-11eb-86a6-426c2707be16.png)

3. Acesse o portal para entrar no Studio

5. Crie um dataset para realizar o upload dos seus dados que serão utilizados no treinamento e teste. Esse conjunto de dados precisa seguir um padrão americano: csv são separados por vírgula, separadores decimais são um ponto (.)
![image](https://user-images.githubusercontent.com/43036486/110165736-1abb0380-7dd2-11eb-8a35-d06820da162a.png)

5. Crie um novo experimento e em seguida Blank Experiment, é aqui que você irá fazer a maior parte do trabalho
![image](https://user-images.githubusercontent.com/43036486/110165854-4b9b3880-7dd2-11eb-977e-956341ce1dc5.png)

6. No topo da tela, adicione um nome ao seu experimento. Adicione o dataset que você acabou de adicionar na nuvem clicando em Saved Datasets. Caso não tenha adicionado, também pode usar um dataset de exemplo buscando em Sample Datasets
![image](https://user-images.githubusercontent.com/43036486/110166375-04617780-7dd3-11eb-8c86-77592b25f96d.png)

7. Em Data Transformation, adicione a transformação Split Data no seu experimento. É aqui que você vai separar entre dataset de treinamento e de teste. Aumente a proporção de dados que estarão em treinamento para algo entre 0.7 e 0.8. Conecte a saída (o ponto na parte inferior) do seu dataset à entrada (o ponto na parte superior) da transformação Split Data.
![image](https://user-images.githubusercontent.com/43036486/110166704-7e91fc00-7dd3-11eb-92bd-fa21209752fe.png)

8. Clique em Machine Learning para escolher um modelo pré-definido para treiná-lo com seus dados, caso o conheça bem, você pode experimentar alterar seus parâmetros também
![image](https://user-images.githubusercontent.com/43036486/110166998-e0526600-7dd3-11eb-9f00-d4ad7cda19c7.png)

9. Ainda em Machine Learning, use o módulo Train Model que fica na aba Train. Conecte o seu modelo na entrada esquerda e conecte a saída esquerda (dados de treinamento) do Split Data na entrada da direita de Train Model. Ao clicar em Train Model, você precisará indicar qual coluna possui os dados que você quer tentar prever
    1. ![image](https://user-images.githubusercontent.com/43036486/110167396-79817c80-7dd4-11eb-883f-07e7a79bc117.png)
    2. ![image](https://user-images.githubusercontent.com/43036486/110167599-bc435480-7dd4-11eb-9a9b-411ffad17e85.png)

10. Adicione o módulo Score Model, você vai conectar o seu modelo treinado (a saída de Train Model) na esquerda e o seu dataset de teste na direita. A ideia é tentar prever os valores do dataset de teste. Em seguida, adicione o módulo Evaluate Model, é quem vai dar a "nota" do modelo. Assim você vai saber se funcionou ou não. Basta conectar a saída de Score Model na esquerda dele. Por último, clique em Run para rodar o experimento
![image](https://user-images.githubusercontent.com/43036486/110168248-aa15e600-7dd5-11eb-94d0-7d62fc67864f.png)

11. Depois de rodar o modelo, clique em Setup Web Service (ao lado do botão Run) e em seguida Predictive Web Service [Recommended], para que o Azure crie uma pipeline de inferência. É isso que vai gerar um link (ou ponto de extremidade) que você pode usar passando seus dados para previsões futuras. Quando o Azure criar a pipeline de inferência, basta usar Run para rodar ela e em seguida Deploy Web Service
![image](https://user-images.githubusercontent.com/43036486/110324206-706ef600-7ff4-11eb-993c-590b94099e3d.png)

12. Com seu ponto de extremidade criado, você navega até ele usando o ícone do globo. Em seguida, clique em New Web Services Experience para obter os parâmetros necessários de forma mais direta. Os parâmetros que interessam são a Primary Key e o Request-Response. Copie e cole eles em um bloco de notas.
    1. ![image](https://user-images.githubusercontent.com/43036486/110324858-5255c580-7ff5-11eb-9e2e-6130eaa38dbe.png)
    2. ![image](https://user-images.githubusercontent.com/43036486/110325110-b24c6c00-7ff5-11eb-9891-5faa669460a2.png)
    3. ![image](https://user-images.githubusercontent.com/43036486/110325174-c728ff80-7ff5-11eb-977f-09418dbf7e34.png)


## Preparando os dados no Power BI para realizar previsões

1. Abra o Power BI. No topo, em obter dados, acesse sua fonte de dados com o que precisa prever. O Power BI oferece uma grande variedade de conexões. Para nosso exemplo, utilize a fonte de arquivos de Texto/CSV. Se os dados estiverem ok, clique em Transformar Dados
    1. ![image](https://user-images.githubusercontent.com/43036486/110331787-4e7a7100-7ffe-11eb-80b6-298a7ac2334b.png)
    2. ![image](https://user-images.githubusercontent.com/43036486/110331862-6651f500-7ffe-11eb-92ae-9fc49cc0c666.png)

2. A ferramenta de transformação do Power BI se chama Power Query, ela tem um conjunto de ferramentas apropriadas para manipular dados, que incluem filtragem, agrupamentos, operações matemáticas e de tempo. Assim que abrir, clique no X próximo de Tipo Alterado, na direita da tela. Não utilizaremos os tipos de dados corretos ainda, pois a API de inferência usa dados de tipo Texto e em formato americano.
![image](https://user-images.githubusercontent.com/43036486/110332765-87ffac00-7fff-11eb-9d49-9bc4bc38ced1.png)

3. Como nosso número de previsões é limitado, vamos reduzir o conjunto de dados filtrando apenas um mês. Como nossas datas estão em formato de texto, utilizaremos o filtro de Começa Com. Coloque "2021-03" na caixa de texto, isso filtrará as linhas no mês de março de 2021
![image](https://user-images.githubusercontent.com/43036486/110332947-c8f7c080-7fff-11eb-9d5d-a1323a90452f.png)

4. Cheque se seus dados estão no formato americano, com um ponto(.) delimitando casas decimais. Caso não esteja, clique uma vez no cabeçalho da coluna que precisa corrigir, clique na aba Transformar no topo da tela e, por fim, use Substituir Valores para trocar a , por um .
![image](https://user-images.githubusercontent.com/43036486/110333499-65ba5e00-8000-11eb-9112-da589a1e6189.png)

5. Outra alteração necessária é que as datas precisam estar no formato ano-mês-dia e acrescentados de Thora:minuto:segundoZ, por exemplo: 2021-03-21T00:00:00Z. Isso é obrigatório mesmo que nosso conjunto de dados não use horários. Para isso podemos adicionar o sufixo, clicando na coluna, Transformar, Formato, Adicionar Sufixo. Basta adicionar "T00:00:00Z"
![image](https://user-images.githubusercontent.com/43036486/110335383-8a173a00-8002-11eb-8de8-f7550a00e887.png)


## Fazendo as chamadas para a API de previsão e carregando para o Power BI

1. Vamos adicionar a função de previsão ao Power Query para invocar em cada linha da tabela. Para isso, clique em Nova Fonte e em seguida, Coluna em Branco
![image](https://user-images.githubusercontent.com/43036486/110337239-72d94c00-8004-11eb-923b-089eaaa511a9.png)

2. Mude o nome da consulta para algo mais apropriado como "RealizarPrevisoesAzure" ou algo assim, esse será o nome da função que você invoca para realizar previsões. Em seguida, clique em Editor Avançado no topo da tela e substitua o conteúdo da caixa de código pelo que está abaixo
```
let
    Função = (url as text, api_key as text, linha as record) =>
    let
        json = "{Inputs: {input1: [" & Text.FromBinary(Json.FromValue(linha)) & "]}, GlobalParameters: {}}",
        previsao = Json.Document(Web.Contents(url, [
            Content = Text.ToBinary(json), 
            Headers = [
                Content = "application/json",
                Authorization = "Bearer " & api_key
            ]
        ]))[Results][output1]{0}
    in
        previsao
in
    Função
``` 

![image](https://user-images.githubusercontent.com/43036486/110337637-e713ef80-8004-11eb-8446-78c56d6d3fef.png)

3. Volte a sua consulta original, clique em Adicionar Coluna, Coluna Personalizada e na caixa insira um nome para a coluna que terá os dados das previsões, e abaixo coloque o nome da função de previsão (o mesmo nome da consulta de previsão). Entre parênteses e com aspas duplas, coloque o parâmetro Request-Response do Azure, em seguida o Primary Key, e coloque um terceiro argumento que é apenas um underline, sem aspas
![image](https://user-images.githubusercontent.com/43036486/110341774-7a4f2400-8009-11eb-8aca-70a196fd0d00.png)

4. Sua nova coluna contém todos os dados de resposta da API, que inclui os mesmos dados que você enviou inicialmente. Agora basta clicar no botão do topo da coluna para extrair apenas o que interessa, e marcar a coluna com a previsão de fato
![image](https://user-images.githubusercontent.com/43036486/110348748-bc2f9880-8010-11eb-873a-8bbc3366583d.png)

5. Agora podemos utilizar os tipos de dados corretos, pois só resta carregar os dados para o Power BI. Para isso, selecione todas as colunas (clique no cabeçalho da primeira coluna, segure shift e clique no cabeçalho da última coluna) e na aba Transformar, clique em Detectar Tipos de Dados
![image](https://user-images.githubusercontent.com/43036486/110349080-16305e00-8011-11eb-9975-9c5e88ea8ff0.png)

6. Finalmente, na aba Página Inicial, clique em Fechar e Aplicar para carregar os dados. Salve o arquivo se o Power Query perguntar
![image](https://user-images.githubusercontent.com/43036486/110349215-40821b80-8011-11eb-941b-58aced307bae.png)


Seus dados estão no Power BI e você pode performar análises sobre eles!


 


