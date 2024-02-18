# Trabalhando com Machine Learning na Prática no Azure ML
Repositório para entrega do desafio 
# Passo a passo
1. Criar uma conta Azure
2. Criar um recurso com o Azure Machine Learning
   - Preencher os campos de subscrição, criando um Grupo de Recursos se não tiver um
   - Preencher o campo **Nome**
   - Preencher a **Região**, a East US tem recursos mais baratos e é mais próxima do Brasil
   - **Armazenamento**, **Registro de Contêiner**, **Key Vault** e **Registro de Contêiner** pode manter como está
   - Click em **Review+create**, revise os dados e espere a validação, click e **create**
   - Espere a implementação ser concluída
   - Click no botão **Ir para recurso**
   - Click no botão **Iniciar o estúdio**
3. Criar um Trabalho
   - No menu da esquerda vá para **ML automatizado**
   - Click em **Novo trabalho de ML automatizado**
   - Preencher **Nome do trabalho** como *mslearn-bike-automl*
   - Preencher **Novo nome do experimento** como *mslearn-bike-rental*
   - Preencher a **Descrição** como *Aprendizado de máquina automatizado para previsão de aluguel de bicicletas*
   - **Marcas** nenhum
   - Avançar
   - Selecionar **Tipo de tarefa** como Regressão
   - Selecionar os dados click em +criar
   - Preencher **Nome de ativos de dados** como *alugueldebicicletas*
   - Preencher **Descrição** como *Dados históricos de aluguel de bicicletas*
   - **Tipo** Tabular
   - **Fontes de dados** selecionar o De arquivos da Web
   - Avançar
   - **URL da Web** *https://aka.ms/bike-rentals*
   - Avançar
   - **Formato do arquivo** Delimitado
   - **Delimitador** Vírgula
   - **Codificação** UTF-8
   - **Cabeçalhos de Coluna** Somente o primeiro arquivo tem cabeçalho
   - **Ignorar linhas** Nenhuma
   - Não selecionar o checkbox
   - Avançar
   - Avançar
   - Criar
   - Selecionar *alugueldebicicletas*
   - Avançar
   - **Coluna de destino** rentals(Integer)
   - **Exibir definições de configuração adicionais**
     - **Métrica primária** NormalizedRootMeanSqueredError
     - Não selecionar nenhum checkbox
     - **Modelos permitidos** selecionar RadomForest e o LightGBM
     - Salvar
   - **Limites**
     - **Máximo de avaliação** 3
     - **Máximo de avaliações simultâneas** 3
     - **Máximo de nós** 3
     - **Limite de pontuação da métrica** 0,085
     - **Tempo limite do experimento (minutos)** 15
     - **Tempo limite de iteração (minutos)** 15
     - Selecionar o checkbox **Habilitar encerramento antecipado**
   - **Tipo de validação** Divisão de validação de treinamento
     - **Validação de percentual de dados** 10
   - **Dados de teste** Nenhum
   - Avançar
   - Selecione o **Tipo de computação** Sem servidor
   - **Tipo de máquina virtual** CPU
   - **Tipo de máquina virtual** Dedicado
   - **Tamanho da máquina virtual** *Standard_DS3_v2*
   - **Número de instancias** 1
   - Avançar
   - Enviar trabalho de treinamento
   - Click no nome de exibição
   - Click em +Modelo de registro
4. Criar um Modelo de registro
   - **Tipo de modelo** MLflow
   - **Saída do trabalho** manter
   - Avançar
   - **Nome** *mslearnbikeauto2*
   - **Descrição** Nenhuma
   - **Versão** 1
   - **Marcas** Nenhuma
   - Avançar
   - Registro
   - No menu da esquerda entre em **Modelos**
   - Click no nome *mslearnbikeauto2*
   - Click em **Implantar** Opção Implante o modelo usando o assistente do ponto de extremidade em tempo real
5. Criar Ponto de extremidade
   - **Máquina virtual** Standard_DS1_v2
   - **Contagem de instância** 1
   - **Ponto de extremidade** Novo
   - **Nome do ponto de extremidade** *prever-aluguel*
   - **Nome da implantação** *mslearnbikeauto2-1*
   - **Inferencing data collection** Desabilitado
   - **Package Model** Desabilitado
   - Implantar
6. Teste
   - No menu da esquerda entre em Pontos de extremidade
   - Click no nome *prever-aluguel*
   - Click em Testar
   - Insira o json abaixo
    ```
    {
      "input_data": {
         "data": [
           {
            "day": 1,
            "mnth": 1,
            "year": 2022,
            "season": 2,
            "holiday": 0,
            "weekday": 1,
            "workingday": 1,
            "weathersit": 2,
            "temp": 0.3,
            "atemp": 0.3,
            "hum": 0.3,
            "windspeed": 0.3
          }
        ]
      }
    }
    ```
   - Click em Testar
   - O resultado do teste foi 346.1    
