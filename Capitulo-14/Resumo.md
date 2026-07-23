# Capítulo 14 – Arquitetura Baseada em Serviços (Service-Based Architecture)

A **Arquitetura Baseada em Serviços (Service-Based Architecture)** é um estilo que fica entre o **Monólito Modular** e os **Microserviços**.

Ela divide o sistema em **serviços de negócio maiores (coarse-grained)**, cada um responsável por uma área completa da aplicação, mas normalmente utilizando um **banco de dados compartilhado**.

---

# 1. O conceito: o caminho do meio

Imagine um sistema de e-commerce.

Em vez de colocar toda a lógica em uma única aplicação enorme (monólito) ou criar dezenas de microserviços pequenos, dividimos o sistema em poucos serviços maiores:

- **Catálogo** → produtos, categorias e pesquisa.
- **Checkout** → carrinho, pagamento e pedidos.
- **Usuários** → autenticação e perfis.

Cada serviço possui sua própria lógica de negócio e pode ser implantado de forma independente.

A principal diferença para os microserviços é que **todos podem compartilhar o mesmo banco de dados**, simplificando consultas e transações.

---

# 2. Vantagens

## Transações simples

Como o serviço concentra toda a lógica de um domínio, é possível utilizar transações tradicionais do banco de dados.

Exemplo:

- criar pedido;
- atualizar estoque;
- registrar pagamento.

Se ocorrer algum erro, basta executar um **rollback**.

---

## Menor complexidade

Comparada aos microserviços, essa arquitetura possui:

- menos comunicação entre serviços;
- menos infraestrutura;
- deploy mais simples;
- manutenção mais barata.

---

## Escalabilidade

Caso o serviço de Checkout receba muito mais acessos que os demais, basta aumentar apenas esse serviço.

---

# 3. Exemplo com React e Node.js

### Front-end

Uma única aplicação React.

Ela apenas consome os serviços disponíveis.

```
React
   │
   ├── /catalog
   ├── /checkout
   └── /users
```

---

### Back-end

Cada domínio possui sua própria API.

```
Catalog Service
Checkout Service
User Service
```

Todas acessam o mesmo banco de dados.

---

# 4. Estrutura do projeto

```
e-commerce-system/
│
├── apps/
│   └── storefront-ui/          # Aplicação React
│       └── src/
│           ├── features/
│           │   ├── catalog/
│           │   ├── checkout/
│           │   └── users/
│           └── components/
│
├── services/
│   ├── catalog-service/        # API responsável pelos produtos
│   ├── checkout-service/       # API responsável por carrinho e pagamentos
│   └── user-service/           # API responsável pelos usuários
│
└── shared-db/                  # Banco de dados utilizado por todos os serviços
```

## Como essa estrutura funciona?

### `apps/`

Contém as aplicações cliente.

Neste exemplo existe apenas uma aplicação React.

```
apps/
└── storefront-ui/
```

---

### `services/`

Cada pasta representa um serviço independente.

```
services/
├── catalog-service/
├── checkout-service/
└── user-service/
```

Cada serviço possui seu próprio:

- controllers;
- services;
- rotas;
- regras de negócio.

Por exemplo:

```
checkout-service/
│
├── controllers/
├── routes/
├── services/
├── models/
└── app.js
```

Toda a lógica de compras fica apenas neste serviço.

---

### `shared-db/`

Todos os serviços acessam o mesmo banco de dados.

```
                 Banco de Dados
                       ▲
       ┌───────────────┼───────────────┐
       │               │               │
Catalog Service  Checkout Service  User Service
```

Isso facilita:

- consultas;
- transações;
- integridade dos dados.

---

# 5. Características

| Característica | Avaliação | Explicação |
|----------------|-----------|------------|
| **Custo** | ⭐⭐⭐⭐ | Mais barato que microserviços. |
| **Simplicidade** | ⭐⭐⭐ | Menos complexo que sistemas distribuídos. |
| **Modularidade** | ⭐⭐⭐⭐ | Serviços separados por domínio. |
| **Escalabilidade** | ⭐⭐⭐ | É possível escalar apenas o serviço necessário. |
| **Testabilidade** | ⭐⭐⭐⭐ | Cada serviço pode ser testado isoladamente. |
| **Deploy** | ⭐⭐⭐⭐ | Atualizações independentes por serviço. |
| **Tolerância a Falhas** | ⭐⭐⭐ | O banco compartilhado continua sendo um ponto único de falha. |

---

# Resumo

A Arquitetura Baseada em Serviços é indicada para sistemas que precisam de mais organização do que um monólito tradicional, mas que ainda não justificam toda a complexidade dos microserviços.

Ela oferece:

- separação por domínio de negócio;
- deploy independente dos serviços;
- escalabilidade moderada;
- menor custo operacional;
- banco de dados compartilhado para simplificar o desenvolvimento.