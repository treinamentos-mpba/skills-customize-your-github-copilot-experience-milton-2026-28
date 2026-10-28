# 📘 Atividade: Testes Automatizados de APIs com pytest e FastAPI

## 🎯 Objetivo

Construa uma suíte de testes de integração para a API de tarefas usando pytest e o TestClient do FastAPI. Pratique isolamento de estado com fixtures, parametrização e verificação de contratos HTTP para detectar regressões de forma reproduzível.

## 📝 Tarefas

### 🛠️ Executar o primeiro teste da API

#### Descrição
Esta atividade é de nível avançado e pressupõe domínio de funções, modelos Pydantic e operações CRUD. Reutilize sua implementação concluída de [Building REST APIs with FastAPI](../building-rest-apis-with-fastapi/README.md), incluindo o arquivo `main.py` com a instância `app`; o repositório fornece o enunciado anterior, não uma solução pronta da API.

No ambiente virtual desse projeto, instale as dependências e crie `tests/test_tasks.py`. Esta primeira etapa deve levar menos de 10 minutos para quem já possui a API funcionando.

```bash
python -m pip install "fastapi[standard]" pytest httpx
python -m pytest -q
```

Execute o comando de testes na pasta que contém `main.py`. Use `fastapi.testclient.TestClient` para chamar a aplicação diretamente, sem iniciar Uvicorn ou depender de uma porta de rede.

#### Requisitos
A suíte concluída deve:

- Ter pelo menos um teste descoberto pelo pytest em `tests/test_tasks.py`.
- Importar `app` de `main` e realizar requisições com `TestClient`.
- Verificar que `GET /` retorna status `200` e exatamente o JSON `{"message": "API de tarefas online"}`.
- Executar com `python -m pytest -q` sem exigir um servidor externo.

### 🛠️ Isolar o estado com fixtures

#### Descrição
A API armazena tarefas em memória. Crie em `tests/conftest.py` uma fixture de escopo de função que entregue um cliente e garanta um armazenamento vazio e um gerador de identificadores reinicializado antes de cada teste. Adapte a preparação à organização da sua API, sem assumir que existe uma função de limpeza pronta.

Você pode usar `monkeypatch` para substituir as referências de armazenamento e contador no módulo que as rotas realmente consultam. Se a API já recebe o repositório via `Depends`, use `app.dependency_overrides` para fornecer uma instância nova por teste e remova o override ao finalizar. Escolha uma estratégia compatível com sua implementação; não é necessário implementar as duas.

#### Requisitos
A suíte concluída deve:

- Centralizar a criação do cliente na fixture e usar `TestClient` como gerenciador de contexto para executar o ciclo de vida da aplicação.
- Preparar armazenamento e contador independentes a cada teste e restaurar alterações ao finalizar, inclusive quando um teste falhar.
- Não criar endpoints de limpeza exclusivos para testes nem acessar serviços externos.
- Incluir um teste que crie uma tarefa e outro que confira a listagem vazia ao iniciar, ambos usando a fixture.
- Passar com esses dois testes executados individualmente e nas duas ordens, informando seus identificadores de teste ao comando `python -m pytest -q`.

### 🛠️ Verificar o contrato do fluxo CRUD

#### Descrição
Automatize o ciclo completo de uma tarefa, conferindo tanto o status HTTP quanto os dados retornados. Cada teste deve preparar seus próprios registros usando a fixture, sem depender de registros ou identificadores criados por outro teste.

#### Requisitos
A suíte concluída deve:

- Verificar que `POST /tasks` com apenas `title` retorna `201`, um `id` inteiro e positivo, o título enviado e `completed` igual a `false`.
- Criar duas tarefas e verificar que os identificadores são distintos.
- Usar o identificador retornado para consultar a tarefa com `GET /tasks/{task_id}`, esperando `200` e os mesmos dados da criação.
- Verificar que `GET /tasks` retorna `200` e contém as tarefas cadastradas.
- Verificar que `PUT /tasks/{task_id}` com `title` e `completed` retorna `200`, altera os dois campos e preserva o identificador; confirmar a alteração com uma nova consulta.
- Verificar que `DELETE /tasks/{task_id}` retorna `204` e corpo vazio, sem tentar interpretar essa resposta como JSON.
- Confirmar que o registro excluído desaparece da listagem e que sua consulta retorna `404` com uma mensagem em `detail`.
- Criar uma tarefa após a exclusão, no mesmo teste, e confirmar que o identificador excluído não foi reutilizado.

### 🛠️ Parametrizar limites e erros de validação

#### Descrição
Use `pytest.mark.parametrize` para exercitar entradas inválidas e limites válidos sem duplicar o corpo dos testes. Em respostas de validação, confira o status e a estrutura do erro sem depender do texto exato produzido por uma versão específica do Pydantic.

Ao concluir, demonstre que a suíte detecta uma regressão: altere temporariamente o status de criação de `201` para `200`, execute o teste correspondente e observe a falha. Restaure o comportamento correto antes da entrega.

#### Requisitos
A suíte concluída deve:

- Parametrizar `POST /tasks` com título ausente, vazio e com 101 caracteres, esperando `422` em todos os casos e uma lista não vazia em `detail`.
- Testar títulos válidos com exatamente 1 e 100 caracteres, esperando `201` e a preservação do título.
- Parametrizar atualizações sem `title`, sem `completed`, com título vazio e com 101 caracteres, esperando `422`; criar antes uma tarefa válida em cada caso e confirmar que ela permanece inalterada após a rejeição.
- Parametrizar `GET`, `PUT` e `DELETE` para um identificador inteiro inexistente, esperando `404` e uma mensagem em `detail`; enviar um corpo válido no caso de `PUT`.
- Parametrizar as mesmas três operações com identificador não inteiro, como `abc`, esperando `422`; usar corpo válido em `PUT` para isolar o erro do parâmetro da rota.
- Confirmar que criações inválidas não adicionam registros ao armazenamento.
- Usar nomes descritivos nos testes e identificadores legíveis nos casos parametrizados.
- Entregar `tests/conftest.py` e `tests/test_tasks.py`, com todos os testes passando após restaurar a regressão temporária, e registrar no relatório de entrega o comando executado e o resumo do pytest.