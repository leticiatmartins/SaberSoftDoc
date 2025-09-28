# 📐 Desenho da Arquitetura do Sistema

A **Plataforma de Estudos** utiliza uma arquitetura baseada em serviços separados e containerização, seguindo o padrão **3-Tier (Três Camadas)** para garantir escalabilidade, manutenção e portabilidade do código.

## 📂 Estrutura de Diretórios do Projeto

A organização do projeto segue a modularização padrão do Django, com clara separação entre a infraestrutura de ambiente (Docker) e a lógica da aplicação.

```bash
.
├── .gitignore             # Arquivos ignorados pelo Git (ex: cache, venvs)
├── ARQUITETURA.md         # Documento com o Desenho da Arquitetura (o que montamos)
├── Dockerfile             # Instruções para construir a imagem do Backend (Python/Django)
├── docker-compose.yml     # Define os serviços 'backend' e 'db' (PostgreSQL com volume)
├── manage.py              # Script principal de execução do Django
├── requirements.txt       # Lista de dependências (Django, djangorestframework, psycopg2)
└── plataforma_estudos/    # Pasta Principal de Configuração do Django
    ├── __init__.py
    ├── asgi.py
    ├── settings.py      # Configurações do projeto (IMPORTANTE: Conexão PostgreSQL)
    ├── urls.py          # Rotas principais da API (inclui as rotas do app 'aluno')
    └── wsgi.py
└── aluno/                 # Aplicação Django: Foco na 1ª História de Usuário (Cadastro)
    ├── __init__.py
    ├── admin.py
    ├── models.py          # O Modelo Aluno (Mapeia a tabela do BD)
    ├── serializers.py     # Lógica de validação e criação do User/Aluno
    ├── urls.py            # Rotas específicas do Aluno (Cadastro/Listagem)
    ├── views.py           # Lógica da API (Views de Cadastro e Listagem)
    └── tests.py           # Arquivo de Testes (CRUD C, R - OK!)
```
## 1. Visão Geral e Stack Tecnológico

A escolha das tecnologias foca em alta performance para o backend e integridade de dados.

| Categoria | Tecnologia | Versão Principal | Justificativa |
| :--- | :--- | :--- | :--- |
| **Backend/API** | **Python** + **Django Rest Framework** | 3.10 / 5.x | Framework de desenvolvimento rápido de API (DRF), ideal para a **Camada de Lógica de Negócios**. |
| **Banco de Dados** | **PostgreSQL** | 14 | Escolhido pela integridade transacional de dados e escalabilidade, fundamental para gerenciar dados de `Aluno` e `Questão`. |
| **Infraestrutura** | **Docker** e **Docker Compose** | Compose v3.8 | Garante que o ambiente seja idêntico em qualquer máquina (portabilidade), isolando o BD e o Backend em serviços separados. |



## 2. Modelo de Arquitetura Lógica (3-Tier)

A arquitetura lógica do projeto é dividida em três camadas distintas, garantindo separação de responsabilidades:

### A. Camada de Apresentação (Cliente)
* **Função:** Envia requisições HTTP e lida com a interface do usuário (UI).
* **Exemplo:** O protótipo de alta fidelidade que fará um **`POST`** para a API de cadastro.

### B. Camada de Lógica de Negócios (Backend/API)
* **Função:** Recebe as requisições, valida os dados via **Serializers** e executa as regras de negócio definidas nas **Views**.
* **Exemplo:** O endpoint `/api/v1/aluno/cadastro/` recebe os dados, valida-os e chama o comando para criar o `User` e o `Aluno` no banco de dados.

### C. Camada de Dados (Persistência)
* **Função:** Armazena o estado do sistema. Controlada pelo **PostgreSQL**, que garante que os dados dos alunos e progressos sejam duráveis e íntegros.


## 3. Visão de Implantação e Infraestrutura (Docker)

O ambiente de desenvolvimento é totalmente containerizado, conforme definido no `docker-compose.yml`.

### a) Isolamento de Serviço

| Serviço | Componente | Comunicação e Mapeamento |
| :--- | :--- | :--- |
| **`backend`** | Container Python/Django | Acessível externamente via porta **`8001`**. Comunica-se com o BD usando o *hostname* **`db`**. |
| **`db`** | Container PostgreSQL 14 | Acessível apenas internamente pelo `backend`. Armazena os dados do projeto. |

### b) Persistência de Dados
O requisito de **Ambiente BD Dockerizado com Volume** é atendido pelo **Volume `db_data`**, que é montado no contêiner do PostgreSQL.

* **Comprovação:** O volume garante que as tabelas (`auth_user`, `aluno_aluno`) e os dados registrados (**ex: o cadastro bem-sucedido**) sobrevivam mesmo se o contêiner `estudos_db` for removido ou atualizado.


## 4. Fundamentação Estrutural (Diagramas UML)

O diagrama UML anexado `Diagrama de Classes.md` representa o design estrutural da Camada de Lógica de Negócios.

### a) Diagrama de Classes
* **Herança (Polimorfismo):** A superclasse **`Questão`** modela o comportamento comum, enquanto as subclasses (ex: `QuestaoDiscursiva`) herdam e implementam a lógica específica (ex: correção manual).
* **Cardinalidade:** O uso de `1..*` (Um para Muitos) nos relacionamentos (ex: `Matéria` contém `1..* Conteúdo`) define as regras de integridade do sistema.


### **Comprovação de Funcionalidade:**
A validade da arquitetura é comprovada pela execução do teste automatizado (`aluno/tests.py`), que retorna **`Ran 2 tests... OK`**, atestando que a criação e leitura de dados no ambiente containerizado funcionam como esperado.