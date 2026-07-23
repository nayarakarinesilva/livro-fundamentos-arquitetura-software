# Exemplo: Monólito Modular

Em um Monólito Modular, a aplicação é dividida por **funcionalidades (domínios)**, mas continua sendo **um único projeto**.

```text
src/
│
├── modules/
│   ├── auth/
│   ├── users/
│   ├── products/
│   ├── orders/
│   └── payments/
│
├── App.jsx
└── main.jsx
```

Cada módulo possui tudo o que precisa.

Exemplo do módulo **users**:

```text
users/
├── components/
├── hooks/
├── pages/
├── services/
└── index.js
```

O mesmo acontece para **products**, **orders**, **payments**, etc.

> Cada módulo cuida apenas da sua responsabilidade.

---

# Como visualizar?

Em vez de organizar por tipo de arquivo:

```text
src/
├── components/
├── pages/
├── services/
└── hooks/
```

O projeto é organizado por funcionalidades:

```text
src/
└── modules/
    ├── auth/
    ├── users/
    ├── products/
    ├── orders/
    └── payments/
```

Essa é a principal diferença do Monólito Modular.

---

# Comunicação entre módulos

## Comunicação direta

Um módulo chama outro diretamente.

```text
Orders
   │
   ▼
Payments
```

Exemplo:

```text
Orders
└── createOrder()
        │
        ▼
Payments
└── processPayment()
```

---

## Comunicação por mediador

Existe uma camada intermediária.

```text
Orders
   │
   ▼
Payment Gateway
   │
   ▼
Payments
```

Assim, o módulo **Orders** não precisa conhecer como o pagamento é realizado.

---

# Tudo continua sendo uma única aplicação

Mesmo organizado em módulos, o sistema continua sendo um único projeto.

```text
                Aplicação
                     │
     ┌───────────────┼───────────────┐
     │               │               │
     ▼               ▼               ▼
   Auth          Products        Orders
     │               │               │
     └───────────────┼───────────────┘
                     │
                 Um único Deploy
```

Se a aplicação parar, **todos os módulos param juntos**.

---

# Resumo Visual

```text
Monólito Tradicional

src/
├── components/
├── pages/
├── services/
└── hooks/
```

↓

```text
Monólito Modular

src/
└── modules/
    ├── auth/
    ├── users/
    ├── products/
    ├── orders/
    └── payments/
```

> O código fica organizado por **domínio de negócio**, mas continua sendo **uma única aplicação**.