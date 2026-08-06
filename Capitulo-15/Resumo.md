# Capítulo 15 - Arquitetura Orientada a Eventos (EDA - Event-Driven Architecture)

O **EDA (Event-Driven Architecture)** é um estilo de arquitetura distribuída e assíncrona. Em vez de um sistema esperar uma solicitação direta, ele reage aos acontecimentos (eventos).

---

# 1. O conceito: Arquitetura da "Reação"

Uma forma simples de entender é comparando dois cenários.

### Baseada em Pedido (Request)

Você faz um pedido e espera uma resposta.

**Exemplo:**
- Você pede um prato em um restaurante.
- O garçom leva o pedido.
- A cozinha prepara.
- O garçom entrega.

➡️ Existe uma comunicação direta entre quem pede e quem responde.

---

### Baseada em Evento (Event)

Alguém faz alguma ação e o sistema reage automaticamente.

**Exemplo:**
- Um cliente faz um lance em um leilão.
- O sistema atualiza o valor.
- Todos os participantes são avisados.
- O cronômetro pode ser reiniciado.

➡️ Ninguém precisou pedir essas ações diretamente. Elas aconteceram porque um evento ocorreu.

---

# Principais componentes

### Evento Iniciador

É o acontecimento que inicia todo o processo.

**Exemplo:**

```
"Cliente comprou um livro"
```

---

### Broker (Mensageiro)

É o responsável por transportar os eventos entre os sistemas.

Exemplos:

- RabbitMQ
- Apache Kafka

---

### Processador de Eventos

É quem escuta o evento e executa alguma ação.

**Exemplo:**

- Emitir Nota Fiscal
- Atualizar estoque
- Enviar e-mail

---

### Evento Derivado

É um novo evento criado depois que outro foi processado.

**Exemplo:**

```
"Nota Fiscal Emitida"
```

---

# 2. Evento x Mensagem

## Evento

Um evento informa que algo **já aconteceu**.

Exemplo:

```
Pedido Criado
```

Ele apenas comunica o ocorrido.

---

## Mensagem

Uma mensagem normalmente é um comando.

Exemplo:

```
Crie este pedido
```

Nesse caso existe alguém esperando executar aquela tarefa.

---

# 3. Exemplo em JavaScript (Node.js)

O Node.js possui um sistema de eventos chamado **EventEmitter**.

```javascript
import { EventEmitter } from "events";

const eventBus = new EventEmitter();

// Consumidor do evento
eventBus.on("PEDIDO_CRIADO", (pedido) => {
  console.log(
    `[Email] Enviando confirmação para ${pedido.email}`
  );

  // Novo evento
  eventBus.emit("EMAIL_ENVIADO", {
    id: pedido.id,
  });
});

// Evento inicial
const novoPedido = {
  id: 123,
  email: "cliente@email.com",
};

console.log("Iniciando processo...");

eventBus.emit("PEDIDO_CRIADO", novoPedido);
```

### Fluxo

```
Pedido Criado
       ↓
Serviço de E-mail recebe
       ↓
Envia confirmação
       ↓
Dispara novo evento
       ↓
Email Enviado
```

---

# 4. Exemplo em React

Os componentes podem se comunicar usando eventos sem depender diretamente uns dos outros.

### Componente que dispara o evento

```javascript
function BuyButton({ product }) {
  const handleBuy = () => {
    const event = new CustomEvent("PRODUCT_ADDED", {
      detail: product,
    });

    window.dispatchEvent(event);
  };

  return (
    <button onClick={handleBuy}>
      Comprar
    </button>
  );
}
```

---

### Componente que escuta o evento

```javascript
function CartIcon() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const updateCart = () =>
      setCount((prev) => prev + 1);

    window.addEventListener(
      "PRODUCT_ADDED",
      updateCart
    );

    return () =>
      window.removeEventListener(
        "PRODUCT_ADDED",
        updateCart
      );
  }, []);

  return <span>🛒 {count}</span>;
}
```

### Fluxo

```
Botão Comprar
      ↓
Dispara PRODUCT_ADDED
      ↓
Carrinho recebe o evento
      ↓
Atualiza quantidade
```

---

# 5. Exemplo de estrutura do projeto

Uma organização simples pode ser:

```
src/
│
├── modules/
│   ├── checkout/
│   │   └── CheckoutProducer.js
│   │
│   ├── inventory/
│   │   └── InventoryConsumer.js
│   │
│   └── notifications/
│       └── EmailConsumer.js
│
└── shared/
    ├── event-bus/
    └── contracts/
```

### O que faz cada pasta?

**checkout**

Dispara o evento.

Exemplo:

```
ORDER_PLACED
```

---

**inventory**

Escuta o evento e reduz o estoque.

---

**notifications**

Escuta o mesmo evento e envia um e-mail.

---

**event-bus**

Configuração do sistema de eventos (EventEmitter, RabbitMQ, Kafka etc.).

---

**contracts**

Define o formato dos eventos enviados entre os sistemas.

---

# 6. Características da EDA

| Característica | Avaliação | Explicação |
|----------------|-----------|------------|
| **Custo** | $$$ | Precisa de infraestrutura de mensagens. |
| **Simplicidade** | ⭐ | Pode ser difícil entender todo o fluxo. |
| **Escalabilidade** | ⭐⭐⭐⭐⭐ | Suporta muitos eventos ao mesmo tempo. |
| **Performance** | ⭐⭐⭐⭐⭐ | Processamento rápido e assíncrono. |
| **Tolerância a Falhas** | ⭐⭐⭐⭐⭐ | Se um serviço estiver indisponível, o evento pode ficar na fila até ele voltar. |
| **Testabilidade** | ⭐⭐ | Os testes podem ser mais difíceis porque o fluxo não é linear. |
| **Modularidade** | ⭐⭐⭐⭐ | Os serviços ficam independentes uns dos outros. |

---

# Vantagens

- Baixo acoplamento entre os serviços.
- Fácil adicionar novos consumidores.
- Alta escalabilidade.
- Boa performance.
- Maior tolerância a falhas.

---

# Desvantagens

- Mais difícil de entender o fluxo.
- Debug mais complexo.
- Testes mais difíceis.
- Infraestrutura mais cara.

---

# Quando usar?

Use EDA quando:

- houver muitos usuários ao mesmo tempo;
- vários sistemas precisarem reagir ao mesmo evento;
- o negócio funcionar por acontecimentos.

### Exemplos

- Marketplace
- Redes sociais
- Leilões online
- Monitoramento de sensores (IoT)
- Sistemas bancários
- Rastreamento de pedidos

---

# Quando não usar?

Evite EDA em aplicações simples.

Exemplos:

- Tela de login.
- Cadastro de usuários.
- Consultar dados e mostrar na tela.
- Sistemas pequenos com poucos usuários.

Nesses casos, uma arquitetura baseada em requisições (Request/Response) costuma ser mais simples e suficiente.