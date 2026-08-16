<div align="center">

# 💰 FinTrack

**Plataforma de controle financeiro pessoal — moderna, responsiva e profissional.**

[![Node.js](https://img.shields.io/badge/Node.js-18%2B-339933?logo=node.js&logoColor=white)](https://nodejs.org)
[![Express](https://img.shields.io/badge/Express-4.x-000000?logo=express&logoColor=white)](https://expressjs.com)
[![Prisma](https://img.shields.io/badge/Prisma-ORM-2D3748?logo=prisma&logoColor=white)](https://www.prisma.io)
[![License](https://img.shields.io/badge/License-MIT-blue)](LICENSE)

</div>

---

## Sumário

- [Visão geral](#-visão-geral)
- [Funcionalidades](#-funcionalidades)
- [Screenshots](#-screenshots)
- [Tecnologias](#-tecnologias)
- [Arquitetura](#-arquitetura)
- [Banco de dados](#-banco-de-dados)
- [API REST](#-api-rest)
- [Instalação e execução](#-instalação-e-execução)
- [Variáveis de ambiente](#-variáveis-de-ambiente)
- [Seed de dados](#-seed-de-dados)
- [Roadmap](#-roadmap)

---

## 🎯 Visão geral

O **FinTrack** é uma aplicação web de controle financeiro pessoal desenvolvida como projeto de portfólio, com foco em demonstrar boas práticas de:

- HTML5 semântico e acessível
- CSS3 moderno (Grid, Flexbox, Custom Properties)
- JavaScript ES6+ sem frameworks
- API REST com Node.js + Express
- Modelagem relacional com Prisma ORM
- Responsividade real para mobile, tablet e desktop

A proposta é que o produto tenha aparência e comportamento de uma **aplicação SaaS financeira real**, não apenas de um exercício acadêmico.

---

## ✨ Funcionalidades

### Dashboard
- Cards de saldo, receitas, despesas e economia do mês
- Insight financeiro automático baseado em regras
- Gráfico de fluxo de caixa semanal (Chart.js)
- Gráfico de distribuição de gastos por categoria (doughnut)
- Meta em destaque com barra de progresso
- Orçamento mensal por categoria com alerta visual (80% / 100%)
- Tabela de transações recentes

### Transações
- Listagem paginada com busca e filtros por tipo
- Criação via modal: descrição, valor, tipo, categoria, conta, forma de pagamento, recorrência
- Exclusão com ajuste automático do saldo da conta vinculada

### Metas financeiras
- Criação de metas com valor objetivo, prazo e aporte mensal recomendado
- Registro de aportes com atualização em tempo real
- Progresso visual por barra e percentual

### Cartões de crédito
- Cadastro com limite, dia de fechamento e vencimento
- Uso atual da fatura e limite disponível
- Barra de progresso do limite utilizado

### Calculadora financeira
- Simulador de juros compostos com projeção gráfica
- Exibe valor final, total investido, juros gerados e rentabilidade

### Relatórios
- Gráfico de barras com evolução mensal de receitas e despesas

### Configurações
- Visualização de contas bancárias com saldo e tipo
- Listagem de categorias por tipo (receita/despesa)

---

## 📸 Screenshots

> Execute `npm run dev` e acesse `http://localhost:3000` para ver a aplicação em funcionamento.

---

## 🛠 Tecnologias

### Frontend

| Tecnologia | Uso |
|---|---|
| HTML5 | Estrutura semântica e acessível |
| CSS3 | Layout (Grid + Flexbox), Custom Properties, animações |
| JavaScript ES6+ | Módulos, Fetch API, manipulação de DOM |
| [Chart.js](https://www.chartjs.org/) | Gráficos interativos (linha, doughnut, barras) |
| [Font Awesome 6](https://fontawesome.com/) | Ícones |
| [Inter](https://fonts.google.com/specimen/Inter) | Tipografia |

### Backend

| Tecnologia | Uso |
|---|---|
| [Node.js](https://nodejs.org/) | Runtime JavaScript |
| [Express.js](https://expressjs.com/) | Framework HTTP e roteamento |
| [Prisma ORM](https://www.prisma.io/) | Acesso ao banco de dados |
| [CORS](https://github.com/expressjs/cors) | Política de origem cruzada |

### Banco de dados

| Ambiente | Banco |
|---|---|
| Desenvolvimento | SQLite (via Prisma) |
| Produção (recomendado) | PostgreSQL |

---

## 🏗 Arquitetura

```
FinTrack/
│
├── public/                    # Frontend estático
│   ├── index.html             # SPA shell — sidebar, header, modal de transação
│   ├── css/
│   │   ├── variables.css      # Design tokens (cores, sombras, raios)
│   │   ├── layout.css         # Shell do app, sidebar, header, breakpoints
│   │   ├── components.css     # Botões, cards, tabelas, formulários, modals
│   │   └── dashboard.css      # Estilos específicos do dashboard
│   └── js/
│       ├── api.js             # Camada de serviço — wrapper do Fetch API
│       ├── router.js          # Roteamento client-side e eventos globais
│       └── pages/
│           ├── dashboard.js   # Dashboard: cards, gráficos, orçamento
│           ├── transactions.js# Listagem, filtros e exclusão
│           ├── goals.js       # Metas e aportes
│           ├── cards.js       # Cartões de crédito
│           ├── calculator.js  # Simulador de juros compostos
│           ├── reports.js     # Relatórios mensais
│           └── settings.js    # Contas e categorias
│
├── server/
│   ├── index.js               # Bootstrap Express, rotas e static files
│   └── controllers/
│       └── controller.js      # Toda a lógica de negócio e acesso ao banco
│
├── prisma/
│   ├── schema.prisma          # Modelos e relacionamentos
│   ├── seed.js                # Dados iniciais para desenvolvimento
│   └── dev.db                 # Banco SQLite local (gerado automaticamente)
│
├── .env.example               # Template de variáveis de ambiente
├── package.json
└── README.md
```

### Fluxo de dados

```
Navegador (SPA)
    ↓  fetch()
api.js (serviço)
    ↓  HTTP
Express (routes)
    ↓
Controller (lógica de negócio)
    ↓
Prisma ORM
    ↓
SQLite / PostgreSQL
```

---

## 🗄 Banco de dados

### Diagrama de entidades

```
User
 ├── Account[]       (contas bancárias)
 ├── Category[]      (categorias de transação)
 ├── CreditCard[]    (cartões de crédito)
 ├── Transaction[]   (transações)
 ├── Goal[]          (metas financeiras)
 └── Budget[]        (orçamentos mensais)

Transaction → Account, Category, CreditCard?
Budget      → Category
```

### Modelos

**User** — `id · name · email · avatarUrl · monthlyIncome · createdAt · updatedAt`

**Account** — `id · userId · name · type · balance · color`

**Category** — `id · userId · name · type (INCOME|EXPENSE) · icon · color`

**CreditCard** — `id · userId · name · limit · closingDay · dueDay · color`

**Transaction** — `id · userId · accountId · categoryId · cardId? · description · amount · type · date · paymentMethod · isRecurring · notes`

**Goal** — `id · userId · title · targetAmount · currentAmount · monthlyDeposit · deadline · icon · color`

**Budget** — `id · userId · categoryId · limitAmount · month · year`

---

## 📡 API REST

### Autenticação / Usuário

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/api/dashboard` | Resumo completo para o dashboard |

### Transações

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/api/transactions` | Lista com paginação e filtros |
| `POST` | `/api/transactions` | Cria transação e ajusta saldo da conta |
| `DELETE` | `/api/transactions/:id` | Exclui e reverte saldo |

**Parâmetros de filtro (GET):** `search`, `type`, `categoryId`, `accountId`, `cardId`, `startDate`, `endDate`, `page`, `limit`

### Metas

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/api/goals` | Lista todas as metas |
| `POST` | `/api/goals` | Cria nova meta |
| `PATCH` | `/api/goals/:id/deposit` | Adiciona aporte à meta |
| `DELETE` | `/api/goals/:id` | Exclui meta |

### Cartões

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/api/cards` | Lista cartões com uso de fatura calculado |
| `POST` | `/api/cards` | Cadastra novo cartão |
| `DELETE` | `/api/cards/:id` | Remove cartão |

### Dados de apoio

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/api/categories` | Lista categorias do usuário |
| `GET` | `/api/accounts` | Lista contas bancárias |
| `GET` | `/api/reports` | Dados mensais consolidados |

### Exemplo de resposta — `GET /api/dashboard`

```json
{
  "user": { "id": "...", "name": "Vinícius", "email": "..." },
  "summary": {
    "totalBalance": 7750.00,
    "totalIncome": 5000.00,
    "totalExpense": 3000.00,
    "savings": 2000.00,
    "savingsRate": 40.0
  },
  "cashflow": {
    "labels": ["Sem 1", "Sem 2", "Sem 3", "Sem 4", "Sem 5"],
    "income": [5000, 0, 0, 0, 0],
    "expense": [1500, 1150, 350, 0, 0]
  },
  "categoryExpenses": { "Alimentação": 650, "Moradia": 1500 },
  "recentTransactions": [...],
  "highlightGoal": { "title": "Comprar Notebook", "currentAmount": 3720, ... },
  "budgetProgress": [{ "category": {...}, "limitAmount": 800, "spent": 650, "percentage": 81 }]
}
```

---

## 🚀 Instalação e execução

### Pré-requisitos

- **Node.js** 18 ou superior
- **npm** 9 ou superior

### 1. Clone o repositório

```bash
git clone https://github.com/seu-usuario/fintrack.git
cd fintrack
```

### 2. Instale as dependências

```bash
npm install
```

### 3. Configure as variáveis de ambiente

```bash
cp .env.example .env
```

Edite o `.env` conforme necessário (veja a seção [Variáveis de ambiente](#-variáveis-de-ambiente)).

### 4. Gere o cliente Prisma

```bash
npm run db:generate
```

### 5. Aplique o schema no banco

```bash
npm run db:push
```

### 6. Popule com dados iniciais

```bash
npm run db:seed
```

### 7. Inicie o servidor

```bash
npm run dev
```

Acesse **http://localhost:3000** no navegador.

---

## ⚙️ Variáveis de ambiente

Crie um arquivo `.env` na raiz do projeto com base no `.env.example`:

```env
# Porta do servidor (padrão: 3000)
PORT=3000

# URL de conexão com o banco de dados
# SQLite (desenvolvimento):
DATABASE_URL="file:./prisma/dev.db"

# PostgreSQL (produção):
# DATABASE_URL="postgresql://usuario:senha@host:5432/fintrack"
```

> **O arquivo `.env` nunca deve ser commitado.** Ele já está listado no `.gitignore`.

---

## 🌱 Seed de dados

O seed (`npm run db:seed`) cria automaticamente:

| Entidade | Itens |
|---|---|
| Usuário | Vinícius (conta demo) |
| Contas | Conta Principal (R$ 3.250) · Reserva & Poupança (R$ 4.500) |
| Categorias | 7 categorias (receita e despesa) |
| Cartões | Nubank Platinum · XP Visa Infinite |
| Transações | 6 transações do mês atual |
| Metas | Notebook · Reserva de Emergência · Viagem de Férias |
| Orçamentos | 5 categorias com limite definido para o mês atual |

---

## 🗺 Roadmap

### V1 — atual
- [x] Dashboard com cards, gráficos e insights
- [x] Transações com criação, listagem e exclusão
- [x] Metas financeiras com aportes
- [x] Cartões de crédito
- [x] Calculadora de juros compostos
- [x] Relatórios mensais
- [x] Configurações (contas e categorias)
- [x] API REST com Prisma ORM
- [x] Responsividade mobile/tablet/desktop

### V2
- [ ] Autenticação completa (JWT + bcrypt)
- [ ] Exportação CSV e PDF
- [ ] Transações recorrentes automáticas
- [ ] Filtros avançados (data, categoria, conta)
- [ ] Notificações de orçamento excedido

### V3
- [ ] Integração com Open Finance
- [ ] Importação de extratos (OFX/CSV)
- [ ] Categorização automática por IA
- [ ] Previsão de gastos

### V4
- [ ] Aplicativo mobile (React Native)
- [ ] Sistema de assinatura (Free/Pro)
- [ ] Painel administrativo
- [ ] Analytics e métricas avançadas

---

## 📄 Licença

Distribuído sob a licença MIT. Consulte o arquivo `LICENSE` para mais detalhes.
