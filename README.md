# DIO-IA-Azure
projeto para as atividades do bootcamp Microsoft - Fundamentos de IA

# Descrição de atividade
# Exercício: Criando um Modelo de IA com Azure Machine Learning

Neste exercício, usei a funcionalidade de aprendizado de máquina automatizado do **Azure Machine Learning** para treinar e avaliar um modelo de machine learning. Após isso, fiz a implantação e realizei testes com o modelo treinado.

# Serviços de IA do Azure - Content Safety Studio

Os serviços de IA do Azure oferecem APIs e modelos para criar aplicações de IA. Neste exercício, mostro como usar o **Azure AI Content Safety** para moderar conteúdo de texto e imagens.

---

### Passos Realizados:

# 1. **Acessar o Content Safety Studio**
   - A primeira etapa foi fazer o login no **Content Safety Studio** usando minha conta do Azure.
   - No menu superior, explorei os outros estúdios de IA do Azure, mas continuei com o Content Safety Studio.

# 2. **Criar um Recurso**
   - No Content Safety Studio, fui até as **Configurações** e cliquei na aba **Recurso**.
   - Depois selecionei **Criar um novo recurso**, que me direcionou para o **Azure Portal**.
   - No portal, configurei o recurso com as seguintes opções:
     - **Assinatura**: Minha assinatura do Azure.
     - **Grupo de recursos**: Selecionei ou criei um novo grupo de recursos.
     - **Região**: Escolhi "East US 2" (ou uma região disponível).
     - **Nome**: Dei um nome único para o recurso.
     - **Plano de preços**: Selecionei o plano **F0 (Grátis)**.
   - Revisei as configurações e criei o recurso.

# 3. **Associar o Recurso**
   - Após a criação, voltei ao Content Safety Studio.
   - Nas **Configurações**, verifiquei que o novo recurso estava listado.
   - No **Azure Portal**, atribuí a função **Cognitive Services User** a mim mesmo para garantir que tivesse permissões para usar o recurso.

# 4. **Moderar Conteúdo de Texto**
   - No **Content Safety Studio**, fui até a seção **Realizar testes de moderação** e selecionei **Experimente**.
   - Testei a moderação de texto com amostras como "Safe Content" e "Violent Content", clicando em **Executar teste**.
   - Após a execução, inspecionei os resultados que indicaram o nível de severidade do conteúdo (de **seguro** a **alto**).

# 5. **Verificar Chaves e Endpoints**
   - Para utilizar o recurso em uma aplicação, verifiquei as **chaves e o endpoint** tanto no Content Safety Studio quanto no **Azure Portal**.
   - No portal, encontrei as informações em **Gerenciamento de Recursos** > **Chaves e Endpoints**.

# 6. **Excluir o Recurso**
   - Para reduzir custos, decidi excluir o recurso após o uso.
   - No **Azure Portal**, na página de visão geral do recurso, cliquei em **Excluir**.

---

# 1. Criar um workspace do Azure Machine Learning

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

## Criando um Recurso de Serviços de IA do Azure

Primeiro, eu precisei criar um recurso do **Azure AI Services** na minha assinatura do Azure para começar a usar os serviços de inteligência artificial.

### Passos que segui:
1. Acessei o [portal do Azure](https://portal.azure.com) e fiz login com minha conta Microsoft.
2. Cliquei no botão **＋Criar um recurso** e procurei por **Azure AI services**.
3. Selecionei a opção **Criar um plano de serviços Azure AI**.
4. Completei os campos de configuração com as seguintes informações:
   - **Assinatura**: Escolhi minha assinatura do Azure.
   - **Grupo de Recursos**: Criei um novo grupo de recursos com um nome único.
   - **Região**: Selecionei a região "East US 2", que estava mais próxima de mim.
   - **Nome**: Dei um nome único ao recurso.
   - **Camada de Preço**: Optei pela camada **Standard S0**.
   - Aceitei os termos e condições, marcando a caixa de confirmação.
5. Após revisar tudo, cliquei em **Revisar + Criar** e depois em **Criar**. Esperei a implantação ser concluída.

---

## Conectando o Recurso ao Vision Studio

Com o recurso criado, precisei conectá-lo ao **Vision Studio**, uma plataforma visual baseada em UI que facilita o uso de várias funcionalidades de IA, sem precisar escrever código.

### Passos que segui:
1. Em uma nova aba do navegador, acessei o [Vision Studio](https://portal.vision.cognitive.azure.com) e fiz login com a mesma conta usada para criar o recurso.
2. Na página inicial do Vision Studio, cliquei em **Ver todos os recursos** sob a seção **Introdução ao Vision**.
3. Na lista de recursos, passei o cursor sobre o recurso recém-criado e marquei a caixa ao lado dele. Depois, selecionei **Selecionar como recurso padrão**.
4. Fechei a página de configurações clicando no "x" no canto superior direito da tela.

---

# Detectando Rostos no Vision Studio

Agora, com tudo configurado, fui para a parte de **detecção de rostos** no Vision Studio. Isso me permitiu testar a detecção de rostos em algumas imagens de exemplo fornecidas pelo Azure.

## Passos que segui:
- No **Vision Studio**, cliquei na aba **Face** e selecionei a opção **Detect Faces in an image**.
- Em **Try It Out**, li a política de uso do serviço e marquei a caixa de aceitação.
- Testei as imagens de exemplo fornecidas no portal e observei os dados da detecção de rostos que foram retornados.

## Testando com Minhas Próprias Imagens:
Para explorar mais, decidi testar com algumas imagens minhas. Fui até o link [detect-faces.zip](https://aka.ms/mslearn-detect-faces) e baixei o arquivo.

1. Extraí o conteúdo do arquivo e localizei as seguintes imagens:
   - **store-camera-1.jpg**: Uma imagem de pessoas em uma loja.
   - **store-camera-2.jpg**: Outra imagem com mais pessoas na loja.
   - **store-camera-3.jpg**: Uma imagem de pessoas em uma loja, mas com uma planta parcialmente cobrindo o rosto de uma pessoa.
   
2. Fiz o upload de cada uma dessas imagens no Vision Studio e revisei os detalhes da detecção de rostos retornados. Quando fiz o upload da **store-camera-3.jpg**, percebi que o Azure AI Face não detectou o rosto da pessoa que estava parcialmente obstruída pela planta, o que mostrou como o serviço lida com faces obstruídas.

---

# Azure AI Search - Implementação e Utilização

O **Azure AI Search** é um serviço poderoso que permite a indexação, consulta e análise de grandes volumes de dados, com suporte a consultas cognitivas para extrair informações significativas de documentos e outros tipos de conteúdo.

### Passos Realizados:

## 1. **Criar um Recurso de Pesquisa Cognitiva do Azure**
   - Acesse o portal do Azure em [https://portal.azure.com](https://portal.azure.com).
   - No menu, selecione **+ Criar um recurso** e busque por **Azure Cognitive Search**.
   - Configure as opções do serviço:
     - **Assinatura**: Selecione a sua assinatura do Azure.
     - **Grupo de Recursos**: Crie ou selecione um grupo de recursos existente.
     - **Nome**: Escolha um nome único para o serviço de busca.
     - **Região**: Selecione uma região próxima de você (por exemplo, **East US**).
     - **Plano de Preço**: Escolha o plano adequado para o seu uso (por exemplo, **Basic** ou **Standard**).

   - Após revisar as configurações, clique em **Criar**. Aguarde até a criação ser concluída.

## 2. **Configurar o Recurso de Pesquisa Cognitiva**
   - Após a criação, acesse o **Azure Cognitive Search** através do painel de **Visão Geral**.
   - Copie a **Chave de API** e o **Endpoint**, necessários para a autenticação no serviço.

## 3. **Criar um Índice de Pesquisa**
   - Acesse o **Azure Portal** e selecione **Cognitive Search**.
   - No painel esquerdo, clique em **Índices** e depois em **+ Adicionar índice**.
   - Defina o nome do índice e os campos que ele deve indexar (por exemplo, se você estiver indexando documentos, você pode definir campos como **id**, **nome**, **conteúdo**).
   - Escolha os tipos de dados apropriados para cada campo (string, booleano, numérico, etc.).

### Exemplo de definição de índice:

```json
{
  "name": "documentos-index",
  "fields": [
    { "name": "id", "type": "Edm.String", "key": true },
    { "name": "nome", "type": "Edm.String" },
    { "name": "conteudo", "type": "Edm.String", "searchable": true },
    { "name": "data_criacao", "type": "Edm.DateTimeOffset" }
  ]
}
```

markdown
Copiar código
# Azure AI Search - Implementação e Utilização

O **Azure AI Search** é um serviço poderoso que permite a indexação, consulta e análise de grandes volumes de dados, com suporte a consultas cognitivas para extrair informações significativas de documentos e outros tipos de conteúdo.

### Passos Realizados:

## 1. **Criar um Recurso de Pesquisa Cognitiva do Azure**
   - Acesse o portal do Azure em [https://portal.azure.com](https://portal.azure.com).
   - No menu, selecione **+ Criar um recurso** e busque por **Azure Cognitive Search**.
   - Configure as opções do serviço:
     - **Assinatura**: Selecione a sua assinatura do Azure.
     - **Grupo de Recursos**: Crie ou selecione um grupo de recursos existente.
     - **Nome**: Escolha um nome único para o serviço de busca.
     - **Região**: Selecione uma região próxima de você (por exemplo, **East US**).
     - **Plano de Preço**: Escolha o plano adequado para o seu uso (por exemplo, **Basic** ou **Standard**).

   - Após revisar as configurações, clique em **Criar**. Aguarde até a criação ser concluída.

## 2. **Configurar o Recurso de Pesquisa Cognitiva**
   - Após a criação, acesse o **Azure Cognitive Search** através do painel de **Visão Geral**.
   - Copie a **Chave de API** e o **Endpoint**, necessários para a autenticação no serviço.

## 3. **Criar um Índice de Pesquisa**
   - Acesse o **Azure Portal** e selecione **Cognitive Search**.
   - No painel esquerdo, clique em **Índices** e depois em **+ Adicionar índice**.
   - Defina o nome do índice e os campos que ele deve indexar (por exemplo, se você estiver indexando documentos, você pode definir campos como **id**, **nome**, **conteúdo**).
   - Escolha os tipos de dados apropriados para cada campo (string, booleano, numérico, etc.).

### Exemplo de definição de índice:

```json
{
  "name": "documentos-index",
  "fields": [
    { "name": "id", "type": "Edm.String", "key": true },
    { "name": "nome", "type": "Edm.String" },
    { "name": "conteudo", "type": "Edm.String", "searchable": true },
    { "name": "data_criacao", "type": "Edm.DateTimeOffset" }
  ]
}
```
4. Carregar Dados no Índice
Você pode carregar dados para o índice usando o Azure SDK, APIs REST ou usando o Azure Data Factory.
Utilize o Azure Portal ou a API para adicionar documentos ao índice, como mostrado no exemplo a seguir:
Exemplo de adicionar documentos:

```json

{
  "value": [
    {
      "@search.action": "upload",
      "id": "1",
      "nome": "Documento Exemplo",
      "conteudo": "Este é um exemplo de conteúdo de documento.",
      "data_criacao": "2024-12-23T12:00:00Z"
    }
  ]
}
```
1. Consultar Dados no Índice
Após os dados serem carregados, você pode realizar consultas usando a API de pesquisa.
A consulta pode incluir filtragem, ordenação e extração de informações com base em palavras-chave.
Exemplo de consulta de pesquisa:
```json
{
  "queryType": "full",
  "search": "exemplo",
  "select": "id, nome, conteudo",
  "top": 10
}
```
A consulta acima retorna os 10 primeiros documentos que correspondem à palavra "exemplo" nos campos indexados.

# Limpeza de Recursos

Depois de realizar os testes, fiz a limpeza dos recursos para evitar custos desnecessários. Isso é sempre importante quando você está concluindo seus exercícios e não precisa mais dos recursos criados.

## Passos que segui:
- No [portal do Azure](https://portal.azure.com), acessei o grupo de recursos onde o recurso foi criado.
- Selecionei o recurso criado e cliquei em **Excluir**. Depois, confirmei a exclusão clicando em **Sim**.

---

