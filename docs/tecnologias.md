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
## Visão Geral e Stack Tecnológico

A escolha das tecnologias foca em alta performance para o backend e integridade de dados.

| Categoria | Tecnologia | Versão Principal | Justificativa |
| :--- | :--- | :--- | :--- |
| **Backend/API** | **Python** + **Django Rest Framework** | 3.10 / 5.x | Framework de desenvolvimento rápido de API (DRF), ideal para a **Camada de Lógica de Negócios**. |
| **Banco de Dados** | **PostgreSQL** | 14 | Escolhido pela integridade transacional de dados e escalabilidade, fundamental para gerenciar dados de `Aluno` e `Questão`. |
| **Infraestrutura** | **Docker** e **Docker Compose** | Compose v3.8 | Garante que o ambiente seja idêntico em qualquer máquina (portabilidade), isolando o BD e o Backend em serviços separados. |


## Fundamentação Estrutural (Diagramas UML)

O diagrama UML anexado `Diagrama de Classes.md` representa o design estrutural da Camada de Lógica de Negócios.

### a) Diagrama de Classes
* **Herança (Polimorfismo):** A superclasse **`Questão`** modela o comportamento comum, enquanto as subclasses (ex: `QuestaoDiscursiva`) herdam e implementam a lógica específica (ex: correção manual).
* **Cardinalidade:** O uso de `1..*` (Um para Muitos) nos relacionamentos (ex: `Matéria` contém `1..* Conteúdo`) define as regras de integridade do sistema.
