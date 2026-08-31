# Capítulo 17 — SOA baseada em Orquestração

O **SOA (Service-Oriented Architecture)** é um estilo de arquitetura que ficou muito popular principalmente nos anos 1990 e início dos anos 2000.

Naquela época, servidores, computadores e softwares eram caros. Além disso, muitas empresas estavam crescendo por meio de fusões e aquisições.

Por isso, as empresas buscavam principalmente:

- **Reutilizar código**;
- **Compartilhar serviços** entre vários sistemas;
- **Centralizar a comunicação** entre as aplicações.

Para fazer isso, o SOA utilizava uma peça central chamada **ESB (Enterprise Service Bus)**.

> 💡 Pense no ESB como um **"gerente"** que recebe uma solicitação e decide quais serviços precisam ser chamados e em qual ordem.

---

## 1. Como funciona o SOA?

Imagine uma **loja online**.

Quando um cliente faz uma compra, várias coisas precisam acontecer:

1. Buscar os dados do cliente;
2. Calcular o preço;
3. Gerar a nota ou recibo;
4. Registrar tudo nos logs.

No SOA, essas responsabilidades ficam separadas em diferentes serviços.

O **ESB** fica no meio coordenando tudo:

```text
Cliente
   ↓
  ESB
   ↓
┌───────────────┐
│ Buscar cliente│
└───────────────┘
        ↓
┌───────────────┐
│ Calcular preço│
└───────────────┘
        ↓
┌───────────────┐
│ Gerar recibo  │
└───────────────┘
```

O ESB decide:

> "Primeiro vou buscar o cliente, depois calcular o preço e, por último, gerar o recibo."

---

# 2. Os 4 tipos de serviços

No SOA, os serviços eram organizados em **quatro tipos principais**.

## 2.1 Serviços de Negócio (Business Services)

São uma espécie de **contrato** que descreve o que o sistema precisa fazer.

Eles não são necessariamente responsáveis pela implementação do código.

Por exemplo:

```text
"Calcular o preço de uma compra"
```

O serviço de negócio define **o que precisa ser feito**, mas não necessariamente **como será feito**.

---

## 2.2 Serviços Corporativos (Enterprise Services)

São serviços que podem ser **reutilizados por vários sistemas da empresa**.

Por exemplo:

```text
BuscarCliente
CalcularPreco
ConsultarEstoque
```

Imagine que a empresa possui:

- Site;
- Aplicativo;
- Sistema interno;
- Sistema de atendimento.

Todos eles podem utilizar o mesmo serviço:

```text
              ┌── Site
              │
              ├── Aplicativo
BuscarCliente ┤
              ├── Sistema interno
              │
              └── Atendimento
```

A ideia era evitar criar a mesma funcionalidade várias vezes.

---

## 2.3 Serviços de Aplicação (Application Services)

São serviços específicos de uma determinada aplicação.

Por exemplo, o site da empresa pode ter:

```text
GerarReciboDoSite
```

Enquanto o aplicativo pode ter:

```text
GerarReciboDoAplicativo
```

Eles possuem regras específicas daquela aplicação.

---

## 2.4 Serviços de Infraestrutura (Infrastructure Services)

São serviços usados para tarefas técnicas.

Por exemplo:

```text
Logging
Autenticação
Monitoramento
Auditoria
Segurança
```

Um exemplo simples:

```javascript
class LoggingService {
  log(message) {
    console.log(`[LOG]: ${message}`);
  }
}
```

Sempre que algum serviço precisar registrar alguma informação, pode utilizar o serviço de logging.

---

# 3. O ESB: o "cérebro" da arquitetura

O **ESB (Enterprise Service Bus)** é uma das principais características do SOA baseado em orquestração.

Ele funciona como um **orquestrador central**.

Por exemplo, para realizar uma compra:

```text
Cliente
   ↓
 ESB
   │
   ├──→ Buscar cliente
   │
   ├──→ Calcular preço
   │
   └──→ Gerar recibo
```

O ESB controla:

- Qual serviço será chamado;
- A ordem das chamadas;
- A comunicação entre os serviços;
- Transformações de dados;
- Tratamento de algumas integrações.

### Exemplo simples

Imagine que o cliente faça:

```text
Comprar livro
```

O ESB pode executar:

```text
1. Buscar cliente
        ↓
2. Verificar categoria do cliente
        ↓
3. Calcular desconto
        ↓
4. Gerar recibo
```

O ponto importante é que **o fluxo fica centralizado no ESB**.

---

# 4. O problema do excesso de reuso

No começo, reutilizar serviços parecia uma ótima ideia.

E realmente é útil.

O problema aparece quando **todo mundo depende exatamente do mesmo serviço**.

Imagine que exista um serviço compartilhado:

```javascript
{
  id: 1,
  name: "Maria",
  email: "maria@email.com"
}
```

Vários sistemas começam a depender desse formato:

```text
Site ──────────┐
               │
Aplicativo ────┼──→ Serviço de Cliente
               │
Sistema interno┘
```

Agora imagine que alguém altere:

```javascript
name
```

para:

```javascript
fullName
```

Todos os sistemas que esperavam `name` podem parar de funcionar.

Isso gera um **forte acoplamento**.

---

# 5. Reuso x Acoplamento

Esse é um dos principais problemas apresentados pelo SOA.

Podemos pensar assim:

```text
Mais reuso
    ↓
Mais sistemas dependentes
    ↓
Mais acoplamento
    ↓
Mais difícil alterar o serviço
```

Ou seja:

> **Reutilizar demais também pode ser um problema.**

### Exemplo

Imagine que 20 sistemas utilizem:

```text
ClienteService
```

Você precisa alterar esse serviço.

Agora precisa verificar:

```text
ClienteService
      ↓
 ┌────┼────┬────┐
 ↓    ↓    ↓    ↓
Site App  ERP  ...
```

Uma pequena alteração pode afetar muitos sistemas.

Por isso, mudanças que deveriam ser simples podem acabar exigindo:

- Muitos testes;
- Alterações em vários sistemas;
- Deploys coordenados;
- Comunicação entre várias equipes.

---

# 6. Exemplo simples em JavaScript

Podemos representar uma versão simplificada do SOA usando JavaScript.

## Serviço de cliente

```javascript
class CustomerService {
  getCustomer(id) {
    return {
      id,
      name: "Maria",
      category: "VIP"
    };
  }
}
```

Esse serviço é responsável por buscar o cliente.

---

## Serviço de preço

```javascript
class PricingService {
  calculate(price, category) {
    if (category === "VIP") {
      return price * 0.9;
    }

    return price;
  }
}
```

Esse serviço calcula o preço.

---

## Serviço de recibo

```javascript
class ReceiptService {
  generate(customer, total) {
    return {
      customer: customer.name,
      total
    };
  }
}
```

Esse serviço gera o recibo.

---

## Orquestrador

Agora temos o serviço que coordena tudo:

```javascript
class ESB {
  constructor() {
    this.customerService = new CustomerService();
    this.pricingService = new PricingService();
    this.receiptService = new ReceiptService();
  }

  checkout(customerId, price) {
    // 1. Busca o cliente
    const customer = this.customerService.getCustomer(customerId);

    // 2. Calcula o preço
    const total = this.pricingService.calculate(
      price,
      customer.category
    );

    // 3. Gera o recibo
    return this.receiptService.generate(
      customer,
      total
    );
  }
}
```

Uso:

```javascript
const esb = new ESB();

const receipt = esb.checkout(1, 100);

console.log(receipt);
```

Resultado:

```javascript
{
  customer: "Maria",
  total: 90
}
```

O fluxo foi:

```text
ESB
 ↓
Buscar cliente
 ↓
Calcular preço
 ↓
Gerar recibo
```

Esse é o conceito principal da **orquestração**.

---

# 7. E onde entra o React?

Podemos entender o problema de acoplamento usando um exemplo simples no React.

Imagine um componente compartilhado:

```jsx
function CustomerCard({ customer }) {
  return (
    <div>
      <h2>{customer.name}</h2>
      <p>{customer.email}</p>
    </div>
  );
}
```

Vários sistemas utilizam esse componente.

Agora o backend muda:

```javascript
{
  name: "Maria"
}
```

para:

```javascript
{
  fullName: "Maria"
}
```

O componente ainda espera:

```javascript
customer.name
```

Então:

```jsx
<h2>{customer.name}</h2>
```

não funcionará corretamente.

---

## Como evitar isso?

Uma alternativa é criar um **adaptador**.

Por exemplo:

```javascript
function adaptCustomer(customer) {
  return {
    name: customer.fullName,
    email: customer.email
  };
}
```

Assim, o componente continua esperando:

```javascript
customer.name
```

e não precisa conhecer os detalhes do backend.

```text
API
 ↓
Adaptador
 ↓
Componente React
```

Isso reduz o acoplamento entre a API e a interface.

---

# 8. Estrutura de um projeto SOA

Uma estrutura simplificada poderia ser:

```text
soa-project/
│
├── esb/
│   ├── esb.js
│   └── orderWorkflow.js
│
├── services/
│   │
│   ├── enterprise/
│   │   ├── customerService.js
│   │   └── pricingService.js
│   │
│   ├── application/
│   │   └── checkoutService.js
│   │
│   └── infrastructure/
│       ├── loggingService.js
│       └── securityService.js
│
└── database/
```

Podemos pensar assim:

```text
ESB
 │
 ├── Enterprise Services
 │     ├── Cliente
 │     └── Preço
 │
 ├── Application Services
 │     └── Checkout
 │
 └── Infrastructure Services
       ├── Logs
       └── Segurança
```

---

# 9. Principais vantagens

O SOA trouxe algumas vantagens importantes:

### ♻️ Reutilização

Um serviço pode ser utilizado por vários sistemas.

```text
Site ───────┐
App ────────┼──→ ClienteService
ERP ────────┘
```

### Separação

As responsabilidades são divididas em serviços diferentes.

### Centralização

O ESB controla a comunicação entre os serviços.

### Integração

Sistemas diferentes conseguem se comunicar através dos serviços.

---

# 10. Principais problemas

Apesar das vantagens, o SOA também possui alguns problemas.

### Alto acoplamento

Muitos sistemas podem depender dos mesmos serviços.

### Mudanças mais difíceis

Uma alteração em um serviço compartilhado pode afetar vários sistemas.

### Testes complexos

Pode ser necessário testar várias partes do sistema juntas.

### Deploy complicado

Uma mudança pode exigir que vários sistemas sejam atualizados.

### Complexidade

O ESB pode se tornar muito grande e difícil de manter.

---

# 11. Características do SOA

| Característica | Avaliação | Explicação |
|---|---|---|
| **Custo** | 💰💰💰💰💰 | Pode ser caro devido à infraestrutura e ferramentas utilizadas |
| **Particionamento** | Técnico | Os serviços são separados principalmente por responsabilidades técnicas |
| **Simplicidade** | ⭐ | Possui bastante complexidade |
| **Modularidade** | ⭐⭐ | Existe separação, mas o forte compartilhamento aumenta o acoplamento |
| **Testabilidade** | ⭐ | Testar tudo pode ser complicado |
| **Deploy** | ⭐ | Mudanças podem afetar vários sistemas |
| **Performance** | ⭐⭐ | Muitas chamadas podem passar pelo ESB |
| **Escalabilidade** | ⭐⭐⭐ | Pode escalar, mas o ESB pode se tornar um ponto importante da arquitetura |

---

# 12. Resumindo

Podemos resumir o **SOA baseado em orquestração** desta forma:

```text
                 ┌──────────────┐
                 │     ESB      │
                 │ Orquestrador │
                 └──────┬───────┘
                        │
          ┌─────────────┼─────────────┐
          ↓             ↓             ↓
     ClienteService PricingService ReceiptService
```

O **ESB coordena os serviços** e decide a ordem em que eles serão executados.

A grande ideia do SOA era:

> **"Crie serviços reutilizáveis e faça vários sistemas utilizarem esses serviços."**

O problema é que, quando o compartilhamento é exagerado:

> **"Quanto mais sistemas dependem do mesmo serviço, mais difícil fica mudar esse serviço."**

Por isso, um dos principais aprendizados do capítulo é:

### Reuso é bom, mas reuso excessivo pode gerar acoplamento.

E esse problema ajudou a motivar arquiteturas mais modernas, como os **microserviços**, que procuram dar mais autonomia para cada serviço.