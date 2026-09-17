# Capítulo 23 — Criação de Diagramas de Arquitetura

O **Capítulo 23 — Criação de Diagramas de Arquitetura** (_Diagramming Architecture_) fala sobre a importância da **comunicação visual na arquitetura**.

Não basta criar uma boa arquitetura: é necessário conseguir **explicá-la de forma clara** para desenvolvedores e gestores.

---

## 1. Consistência Representacional

Ao explicar uma arquitetura, devemos começar pela **visão geral** e depois detalhar partes específicas.

> Primeiro mostramos **onde algo está no sistema** e depois mostramos **como funciona internamente**.

Exemplo:

```text
E-commerce
│
├── Catálogo
├── Carrinho
└── Checkout
        ↓
   CheckoutForm
   useCheckout
   PaymentService
```

Assim, a pessoa entende o contexto antes de entrar nos detalhes.

---

## 2. Principais Padrões

### UML

Ainda pode ser útil para alguns tipos de diagramas, principalmente:

- **Diagrama de Classes**
- **Diagrama de Sequência**

### C4

O **C4** organiza a arquitetura em níveis de detalhe:

```text
Contexto
   ↓
Container
   ↓
Componente
   ↓
Código
```

Exemplo em uma aplicação React:

- **Contexto:** Usuário → E-commerce
- **Container:** React → API → Banco de Dados
- **Componente:** Catalog → Checkout → Auth
- **Código:** Componentes, hooks e serviços

### ArchiMate

Padrão utilizado principalmente para representar **arquiteturas corporativas**.

---

## 3. Boas Práticas para Diagramas

- Use **títulos claros**.
- Não dependa apenas de **cores** para diferenciar elementos.
- Utilize **legendas** quando houver símbolos ou tipos de linhas diferentes.
- Prefira ferramentas simples e fáceis de manter, como:
  - Mermaid
  - PlantUML
  - Draw.io

---

## 4. Exemplo com React

Uma arquitetura simples pode ser representada assim:

```text
Usuário
   ↓
React SPA
   ↓
API
   ↓
Banco de Dados
```

E, dentro do React:

```text
React
│
├── Auth
├── Catalog
├── Cart
└── Checkout
      ├── CheckoutForm
      └── useCheckout
```

---

## Resumo

A principal ideia do capítulo é:

> **Um bom arquiteto precisa saber comunicar a arquitetura, não apenas projetá-la.**

Os diagramas devem:

- Mostrar primeiro o **contexto** e depois os detalhes.
- Usar níveis de abstração adequados.
- Ser simples e fáceis de entender.
- Ajudar a equipe a compreender **como as partes do sistema se relacionam**.

O **C4** é uma das principais abordagens apresentadas para organizar essa comunicação visual.

---

# Leitura

![alt text](image.png)
