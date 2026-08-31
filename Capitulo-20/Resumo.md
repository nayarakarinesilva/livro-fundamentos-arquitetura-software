# Capítulo 20 — Padrões Arquiteturais

O **Capítulo 20** apresenta vários **padrões arquiteturais** usados para resolver problemas específicos de um sistema.

Uma forma simples de diferenciar:

- **Estilo arquitetural:** define a estrutura geral do sistema.
- **Padrão arquitetural:** resolve um problema específico dentro dessa estrutura.

Por exemplo:

```text
Estilo: Microserviços
        ↓
Padrão: Hexagonal
        ↓
Padrão: CQRS
        ↓
Padrão: Sidecar
```

---

## 1. Arquitetura Hexagonal

A **Arquitetura Hexagonal (Ports and Adapters)** coloca as **regras de negócio no centro**.

A ideia é que o negócio não dependa diretamente de:

- Banco de dados;
- React;
- APIs;
- Frameworks.

```text
        React
          ↓
     [ Adaptador ]
          ↓
       [ Porta ]
          ↓
   REGRAS DE NEGÓCIO
          ↓
       [ Porta ]
          ↓
     [ Adaptador ]
          ↓
      Banco de Dados
```

### Exemplo simples

Imagine uma regra para calcular o desconto de um pedido.

A regra de negócio pode ser:

```javascript
function calculateDiscount(total) {
  return total >= 100 ? 10 : 0;
}
```

Essa regra não precisa saber se os dados vieram do:

```text
MongoDB
MySQL
API
Arquivo
```

Isso facilita trocar tecnologias sem precisar alterar a regra de negócio.

### Resumindo

> **O negócio fica no centro e as tecnologias ficam nas bordas.**

---

## 2. Sidecar e Service Mesh

Esses padrões são muito usados em sistemas com vários serviços.

### Sidecar

O **Sidecar** é um serviço que fica ao lado da aplicação e cuida de tarefas técnicas.

```text
┌─────────────────┐
│   Aplicação     │
│   Node.js       │
└─────────────────┘
        +
┌─────────────────┐
│    Sidecar      │
│ Logs / Segurança│
└─────────────────┘
```

Assim, a aplicação não precisa ter todo esse código dentro dela.

### Service Mesh

Quando temos muitos Sidecars, o **Service Mesh** ajuda a controlar a comunicação entre eles.

```text
Service A
    ↕
Service Mesh
    ↕
Service B
```

> 💡 **Sidecar:** ajuda um serviço.  
> **Service Mesh:** ajuda a controlar a comunicação entre vários serviços.

---

## 3. Orquestração e Coreografia

São duas formas de fazer serviços trabalharem juntos.

### Orquestração

Existe um serviço que funciona como um **maestro**.

```text
          Orquestrador
          /    |    \
         ↓     ↓     ↓
    Pagamento Estoque Entrega
```

Ele decide a ordem das operações.

Exemplo:

```text
1. Criar pedido
2. Fazer pagamento
3. Reservar estoque
4. Enviar pedido
```

### Coreografia

Não existe um maestro.

Os serviços reagem a eventos.

```text
Pedido criado
     ↓
Pagamento
     ↓
Pagamento aprovado
     ↓
Estoque
     ↓
Estoque reservado
```

Cada serviço decide o que fazer quando recebe um evento.

### Resumindo

| Orquestração | Coreografia |
|---|---|
| Tem um controlador | Não tem controlador central |
| Mais fácil acompanhar o fluxo | Mais desacoplada |
| Bom para fluxos complexos | Bom para eventos |

---

## 4. CQRS

**CQRS** significa separar:

- **Command:** altera os dados.
- **Query:** consulta os dados.

Exemplo:

```text
Command
   ↓
Criar pedido

Query
   ↓
Consultar pedidos
```

Em uma aplicação comum:

```javascript
createOrder();
getOrders();
```

Com CQRS, pensamos nessas operações como responsabilidades diferentes.

### Quando usar?

Principalmente quando temos **muito mais leitura do que escrita**.

Exemplo:

```text
100.000 consultas de produtos
        ↓
      poucas
   alterações
```

Podemos otimizar a parte de leitura sem precisar otimizar a escrita da mesma forma.

> 💡 **CQRS = separar quem altera os dados de quem consulta os dados.**

---

## 5. Estrutura de Projeto

Um projeto utilizando Hexagonal pode ser organizado assim:

```text
/my-app
  │
  ├── /domain
  │    ├── entities
  │    └── services
  │
  ├── /ports
  │    ├── databasePort.js
  │    └── paymentPort.js
  │
  ├── /adapters
  │    ├── /react
  │    ├── mongoAdapter.js
  │    └── paymentAdapter.js
  │
  └── package.json
```

A ideia é:

```text
React
  ↓
Adapter
  ↓
Port
  ↓
Regra de negócio
  ↓
Port
  ↓
Adapter
  ↓
Banco / API
```

---

## 6. Principais Padrões

| Padrão | Principal vantagem | Quando usar? |
|---|---|---|
| **Hexagonal** | Facilita testes e mudanças | Quando o negócio não deve depender da tecnologia |
| **Sidecar** | Separa tarefas técnicas | Logs, segurança e monitoramento |
| **Service Mesh** | Controla a comunicação | Muitos microserviços |
| **Orquestração** | Facilita controlar fluxos | Processos com várias etapas |
| **Coreografia** | Reduz o acoplamento | Sistemas baseados em eventos |
| **CQRS** | Melhora leitura e escalabilidade | Quando existem muitas consultas |
| **Domain-Broker** | Ajuda no isolamento | Sistemas que usam filas e mensagens |

---

## 7. Resumo

Os principais conceitos do capítulo são:

```text
Hexagonal
→ Separa negócio de tecnologia

Sidecar
→ Coloca tarefas técnicas fora da aplicação

Service Mesh
→ Controla a comunicação entre serviços

Orquestração
→ Um serviço controla o fluxo

Coreografia
→ Serviços reagem a eventos

CQRS
→ Separa leitura de escrita
```

### 💡 Ideia principal

> **Padrões arquiteturais são soluções que podemos aplicar para resolver problemas específicos do sistema.**

Eles não precisam ser usados todos juntos. A escolha depende do problema que queremos resolver.