## Capítulo 16 — Arquitetura Baseada em Espaço

A **Arquitetura Baseada em Espaço (Space-Based Architecture)** é um estilo de arquitetura criado para resolver problemas de **escalabilidade** e de **muitos acessos ao mesmo tempo**.

O nome vem do conceito de **"espaço de tuplas" (tuple space)**.

A ideia principal é **não depender do banco de dados para todas as operações**. Em vez disso, os dados usados com frequência ficam na **memória (RAM)**, permitindo que o sistema processe as informações mais rapidamente.

---

## 1. O conceito: tirar o banco de dados do caminho principal

Em uma aplicação tradicional, muitos usuários podem acessar o sistema ao mesmo tempo e fazer operações no banco de dados.

Por exemplo:

```text
Milhões de usuários
        ↓
     Sistema
        ↓
Banco de dados
        ↓
   Sobrecarga
```

Nesse caso, o banco de dados pode se tornar um **gargalo**, ou seja, a parte que limita a velocidade do sistema.

Na Arquitetura Baseada em Espaço, os dados necessários ficam na **memória das unidades de processamento**.

```text
Usuário
   ↓
Unidade de Processamento
   ↓
Dados na memória
```

O banco de dados é atualizado depois, de forma **assíncrona**, sem precisar fazer parte do caminho principal da requisição.

### Principais componentes

#### Unidade de Processamento (Processing Unit — PU)

É onde ficam:

- A lógica da aplicação.
- Os dados usados naquele momento.
- Um cache em memória.

A PU consegue processar as informações rapidamente porque não precisa consultar o banco de dados o tempo todo.

#### Middleware Virtualizado

Funciona como um **coordenador** entre as unidades.

Ele:

- Recebe as requisições.
- Decide para qual unidade enviar cada requisição.
- Ajuda a manter os dados sincronizados entre as unidades.

#### Data Pump

É responsável por enviar as alterações para o banco de dados.

Esse processo acontece **em segundo plano**, para não deixar o usuário esperando.

---

## 2. Exemplo em JavaScript (Node.js)

Imagine um sistema de **leilão online**.

O preço pode mudar várias vezes por segundo. Se o sistema precisasse salvar cada lance diretamente no banco de dados, o banco poderia ficar sobrecarregado.

Em vez disso, o preço atual fica na memória:

```js
const processingUnit = {
  internalCache: {
    currentBid: 100,
  },

  handleBid(newBid) {
    if (newBid > this.internalCache.currentBid) {
      this.internalCache.currentBid = newBid;

      // Envia a alteração para o Data Pump
      dataPump.push({
        type: 'UPDATE_BID',
        value: newBid,
      });

      return 'Lance aceito!';
    }
  },
};

const dataPump = {
  push(data) {
    setTimeout(() => {
      console.log('Banco de dados atualizado:', data);
    }, 1000);
  },
};
```

Nesse exemplo:

1. O lance chega.
2. O sistema verifica o valor na memória.
3. Se o lance for maior, atualiza o valor na memória.
4. O usuário recebe a resposta rapidamente.
5. O `Data Pump` envia a alteração para o banco depois.

### Ideia principal

**O usuário não precisa esperar o banco de dados para receber a resposta.**

---

## 3. Exemplo em React

No frontend, podemos pensar em um conceito parecido usando um **estado ou cache local**.

Por exemplo:

- Redux.
- Context API.
- Zustand.

Imagine um dashboard que mostra preços de ações.

O frontend pode receber atualizações por **WebSocket** e atualizar a tela rapidamente, sem precisar consultar o banco de dados a cada alteração.

Uma representação simples seria:

```text
Servidor
   ↓
WebSocket
   ↓
Estado no React
   ↓
Interface atualizada
```

> **Importante:** isso é apenas uma comparação para facilitar o entendimento. A Arquitetura Baseada em Espaço é uma arquitetura de sistema muito maior do que simplesmente usar Redux, Context ou Zustand.

---

## 4. Estrutura de um projeto

Uma estrutura poderia separar as responsabilidades dessa forma:

```text
/space-based-system
│
├── /middleware
│   ├── messaging-grid.js   # Encaminha as requisições
│   └── data-grid.js        # Sincroniza os dados entre as unidades
│
├── /processing-units
│   ├── /inventory-pu       # Lógica + cache de estoque
│   └── /bidding-pu         # Lógica + cache de lances
│
├── /data-conduits
│   ├── data-pump.js        # Envia alterações para o banco
│   └── data-reader.js      # Carrega os dados ao iniciar
│
└── /database               # Armazena os dados permanentemente
```

A ideia é separar:

```text
Processamento rápido → Memória

Persistência → Banco de dados
```

---

## 5. Características desse estilo

A Arquitetura Baseada em Espaço é muito boa para sistemas que precisam suportar **muitos usuários e muitas operações ao mesmo tempo**.

| Característica | Classificação | Explicação |
|---|---|---|
| **Custo** | $$$$$ | Muito alto devido à infraestrutura e, em alguns casos, ferramentas específicas. |
| **Simplicidade** | ⭐ | É complexa porque é necessário controlar várias unidades e manter os dados sincronizados. |
| **Escalabilidade** | ⭐⭐⭐⭐⭐ | Consegue lidar com uma quantidade muito grande de usuários. |
| **Elasticidade** | ⭐⭐⭐⭐⭐ | É possível adicionar ou remover unidades conforme a quantidade de acessos muda. |
| **Performance** | ⭐⭐⭐⭐⭐ | O processamento acontece principalmente em memória, que é muito rápida. |
| **Testabilidade** | ⭐ | Pode ser difícil testar vários servidores, sincronização e possíveis falhas. |

---

## 6. Quando usar?

Esse estilo faz mais sentido quando o sistema precisa lidar com **carga muito alta**, por exemplo:

- Venda de ingressos para eventos muito grandes.
- Grandes sistemas de leilão online.
- Sistemas financeiros com muitas operações simultâneas.
- Aplicações que precisam responder rapidamente para milhões de usuários.

Para aplicações menores e mais comuns, provavelmente **não vale a pena usar essa arquitetura**, porque a complexidade e o custo podem ser maiores do que o benefício.

---

## Resumo para lembrar

A ideia principal do capítulo é:

> **"Em vez de fazer o banco de dados trabalhar o tempo todo, coloque os dados necessários na memória e processe rapidamente. Depois, envie as alterações para o banco."**

### Em uma frase

**Space-Based Architecture = processamento em memória + várias unidades de processamento + sincronização + banco atualizado em segundo plano.**

### Principal problema que ela resolve

**Muitos usuários acessando o sistema ao mesmo tempo e sobrecarregando o banco de dados.**