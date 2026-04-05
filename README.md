# <h1> 📦 Api Gerenciador De Tarefas</h1>

O **ApiGerenciadorDeTarefas** é uma solução completa de back-end para o gerenciamento de fluxos de trabalho, equipes e permissões. Desenvolvida como uma API RESTful, ela permite a organização de tarefas com diferentes níveis de prioridade, controle de acesso baseado em papéis (**RBAC**) e rastreabilidade total através de históricos de alteração.

---

## 🛠️ Tecnologias Utilizadas

*   ![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white) **Runtime**
*   ![Express](https://img.shields.io/badge/express.js-%23404d59.svg?style=for-the-badge&logo=express&logoColor=%2361DAFB) **Framework**
*   ![Prisma](https://img.shields.io/badge/Prisma-3982CE?style=for-the-badge&logo=Prisma&logoColor=white) **ORM**
*   ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white) **Banco de Dados**
*   ![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens) **Autenticação**
*   ![Zod](https://img.shields.io/badge/Zod-3068b7?style=for-the-badge&logo=zod&logoColor=white) **Validação**
*   ![Jest](https://img.shields.io/badge/-jest-%23C21325?style=for-the-badge&logo=jest&logoColor=white) **Testes**
*   ![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white) **Infraestrutura**

---

## ✨ Funcionalidades

*   🔐 **Segurança:** Autenticação JWT e proteção de senhas com `bcrypt`.
*   👥 **Níveis de Acesso:** Diferenciação entre usuários `admin` (gestão total) e `member` (visualização restrita).
*   🏢 **Gestão de Equipes:** Criação de times e associação dinâmica de membros.
*   📋 **Sistema de Tarefas:** CRUD completo com controle de prioridade, status e atribuições.
*   🕒 **Histórico:** Registro automático de todas as mudanças de status para auditoria.
*   📄 **Paginação:** Performance otimizada em listagens de usuários e equipes.
*   ✅ **Robustez:** Validação de dados de entrada com Zod e cobertura de testes com Jest.

---

## 📂 Estrutura do Projeto

```text
prisma/
 ├── migrations/      # Histórico de alterações do banco
 └── schema.prisma    # Definição dos modelos de dados
src/
 ├── configs/         # Configurações globais (Auth, etc.)
 ├── controllers/     # Lógica das rotas e orquestração
 ├── database/        # Inicialização do Prisma Client
 ├── middlewares/     # Validação de JWT e RBAC
 ├── routes/          # Definição dos endpoints da API
 ├── tests/           # Testes unitários e de integração
 ├── types/           # Definições de tipos TypeScript
 └── utils/           # Funções auxiliares e AppError
```
<brb>## 📦 Instalação e Execução

### Pré-requisitos
- Node.js v18+
- Docker Desktop

### Passo a Passo

**1. Clone o repositório:**
```bash
git clone https://github.com/FernandoCyber3/Api-Gerenciamento-de-Tarefas-com-Node.js.git
cd Api-Gerenciamento-de-Tarefas-com-Node.js
```

**2. Instale as dependências:**
```bash
npm install
```

**3. Configure o ambiente:**

Crie um arquivo `.env` na raiz do projeto:
```env
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/gerenciadortarefas?schema=public"
JWT_SECRET="sua_chave_secreta"
PORT=3333
```

**4. Suba o banco e execute as migrations:**
```bash
docker-compose up -d
npx prisma migrate deploy
```

**5. Inicie o servidor:**
```bash
npm run dev
```

> O servidor estará disponível em: http://localhost:3333

---

## 🧪 Suíte de Testes

Para rodar os testes automatizados em ambiente de desenvolvimento:
```bash
npm run test:dev
```

---

## 📚 Principais Endpoints

### 🔑 Sessões e Usuários

| Método | Rota | Descrição | Acesso |
|---|---|---|---|
| `POST` | `/users` | Cadastro de novo usuário | Público |
| `POST` | `/sessions` | Login (retorna Token JWT) | Público |
| `GET` | `/users` | Lista usuários (paginado) | `admin` |

### 👥 Equipes (Teams)

| Método | Rota | Descrição | Acesso |
|---|---|---|---|
| `POST` | `/teams` | Criar nova equipe | `admin` |
| `POST` | `/team-members` | Adicionar membro à equipe | `admin` |
| `GET` | `/teams` | Listar todas as equipes | `admin` |

### 📋 Tarefas (Tasks)

| Método | Rota | Descrição | Acesso |
|---|---|---|---|
| `POST` | `/tasks` | Criar nova tarefa | `admin` |
| `GET` | `/tasks` | Listar todas as tarefas | `admin` |
| `GET` | `/team-tasks` | Ver tarefas das minhas equipes | `member` |
| `POST` | `/task-history` | Atualizar status da tarefa | `member` / `admin` |

---

## ⚠️ Tratamento de Erros

A API utiliza um middleware global para garantir respostas padronizadas:

| Status | Descrição |
|---|---|
| `400 Bad Request` | Erros de validação (Zod) ou lógica de negócio |
| `401 Unauthorized` | Token ausente, expirado ou inválido |
| `403 Forbidden` | Usuário autenticado, mas sem permissão de cargo (`role`) |
| `404 Not Found` | Recurso não encontrado no banco de dados |

---

## 🔒 Autenticação

Todas as rotas protegidas exigem o envio do token no cabeçalho da requisição:

```http
Authorization: Bearer <seu_token_jwt>
```
