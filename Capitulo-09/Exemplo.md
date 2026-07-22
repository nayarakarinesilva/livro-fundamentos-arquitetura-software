# 1. Estilo x Padrão de Arquitetura

## Exemplo de um estilo: Arquitetura em Camadas

```text
src/
│
├── pages/
│   ├── Login.jsx
│   └── Dashboard.jsx
│
├── components/
│   ├── Button.jsx
│   └── Header.jsx
│
├── services/
│   └── api.js
│
├── repositories/
│   └── UserRepository.js
│
└── App.jsx
```

Cada pasta representa uma responsabilidade diferente.

Já um **padrão de arquitetura** pode existir dentro desse estilo.

### Exemplo: Repository Pattern

```text
src/
│
├── repositories/
│   ├── UserRepository.js
│   ├── ProductRepository.js
│   └── OrderRepository.js
```

Nesse caso estamos utilizando o **Repository Pattern**, mas o estilo continua sendo **Arquitetura em Camadas**.

---

# 2. Padrões Fundamentais

## Big Ball of Mud

Um projeto sem organização normalmente acaba assim:

```text
src/
│
├── App.jsx
├── Login.jsx
├── Dashboard.jsx
├── User.jsx
├── Product.jsx
├── api.js
├── auth.js
├── utils.js
├── helper.js
├── functions.js
├── styles.css
├── index.css
├── teste.js
├── novo.js
├── copia.js
└── ...
```

Ou pior:

```text
src/
└── App.jsx
```

Com centenas ou milhares de linhas contendo:

- Interface
- Chamadas da API
- Regras de negócio
- Validações
- Manipulação do banco
- Autenticação

Tudo junto. Esse é o famoso **Big Ball of Mud**.

## Arquitetura Unitária

Tudo faz parte da mesma aplicação.

```text
Sistema
├── Interface
├── Regras de negócio
├── Banco de dados
└── Relatórios
```

Não existem aplicações separadas.

## Cliente / Servidor

### Frontend (React)

```text
src/
├── pages/
├── components/
├── hooks/
└── services/
```

⬇️ Comunicação via HTTP

### Backend (Node.js)

```text
src/
├── controllers/
├── services/
├── models/
├── routes/
└── database/
```

Cada aplicação possui sua própria estrutura.

---

# 3. Como dividir a arquitetura

## Particionamento Técnico

Aqui a divisão é pela tecnologia utilizada.

```text
src/
│
├── components/
├── pages/
├── hooks/
├── services/
├── contexts/
├── utils/
├── assets/
└── styles/
```

Ou seja:

- Todos os componentes ficam juntos.
- Todos os hooks ficam juntos.
- Todos os serviços ficam juntos.

### Exemplo

```text
src/

components/
├── UserCard.jsx
├── ProductCard.jsx
├── OrderCard.jsx
└── Button.jsx

services/
├── userService.js
├── productService.js
└── orderService.js
```

Perceba que arquivos de funcionalidades diferentes ficam misturados na mesma pasta.

## Particionamento por Domínio

Agora cada funcionalidade possui sua própria estrutura.

```text
src/
│
└── modules/
    ├── users/
    │   ├── components/
    │   ├── hooks/
    │   ├── services/
    │   ├── pages/
    │   └── routes.js
    │
    ├── products/
    │   ├── components/
    │   ├── hooks/
    │   ├── services/
    │   ├── pages/
    │   └── routes.js
    │
    └── orders/
        ├── components/
        ├── hooks/
        ├── services/
        ├── pages/
        └── routes.js
```

- Tudo relacionado a **Usuários** fica na pasta `users`.
- Tudo relacionado a **Produtos** fica na pasta `products`.
- Tudo relacionado a **Pedidos** fica na pasta `orders`.

Esse modelo facilita muito a manutenção.

---

# 4. Monolítico x Distribuído

## Monolítico

Existe apenas um projeto.

```text
MeuSistema/
├── frontend/
├── Login/
├── Dashboard/
├── Financeiro/
├── Estoque/
├── Vendas/
└── build/
```

Todo o sistema é publicado junto.

## Distribuído

Agora cada domínio pode virar um serviço.

```text
Sistema/
├── frontend-react/
├── api-users/
├── api-products/
├── api-orders/
└── api-finance/
```

Cada API pode ser desenvolvida por uma equipe diferente.

---

# 6. Organização das Equipes (Team Topologies)

A estrutura do projeto pode refletir a divisão das equipes.

### Equipe Usuários

```text
modules/
└── users/
```

### Equipe Produtos

```text
modules/
└── products/
```

### Equipe Financeiro

```text
modules/
└── finance/
```

Cada equipe trabalha apenas no seu domínio.

---

# Comparação Visual

## Particionamento Técnico

```text
src/
├── components/
├── hooks/
├── services/
├── pages/
└── utils/
```

> Organização baseada no tipo de arquivo.

## Particionamento por Domínio

```text
src/
└── modules/
    ├── users/
    ├── products/
    ├── orders/
    └── finance/
```

> Organização baseada nas funcionalidades do sistema.