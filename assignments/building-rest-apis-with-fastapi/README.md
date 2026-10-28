# 📘 Atividade: Building REST APIs with FastAPI

## 🎯 Objetivo

Construa uma API REST de gerenciamento de tarefas com FastAPI, praticando rotas HTTP, modelos Pydantic e respostas JSON. Ao concluir, você será capaz de implementar operações CRUD, validar entradas e verificar respostas de sucesso e erro.

## 📝 Tarefas

### 🛠️ Iniciar uma aplicação FastAPI

#### Descrição
Comece criando uma aplicação mínima que responda a uma requisição HTTP. Esta atividade pressupõe conhecimentos de funções, listas, dicionários e tipos em Python; o primeiro passo deve levar cerca de 10 minutos.

Crie e ative um ambiente virtual e instale o FastAPI com as ferramentas de desenvolvimento:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install "fastapi[standard]"
```

Crie um arquivo `main.py` com uma instância de `FastAPI` chamada `app`. Execute a aplicação com `fastapi dev main.py` e acesse `http://127.0.0.1:8000/docs`.

#### Requisitos
O programa concluído deve:

- Iniciar localmente sem erros de importação.
- Disponibilizar `GET /` com status `200` e resposta JSON `{"message": "API de tarefas online"}`.
- Exibir a rota na documentação interativa em `/docs`.

### 🛠️ Criar e consultar tarefas

#### Descrição
Implemente a criação e a leitura de tarefas usando modelos Pydantic. Armazene os registros em memória; não é necessário usar banco de dados, autenticação ou serviços externos. Os dados podem ser perdidos ao reiniciar o servidor.

Exemplo de corpo para `POST /tasks`:

```json
{
  "title": "Estudar FastAPI",
  "completed": false
}
```

Exemplo de resposta para a primeira tarefa criada:

```json
{
  "id": 1,
  "title": "Estudar FastAPI",
  "completed": false
}
```

#### Requisitos
O programa concluído deve:

- Definir um modelo de entrada com `title` obrigatório, entre 1 e 100 caracteres, e `completed` booleano com valor padrão `false`.
- Definir um modelo de resposta contendo `id` inteiro, `title` e `completed`, usando-o nas rotas que retornam tarefas.
- Implementar `POST /tasks` com status `201` e gerar um identificador único que não seja reutilizado após exclusões durante a execução.
- Implementar `GET /tasks` com status `200`, retornando uma lista JSON, inclusive `[]` quando não houver tarefas.
- Implementar `GET /tasks/{task_id}` com status `200` para uma tarefa existente e `404` com uma mensagem em `detail` para um identificador inexistente.
- Retornar status `422` quando o título estiver ausente, vazio ou exceder 100 caracteres, ou quando o identificador da rota não for um inteiro válido.

### 🛠️ Atualizar e excluir tarefas

#### Descrição
Complete as operações CRUD permitindo substituir os dados de uma tarefa e removê-la. Use `PUT` para atualização completa: o cliente deve enviar tanto `title` quanto `completed`.

#### Requisitos
O programa concluído deve:

- Implementar `PUT /tasks/{task_id}` com um modelo de entrada que exija os dois campos e mantenha as mesmas regras de validação do título.
- Retornar status `200` e a tarefa atualizada, preservando seu identificador.
- Retornar status `422` para atualizações sem algum dos campos obrigatórios ou com um título inválido.
- Implementar `DELETE /tasks/{task_id}` com status `204` e sem corpo de resposta.
- Retornar status `404` com uma mensagem em `detail` ao atualizar ou excluir uma tarefa inexistente.
- Garantir que uma tarefa excluída não apareça na listagem e que sua consulta retorne `404`.

### 🛠️ Verificar o comportamento da API

#### Descrição
Utilize a documentação interativa em `/docs` para executar o fluxo completo e conferir os códigos HTTP e os corpos das respostas. Registre em um arquivo `verification.md` as requisições realizadas, o status esperado e o resultado observado.

#### Requisitos
A verificação concluída deve:

- Consultar a lista vazia antes de cadastrar tarefas.
- Criar uma tarefa sem informar `completed` e confirmar que a resposta contém `false`.
- Criar outra tarefa e confirmar que os identificadores são diferentes.
- Consultar uma tarefa, atualizar seu título e marcar `completed` como `true`.
- Excluir uma tarefa, confirmar a ausência de corpo na resposta `204` e verificar que sua consulta retorna `404`.
- Criar uma nova tarefa após a exclusão e confirmar que o identificador excluído não foi reutilizado.
- Conferir o status `404` ao tentar atualizar e excluir um identificador inexistente.
- Conferir o status `422` para título ausente, vazio ou com mais de 100 caracteres, identificador não inteiro e atualização incompleta.