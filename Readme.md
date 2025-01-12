

- [1. docker-compose-configs](#1-docker-compose-configs)
  - [1.1. Objetivo](#11-objetivo)
  - [1.2. Estrutura do Repositório](#12-estrutura-do-repositório)
  - [1.3. Construção do Ambiente Exploratório](#13-construção-do-ambiente-exploratório)
    - [1.3.1. Docker Compose](#131-docker-compose)
      - [1.3.1.1. Iniciando o container e customizando](#1311-iniciando-o-container-e-customizando)
      - [1.3.1.2. Parar o container do jeito que está:](#1312-parar-o-container-do-jeito-que-está)
    - [1.3.2. Configurando o Poetry](#132-configurando-o-poetry)
      - [1.3.2.1. pyproject.toml Não definido:](#1321-pyprojecttoml-não-definido)
      - [1.3.2.2. pyproject.toml já definido:](#1322-pyprojecttoml-já-definido)
    - [1.3.3. Passo a Passo para Obter o Token do Jupyter e Usar no VSCode](#133-passo-a-passo-para-obter-o-token-do-jupyter-e-usar-no-vscode)
      - [1.3.3.1. Use o link e token](#1331-use-o-link-e-token)
  - [1.4. Estrutura do Projeto](#14-estrutura-do-projeto)
    - [1.4.1. Principais notebooks](#141-principais-notebooks)

# 1. docker-compose-configs

Este repositório é dedicado a armazenar as configurações e informações relacionadas aos meus scripts padrão para a construção e orquestração de containers `Docker` usando `Docker Compose`. O objetivo deste repositório é centralizar os arquivos de configuração que facilitam a criação de ambientes de desenvolvimento, homologação e produção baseados em containers.

## 1.1. Objetivo

O repositório contém templates, exemplos e ajustes personalizados para configuração de containers e orquestração utilizando o Docker Compose. A ideia é fornecer uma estrutura reutilizável para qualquer projeto, permitindo facilmente iniciar e gerenciar containers, como bancos de dados, serviços de backend, e aplicações web.

## 1.2. Estrutura do Repositório

- `docker-compose.yml`: Arquivo principal que contém a definição dos serviços, volumes, redes e configurações necessárias para rodar os containers.
- `config/`: Diretório com arquivos de configuração adicionais, como variáveis de ambiente e scripts personalizados para containers.
- `scripts/`: Scripts auxiliares que podem ser usados para automatizar a criação e manutenção dos containers.


## 1.3. Construção do Ambiente Exploratório


### 1.3.1. Docker Compose

Essa padronização não usa o Dockerfile, para verificar como seria com dockerfile vide: `link`

Para o passo a passo a seguir é necessário ter na nessa pasta os arquivos  [docker-compose up](docker-compose.yml) e o arquivo [customizations.sh](/scripts/customizations.sh).

- `docker-compose.yml`: Baixar e gerencia os serviços e imagens
- `customizations.sh`: Realizar as principais customizações, como por exemplo a instalação do poetry, e instalação do **driver JDBC do PostgreSQL**.
  
#### 1.3.1.1. Iniciando o container e customizando

1. **Iniciar o Docker Compose:** `docker-compose up`
2. **Verificar os containers em execução:** `docker ps`
3. **Acessar o bash do container:** `docker exec -it <container> bash`

    ```bash
    # Após pegar o nome do container com docker ps
    docker exec -it pyspark_with_customizations bash
    ```
4. **Instalar as customizações:** `sh customizations.sh`
5. **Definir caminho para o poetry funcionar:** `export PATH="$HOME/.local/bin:$PATH"`

#### 1.3.1.2. Parar o container do jeito que está:
1. **Parar todos os containers\serviços já definidos no arquivo `docker-compose.yml`:** `docker-compose stop`

2. **Iniciar os container\serviços já existentes definidos no arquivo `docker-compose.yml`:** `docker-compose start`


### 1.3.2. Configurando o Poetry

#### 1.3.2.1. pyproject.toml Não definido:
Caso o arquivo **`pyproject.toml`** não esteja definido, siga os passos:
1. **Iniciar o poetry:** `poetry init`
2. **Necessário instalar:** `poetry add ipykernel`
3. **Instalar dependências com Poetry:** `poetry install --no-root`, esse comando dessativa o empacotamento do projeto.

#### 1.3.2.2. pyproject.toml já definido:
vá direto para o comando

1. **Instalar dependências com Poetry:** `poetry install --no-root`, esse comando dessativa o empacotamento do projeto.
Em meus projetos geralmente já inicio com as principais  no arquivo **`pyproject.toml`**:
- python = "^3.10"
- ipykernel = "^6.28.0"

2. **Configurar o Kernel do Jupyter:** ```python -m ipykernel install --user --name=<container> --display-name "Python (nome_que_desejar)"```

    ```bash
    # Use esse para uma organização melhor
    python -m ipykernel install --user --name=ambiente_exploratorio --display-name "minha_venv"
    ```

### 1.3.3. Passo a Passo para Obter o Token do Jupyter e Usar no VSCode

#### 1.3.3.1. Use o link e token

- Use o link com token completo fornecido pelo docker, basta copiar o link com o token (**`http://127.0.0.1:9090/lab/?token=abc123def456`**) e colar na caixa `Existent Jupyter Server`.

- Clique em `Select Kernel` > `Select Another Kernel` > `Existent Jupyter Server`.

- O link comentado é o mesmo link utilizado para acessar o jupyter notebook via navegador

- Pode ser acesso também com os comandos:
  
    ```bash
    docker-compose logs <service_name>
    docker-compose logs
    docker logs <container_id>
    ```

## 1.4. Estrutura do Projeto

```bash
.
├── config                      
│   ├── main.yaml                   # Main configuration file
│   ├── model                       # Configurations for training model
│   │   ├── model1.yaml             # First variation of parameters to train model
│   │   └── model2.yaml             # Second variation of parameters to train model
│   └── process                     # Configurations for processing data
│       ├── process1.yaml           # First variation of parameters to process data
│       └── process2.yaml           # Second variation of parameters to process data
├── data            
│   ├── final                       # data after training the model
│   ├── processed                   # data after processing
│   └── raw                         # raw data
├── docs                            # documentation for your project
├── .gitignore                      # ignore files that cannot commit to Git
├── Makefile                        # store useful commands to set up the environment
├── models                          # store models
├── notebooks                       # store notebooks
│   ├── exploration
│   │   └── .gitkeep
│   ├── modeling
│   │   └── .gitkeep
│   ├── preprocessing
│   │   └── .gitkeep
│   └── reporting
│       └── .gitkeep
├── output                          # store outputs
│   ├── figures
│   │   └── .gitkeep
│   ├── predictions
│   │   └── .gitkeep
│   └── reports
│       └── .gitkeep
├── .pre-commit-config.yaml         # configurations for pre-commit
├── pyproject.toml                  # dependencies for poetry
├── README.md                       # describe your project
├── src                             # store source code
│   ├── __init__.py                 # make src a Python module 
│   ├── process.py                  # process data before training model
│   ├── train_model.py              # train model
│   └── utils.py                    # store helper functions
└── tests                           # store tests
    ├── __init__.py                 # make tests a Python module 
    ├── test_process.py             # test functions for process.py
    └── test_train_model.py         # test functions for train_model.py
```

### 1.4.1. Principais notebooks
