# Integração do Azure Machine Learning Classic com o Power BI

## Criando um modelo de machine learning no Azure Machine Learning Studio (Classic)

1. Acesse o [portal do Azure ML Studio (Classic)](https://studio.azureml.net/).
2. Faça seu cadastro com uma conta Microsoft (pode ser seu Hotmail, Outlook, etc, ou domínio organizacional). Utilize a opção de Free Workspace
    1. ![image](https://user-images.githubusercontent.com/43036486/110165074-19d5a200-7dd1-11eb-8a18-7bcb6af4f241.png)
    2. ![image](https://user-images.githubusercontent.com/43036486/110165148-425d9c00-7dd1-11eb-86a6-426c2707be16.png)
3. Acesse o portal para entrar no Studio
4. Crie um dataset para realizar o upload dos seus dados que serão utilizados no treinamento e teste. Esse conjunto de dados precisa seguir um padrão americano: csv são separados por vírgula, separadores decimais são um ponto (.)
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
11. Depois de rodar o modelo, clique em Setup Web Service (ao lado do botão Run) para que o Azure crie uma pipeline de inferência. É o que vai gerar um link (ou ponto de extremidade) que você pode usar passando seus dados para previsões futuras. Quando o Azure criar a pipeline de inferência, basta usar Run para rodar ela e em seguida Deploy Web Service
![image](https://user-images.githubusercontent.com/43036486/110324206-706ef600-7ff4-11eb-993c-590b94099e3d.png)
12. Com seu ponto de extremidade criado, você navega até ele usando o ícone do globo. Em seguida, clique em New Web Services Experience para obter os parâmetros necessários de forma mais direta. Os parâmetros que interessam são a Primary Key e o Request-Response. Copie e cole eles em um bloco de notas.
    1. ![image](https://user-images.githubusercontent.com/43036486/110324858-5255c580-7ff5-11eb-9e2e-6130eaa38dbe.png)
    2. ![image](https://user-images.githubusercontent.com/43036486/110325110-b24c6c00-7ff5-11eb-9891-5faa669460a2.png)
    3. ![image](https://user-images.githubusercontent.com/43036486/110325174-c728ff80-7ff5-11eb-977f-09418dbf7e34.png)
 


