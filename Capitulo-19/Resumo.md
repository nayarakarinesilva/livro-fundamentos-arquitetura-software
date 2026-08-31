# Capítulo 19 — Escolhendo o Estilo de Arquitetura

O **Capítulo 19** explica como escolher a arquitetura mais adequada para um sistema.

A principal ideia é:

> **Não existe uma arquitetura perfeita para todos os projetos. A escolha depende das necessidades do sistema.**

---

## 1. Não escolha arquitetura só porque está na moda

Tecnologias como **Docker, Kubernetes e Microserviços** ficaram muito populares.

Mas isso não significa que todo projeto precisa usá-las.

Por exemplo:

```text
Site pequeno
↓
Poucos usuários
↓
Poucas funcionalidades
↓
Monólito pode ser suficiente
```

Não faria sentido criar dezenas de microserviços para um sistema pequeno.

> 💡 **Escolha a arquitetura pela necessidade do projeto, não pela moda.**

---

## 2. Monólito ou sistema distribuído?

Uma das principais decisões é escolher entre **Monólito** e **Distribuído**.

### Monólito

Todo o sistema funciona como uma única aplicação.

```text
        Aplicação
    ┌───────────────┐
    │ Usuários      │
    │ Produtos      │
    │ Pedidos       │
    │ Pagamentos    │
    └───────────────┘
```

É uma boa opção quando as partes do sistema possuem necessidades parecidas.

### Distribuído

O sistema é dividido em várias aplicações.

```text
Usuários  → Serviço de Usuários
Produtos  → Serviço de Produtos
Pedidos   → Serviço de Pedidos
Pagamentos → Serviço de Pagamentos
```

Pode ser interessante quando algumas partes precisam de características muito diferentes.

Por exemplo:

```text
Produtos → muita escalabilidade
Administração → mais segurança
Relatórios → muito processamento
```

---

## 3. Quanta de Arquitetura

O livro usa o conceito de **Quantum** para ajudar nessa decisão.

De forma simples:

> **Quantum é uma parte do sistema que precisa funcionar e ser implantada junto.**

### 1 Quantum

Tudo funciona junto:

```text
┌─────────────────────┐
│      Aplicação      │
│                     │
│ Produtos            │
│ Pedidos             │
│ Usuários            │
└─────────────────────┘
```

➡️ Geralmente representa um **monólito**.

### Vários Quanta

As partes funcionam separadamente:

```text
Produtos ──→ Serviço 1

Pedidos ───→ Serviço 2

Pagamentos ─→ Serviço 3
```

➡️ Geralmente representa uma arquitetura **distribuída**.

---

## 4. Isomorfismo de Domínio

O nome parece complicado, mas a ideia é simples:

> **A arquitetura deve acompanhar a forma como o negócio funciona.**

Imagine uma empresa de sanduíches:

```text
Cardápio
Estoque
Pedidos
Entrega
```

A arquitetura pode refletir essas áreas:

```text
Cardápio → módulo de Cardápio
Estoque  → módulo de Estoque
Pedidos  → módulo de Pedidos
Entrega  → módulo de Entrega
```

Se o negócio é simples, não precisamos criar uma arquitetura extremamente complexa.

---

## 5. Exemplo: Monólito Modular

Uma empresa pequena pode começar com um monólito, mas organizar o código em módulos:

```text
/backend
  ├── /products
  ├── /orders
  ├── /customers
  └── /payments
```

Tudo continua sendo **uma aplicação**, mas cada parte possui sua responsabilidade.

Isso facilita uma possível evolução no futuro.

---

## 6. BFF — Backend for Frontend

O **BFF** é uma API criada especificamente para um tipo de frontend.

Por exemplo:

```text
             Backend
                ↑
        ┌───────┴───────┐
        │               │
     BFF Web         BFF Mobile
        ↓               ↓
     React            App Mobile
```

Cada BFF entrega somente os dados que aquele frontend precisa.

### Exemplo simples

O backend possui:

```json
{
  "id": 10,
  "name": "X-Burger",
  "price": 25,
  "description": "...",
  "ingredients": [],
  "images": [],
  "nutrition": {},
  "reviews": []
}
```

O celular talvez precise apenas de:

```json
{
  "id": 10,
  "name": "X-Burger",
  "price": 25
}
```

O **BFF Mobile** pode pegar os dados completos e devolver somente o necessário.

> 💡 O BFF funciona como um **adaptador entre o backend e o frontend**.

---

## 7. Exemplo simples de BFF com Node.js

```javascript
app.get("/mobile/product/:id", async (req, res) => {
  const response = await axios.get(
    `http://api/products/${req.params.id}`
  );

  const product = response.data;

  res.json({
    id: product.id,
    name: product.name,
    price: product.price
  });
});
```

O backend possui muitos dados, mas o BFF entrega apenas o que o aplicativo precisa.

---

## 8. Estrutura do Projeto

Um exemplo simples utilizando React, Node.js e BFF:

```text
/sandwich-system
  │
  ├── /frontend
  │    ├── /web-react
  │    └── /mobile
  │
  ├── /bff
  │    ├── bff-web.js
  │    └── bff-mobile.js
  │
  └── /backend
       ├── /products
       ├── /orders
       └── /inventory
```

Fluxo:

```text
React Web
    ↓
BFF Web
    ↓
Backend

Mobile
    ↓
BFF Mobile
    ↓
Backend
```

---

## 9. Como escolher?

Não existe uma resposta única.

Podemos pensar assim:

| Situação | Boa opção |
|---|---|
| Projeto pequeno | **Camadas** |
| Sistema simples, mas organizado | **Monólito Modular** |
| Sistema que precisa de plugins | **Microkernel** |
| Sistema que precisa dividir algumas partes | **Baseada em Serviços** |
| Sistema que trabalha muito com eventos | **Orientada a Eventos** |
| Sistema muito grande e com alta necessidade de independência | **Microservices** |

### Comparação rápida

| Arquitetura | Custo | Simplicidade | Escalabilidade |
|---|---|---|---|
| Camadas | 💰 | ⭐⭐⭐⭐⭐ | ⭐ |
| Monólito Modular | 💰 | ⭐⭐⭐⭐⭐ | ⭐⭐ |
| Microkernel | 💰 | ⭐⭐⭐⭐ | ⭐ |
| Baseada em Serviços | 💰💰 | ⭐⭐⭐ | ⭐⭐⭐ |
| Eventos | 💰💰💰 | ⭐ | ⭐⭐⭐⭐⭐ |
| Microservices | 💰💰💰💰💰 | ⭐ | ⭐⭐⭐⭐⭐ |

---

## 10. Regra principal

Antes de escolher uma arquitetura, faça algumas perguntas:

```text
O sistema é grande?
       ↓
Precisa escalar partes diferentes?
       ↓
As partes precisam de deploy independente?
       ↓
Existe orçamento para uma infraestrutura maior?
       ↓
A equipe consegue lidar com essa complexidade?
```

Se a resposta for **não**, uma solução mais simples pode ser melhor.

---

## Resumo

O Capítulo 19 ensina que:

- **Não existe arquitetura perfeita.**
- Não devemos escolher uma arquitetura apenas porque está na moda.
- **Monólitos** são mais simples e baratos.
- **Arquiteturas distribuídas** oferecem mais independência, mas aumentam a complexidade.
- O **BFF** adapta o backend para as necessidades de cada frontend.
- A arquitetura deve fazer sentido para o **negócio e para as necessidades técnicas do sistema**.

> ### 💡 Regra de ouro
> **Comece com a solução mais simples que atende às necessidades do projeto.**
> 
> Só adicione complexidade quando realmente existir uma necessidade.