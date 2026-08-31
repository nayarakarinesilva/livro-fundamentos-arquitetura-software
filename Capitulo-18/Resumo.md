# Capítulo 18 — Arquitetura de Microserviços

A **Arquitetura de Microserviços** divide uma aplicação grande em **serviços menores e independentes**.

O principal objetivo é facilitar:

- Mudanças;
- Deploy independente;
- Escalabilidade;
- Trabalho de equipes diferentes.

> 💡 Cada microserviço cuida de uma parte específica do negócio e possui seus próprios dados.

---

## 1. Bounded Context e Desacoplamento

No SOA, vimos bastante **reutilização de serviços**. Nos Microserviços, o foco é diferente: **evitar dependências entre os serviços**.

### Bounded Context

Cada microserviço possui uma responsabilidade bem definida.

Exemplo:

```text
Catalog Service → Produtos
Order Service   → Pedidos
Payment Service → Pagamentos
```

Cada serviço possui seu próprio código e dados.

### Database-per-Service

Cada serviço também deve controlar seu próprio banco.

```text
Catalog Service → Catalog DB
Order Service   → Order DB
Payment Service → Payment DB
```

Um serviço **não deve acessar diretamente o banco de outro**.

❌

```text
Order Service → Catalog DB
```

✅

```text
Order Service → Catalog Service → Catalog DB
```

Assim, cada serviço fica mais independente.

---

## 2. Infraestrutura

Como existem vários serviços independentes, algumas ferramentas ajudam a organizar a comunicação.

### API Gateway

É a **porta de entrada** da aplicação.

```text
React
  ↓
API Gateway
  ├──→ Catalog Service
  ├──→ Order Service
  └──→ Payment Service
```

O Gateway recebe a requisição e encaminha para o serviço correto.

### Sidecar

É um pequeno serviço que fica ao lado do microserviço para cuidar de tarefas como:

- Logs;
- Monitoramento;
- Segurança;
- Resiliência.

```text
Order Service
      +
   Sidecar
```

### Service Mesh

Ajuda a controlar e monitorar a comunicação entre vários serviços e seus Sidecars.

### Service Discovery

Ajuda um serviço a **encontrar outro serviço**, principalmente quando existem várias instâncias.

```text
Order Service
      ↓
Service Discovery
      ↓
Catalog 1
Catalog 2
Catalog 3
```

---

## 3. Comunicação e Saga

Os microserviços podem se comunicar por **APIs** ou **eventos**.

### Coreografia

Os serviços reagem a eventos sem existir um controlador central.

```text
Pedido criado
     ↓
Pagamento
     ↓
Estoque
     ↓
Entrega
```

Cada serviço sabe o que fazer quando recebe determinado evento.

### Orquestração

Um serviço específico controla um fluxo.

```text
Order Orchestrator
      ├──→ Pagamento
      ├──→ Estoque
      └──→ Entrega
```

### Saga

Como cada serviço possui seu próprio banco, não existe um `rollback` único para todos.

Se algo der errado, usamos **ações de compensação**.

Exemplo:

```text
Criar pedido      ✅
Pagar             ✅
Reservar estoque  ❌
        ↓
Estornar pagamento
        ↓
Cancelar pedido
```

> 💡 A Saga tenta desfazer as ações anteriores através de novas ações.

---

## 4. Exemplo em Node.js

### Catalog Service

O catálogo possui seus próprios dados:

```javascript
const catalogDb = [
  {
    id: "p101",
    name: "Livro de Arquitetura de Software",
    price: 99.90
  }
];

app.get("/products/:id", (req, res) => {
  const product = catalogDb.find(
    p => p.id === req.params.id
  );

  if (!product) {
    return res.status(404).json({
      error: "Produto não encontrado"
    });
  }

  res.json(product);
});
```

### Order Service

O pedido precisa do preço do produto.

Em vez de acessar `catalogDb`, ele chama a API do `Catalog Service`:

```javascript
const response = await axios.get(
  `http://localhost:3001/products/${productId}`
);

const product = response.data;

const orderTotal = product.price * quantity;
```

O fluxo fica:

```text
Order Service
      ↓
Catalog Service
      ↓
Catalog DB
```

Isso mantém os serviços desacoplados.

---

## 5. Exemplo em React — Micro-frontends

O conceito de independência também pode ser usado no frontend.

Por exemplo:

```text
Loja
 ├── Catálogo
 ├── Carrinho
 └── Checkout
```

O `ProductCard` pode disparar um evento:

```javascript
const event = new CustomEvent(
  "PRODUCT_ADDED_TO_CART",
  { detail: product }
);

window.dispatchEvent(event);
```

O carrinho escuta esse evento:

```javascript
window.addEventListener(
  "PRODUCT_ADDED_TO_CART",
  handleEvent
);
```

Assim:

```text
ProductCard
     ↓
Evento
     ↓
CartButton
```

O catálogo não precisa importar diretamente o código do carrinho.

---

## 6. Estrutura do Projeto

```text
/microservices-system
  │
  ├── /micro-frontends
  │    ├── /catalog-mfe
  │    │    ├── ProductCard.jsx
  │    │    └── package.json
  │    │
  │    └── /cart-mfe
  │         ├── CartButton.jsx
  │         └── package.json
  │
  ├── /backend-services
  │    ├── /catalog-service
  │    │    ├── server.js
  │    │    └── product-database
  │    │
  │    └── /order-service
  │         ├── server.js
  │         └── order-database
  │
  └── /api-gateway
       └── gateway.js
```

A ideia principal é:

```text
Catálogo → seus dados
Pedidos  → seus dados
Carrinho → sua interface
```

Cada parte possui uma responsabilidade própria.

---

## 7. Vantagens e Desvantagens

### ✅ Vantagens

- **Deploy independente:** podemos publicar um serviço sem publicar todos.
- **Escalabilidade:** podemos aumentar apenas o serviço que está recebendo muita demanda.
- **Desacoplamento:** os serviços dependem menos uns dos outros.
- **Evolução:** cada serviço pode evoluir de forma independente.
- **Tecnologias diferentes:** serviços podem usar tecnologias diferentes quando necessário.

### ❌ Desvantagens

- **Custo alto:** muitos serviços exigem mais infraestrutura.
- **Complexidade:** existem muitos serviços para administrar.
- **Comunicação pela rede:** pode gerar latência e falhas.
- **Testes mais complexos:** é necessário testar a comunicação entre serviços.
- **Transações difíceis:** operações que envolvem vários serviços exigem soluções como Saga.

---

## 8. Microservices Scorecard

| Característica | Avaliação | Explicação |
|---|---|---|
| **Custo Geral** | 💰💰💰💰💰 | Alto devido à infraestrutura necessária |
| **Particionamento** | **Domínio** | Serviços separados por áreas do negócio |
| **Número de Quanta** | **1 a Muitos** | Cada serviço pode funcionar de forma independente |
| **Simplicidade** | ⭐ | É uma arquitetura complexa |
| **Modularidade** | ⭐⭐⭐⭐⭐ | Alto nível de independência |
| **Testabilidade** | ⭐⭐⭐⭐⭐ | Serviços menores facilitam testes |
| **Deployability** | ⭐⭐⭐⭐⭐ | Cada serviço pode ser publicado separadamente |
| **Evolvabilidade** | ⭐⭐⭐⭐⭐ | Serviços podem evoluir de forma independente |
| **Performance** | ⭐⭐ | A comunicação pela rede pode aumentar a latência |
| **Scalability** | ⭐⭐⭐⭐⭐ | Podemos escalar apenas o serviço necessário |
| **Elasticidade** | ⭐⭐⭐⭐⭐ | Serviços podem aumentar ou diminuir conforme a demanda |
| **Tolerância a Falhas** | ⭐⭐⭐⭐⭐ | Uma falha pode ficar isolada em um serviço |

---

## 9. Resumo

A ideia principal dos **Microserviços** é:

```text
Aplicação grande
      ↓
Divide em serviços menores
      ↓
Cada serviço cuida de uma parte do negócio
      ↓
Cada serviço possui seus próprios dados
      ↓
Serviços se comunicam por APIs ou eventos
```

### Microserviços em uma frase:

> **Dividir o sistema em serviços independentes para facilitar mudanças, deploys e escalabilidade.**

### ⚠️ Principal cuidado:

> **Microserviços aumentam a independência, mas também aumentam a complexidade.**