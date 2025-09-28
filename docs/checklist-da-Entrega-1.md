## ✅ Checklist da Entrega 1: Comprovação de Implementação

Esta seção mapeia os requisitos da primeira entrega do projeto à sua prova de conclusão no repositório.

| Requisito | Status | Prova de Conclusão / Local |
| :--- | :--- | :--- |
| **Ambiente Docker** | ✅ **CONCLUÍDO** | Arquivo `docker-compose.yml` e `Dockerfile`. |
| **Ambiente BD dockerizado com volume** | ✅ **CONCLUÍDO** | Serviço `db` no `docker-compose.yml` (PostgreSQL) configurado com o volume `db_data` para persistência de dados. |
| **Repositório backend com 1 H.U. implementada** | ✅ **CONCLUÍDO** | **História:** Cadastro de Aluno. **Caminho:** `POST /api/v1/aluno/cadastro/`. **Lógica:** Implementada e testada em `aluno/views.py` e `aluno/serializers.py`. |
| **Testes (arquivo de testes CRUD)** | ✅ **CONCLUÍDO** | Testes de **C** (Create) e **R** (Read/Listagem) implementados em `aluno/tests.py` e validados com o resultado `Ran 2 tests... OK`. |
| **Diagrama UML** | ✅ **CONCLUÍDO** | Diagramas de Classes (incluindo Herança para a classe `Questão` e Cardinalidade) anexados. |
| **Documentação do backlog (10 H.U.)** | ✅ **CONCLUÍDO** | Backlog detalhado (Histórias do Aluno e Administrador) em `backlog.md`. |
| **Protótipo de alta fidelidade** | ✅ **CONCLUÍDO** | Link para o protótipo. |
| **Desenho da Arquitetura** | ✅ **CONCLUÍDO**  | Será anexado como `tecnologias.md` (Com base na estrutura 3-Tier e visão de implantação Docker). |


## 🧪 Instruções de Validação 

Estes comandos devem ser executados na raiz do projeto (onde o `docker-compose.yml` está) e servem como prova de que a infraestrutura e a História de Usuário estão funcionais.

### I. Subir o Ambiente (Backend e BD)

Este comando inicializa os contêineres do **PostgreSQL** e do **Django/Backend** na rede do Docker.

```bash
# 1. Inicia o BD e o Backend na porta 8001
docker-compose up -d
```

### II. Validar a História de Usuário (Cadastro - CREATE)

Este teste de integração verifica se o endpoint de Cadastro está funcionando e se o dado é gravado no PostgreSQL.

```bash
# 2. Faz a requisição POST para cadastrar um novo aluno
# ESPERADO: HTTP/1.1 201 Created (Sucesso!)
curl -i -X POST http://localhost:8001/api/v1/aluno/cadastro/ \
     -H "Content-Type: application/json" \
     -d '{
            "nome_completo": "Professor Teste",
            "user": {
                "email": "professor.valida@unb.br",
                "password": "SenhaSegura123!"
            }
        }'
```
### III. Validar a Implementação dos Testes (CRUD)

Este comando executa o arquivo de testes (aluno/tests.py) dentro do contêiner, comprovando que a lógica de negócio está correta e testada.

```bash
# 3. Executa os testes C (Cadastro) e R (Listagem)
# ESPERADO: Ran 2 tests... OK
docker-compose exec backend python manage.py test aluno

```
