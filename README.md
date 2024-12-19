# DIO-IA-Azure
projeto para as atividades do bootcamp Microsoft - Fundamentos de IA

# Descrição de atividade
# Exercício: Criando um Modelo de IA com Azure Machine Learning

Neste exercício, usei a funcionalidade de aprendizado de máquina automatizado do **Azure Machine Learning** para treinar e avaliar um modelo de machine learning. Após isso, fiz a implantação e realizei testes com o modelo treinado.

## 1. Criar um workspace do Azure Machine Learning

1. Acessei o portal do Azure [https://portal.azure.com](https://portal.azure.com) e me loguei com minhas credenciais da Microsoft.
2. No menu do portal, selecionei **+ Criar um recurso**, busquei por "Machine Learning" e criei um novo recurso de **Azure Machine Learning** com as seguintes configurações:
    - **Assinatura**: Selecionei a minha assinatura Azure.
    - **Grupo de Recursos**: Criei ou selecionei um grupo de recursos.
    - **Nome**: Escolhi um nome único para o meu workspace.
    - **Região**: Selecionei a região **East US**.
    - **Armazenamento**: O armazenamento foi criado automaticamente.
    - **Key Vault**: O Key Vault foi criado automaticamente.
    - **Application Insights**: Foi criado automaticamente.
    - **Container Registry**: Nenhum foi selecionado, pois seria criado automaticamente ao implantar o modelo.

3. Cliquei em **Revisar + Criar** e depois em **Criar**. Esperei a criação do workspace, o que levou alguns minutos.

4. Após a criação, fui até o painel **Visão Geral** e selecionei o **Grupo de Recursos**. No menu esquerdo, selecionei **Controle de Acesso (IAM)**.

5. Atualizei as permissões de acesso:
    - Selecionei **Adicionar atribuição de função** e busquei por `Microsoft.MachineLearningServices/workspaces/datastores/listsecrets/action`.
    - Selecionei a função **Azure AI Administrator** e prossegui com a atribuição.
    - Adicionei meu email associado à assinatura da Azure e confirmei a atribuição.

6. Retornei ao painel **Visão Geral** do grupo de recursos e selecionei o recurso **Azure Machine Learning**.

## 2. Iniciar o Azure Machine Learning Studio

1. No recurso do Azure Machine Learning, selecionei **Lançar Studio** (ou abri uma nova guia do navegador e fui para [https://ml.azure.com](https://ml.azure.com)).
2. No **Azure Machine Learning Studio**, selecionei o workspace recém-criado (se não apareceu, fui em **Todos os Workspaces** e selecionei o workspace correto).

## 3. Treinar um modelo com aprendizado de máquina automatizado

1. No **Azure Machine Learning Studio**, fui até a página **Automated ML** (sob a aba **Authoring**).
2. Criei um novo trabalho de **Automated ML** com as seguintes configurações:
    - **Nome do trabalho**: O nome foi preenchido automaticamente com um nome único.
    - **Nome do experimento**: `mslearn-bike-rental`.
    - **Descrição**: "Automated machine learning for bike rental prediction".
    - **Tipo de tarefa**: Selecionei **Regressão**.
    - **Dataset**:
        - **Nome**: `bike-rentals`.
        - **Descrição**: Dados históricos de aluguel de bicicletas.
        - **Fonte de dados**: Selecionei **Do arquivo local** e fiz o upload dos dados extraídos do link [https://aka.ms/bike-rentals](https://aka.ms/bike-rentals).
        - **Armazenamento**: Selecionei **Azure Blob Storage**.

3. Defini as configurações do trabalho:
    - **Coluna alvo**: `rentals` (número de aluguéis).
    - **Métrica primária**: **NormalizedRootMeanSquaredError**.
    - **Modelos permitidos**: Selecionei **RandomForest** e **LightGBM**.
    - **Limites**:
        - **Máximo de tentativas**: 3.
        - **Máximo de tentativas simultâneas**: 3.
        - **Máximo de nós**: 3.
        - **Limite de pontuação da métrica**: 0.085.
    - **Computação**:
        - **Tipo de computação**: **Serverless**.
        - **Máquina virtual**: **Standard_DS3_V2**.
        - **Número de instâncias**: 1.

4. Submeti o trabalho, que iniciou automaticamente e levou algum tempo para ser concluído.

## 4. Revisar o melhor modelo

1. Quando o trabalho de aprendizado automatizado foi concluído, fui à aba **Visão Geral** e vi o resumo do melhor modelo treinado.
2. Cliquei no nome do algoritmo do melhor modelo para ver mais detalhes.
3. Na aba **Métricas**, analisei os gráficos de **residuais** e **previsão versus verdade** para entender o desempenho do modelo.

## 5. Implantar e testar o modelo

1. No **Azure Machine Learning Studio**, selecionei o melhor modelo treinado e cliquei em **Implantar**.
2. Escolhi a opção de **endpoint em tempo real** com as configurações:
    - **Máquina virtual**: **Standard_DS3_v2**.
    - **Número de instâncias**: 3.
    - **Nome do endpoint**: Deixei o nome padrão ou criei um nome único.
    - **Coleta de dados de inferência**: Desabilitada.
3. Esperei até que o status da implantação mudasse para **Concluído**, o que levou cerca de 5-10 minutos.

## 6. Testar o serviço implantado

1. No **Azure Machine Learning Studio**, fui até a aba **Endpoints** e selecionei o endpoint **predict-rentals**.
2. Na aba **Testar**, substituí o JSON de entrada pelo seguinte:

``` json
{
  "input_data": {
    "columns": [
      "day", "mnth", "year", "season", "holiday", "weekday", "workingday", "weathersit", "temp", "atemp", "hum", "windspeed"
    ],
    "index": [0],
    "data": [[1,1,2022,2,0,1,1,2,0.3,0.3,0.3,0.3]]
  }
}
```

- Cliquei em Testar e vi o número previsto de aluguéis como resultado:
```json
[352.3564674945718]
```
## 7. Limpeza de recursos
Após concluir os testes, excluí o endpoint para evitar cobranças adicionais. Em Azure Machine Learning Studio, fui até Endpoints, selecionei o endpoint predict-rentals e cliquei em Excluir.
Para excluir o workspace e os recursos associados, no portal do Azure, fui até Grupos de Recursos, selecionei o grupo de recursos do meu workspace e cliquei em Excluir.

