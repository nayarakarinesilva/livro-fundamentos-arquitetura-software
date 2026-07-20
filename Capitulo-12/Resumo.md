# Capítulo 12 – Estilo Arquitetural de Pipeline

A **Arquitetura de Pipeline**, também chamada de **Pipes and Filters**, organiza o sistema como uma sequência de etapas. Cada etapa faz apenas **uma única tarefa** e depois envia o resultado para a próxima.

Uma boa forma de imaginar esse estilo é pensar em uma **linha de produção de uma fábrica**.

Cada máquina faz apenas uma parte do trabalho até que, no final, o produto esteja pronto.

---

# 1. O que é a Arquitetura de Pipeline?

A arquitetura é formada por dois elementos principais:

## Filtros (Filters)

Os filtros são os componentes responsáveis por processar os dados.

Cada filtro executa apenas **uma responsabilidade**.

Exemplos:

- Validar dados
- Formatar informações
- Filtrar registros
- Converter valores

Depois de terminar seu trabalho, ele envia os dados para o próximo filtro.

> 💡 A ideia é que cada filtro faça apenas **uma tarefa**, seguindo o princípio da responsabilidade única (*Single Responsibility Principle*).

---

## Pipes (Tubos)

Os **Pipes** são os caminhos por onde os dados passam.

Eles apenas transportam as informações entre um filtro e outro.

O fluxo sempre acontece em um único sentido:

```text
Filtro A
    │
    ▼
Filtro B
    │
    ▼
Filtro C
```

Cada filtro recebe os dados, faz seu trabalho e envia o resultado para o próximo.

---

# 2. Tipos de Filtros

Existem quatro tipos principais.

## Producer (Produtor)

É o ponto de entrada do pipeline.

Sua função é fornecer os dados para iniciar o processamento.

Exemplo:

- Ler um arquivo.
- Receber uma requisição HTTP.
- Buscar dados de uma API.

---

## Transformer (Transformador)

Recebe um dado, modifica esse dado e envia o resultado para frente.

Exemplos:

- Converter texto para maiúsculo.
- Formatar uma data.
- Aplicar desconto em um produto.
- Converter moedas.

---

## Tester (Testador)

Verifica se os dados atendem a uma determinada regra.

Se atenderem, continuam no fluxo.

Caso contrário, podem ser descartados.

Exemplos:

- Apenas usuários maiores de 18 anos.
- Produtos em estoque.
- E-mails válidos.
- Pedidos pagos.

---

## Consumer (Consumidor)

É a última etapa do pipeline.

Recebe os dados já processados e faz alguma ação.

Exemplos:

- Salvar no banco de dados.
- Enviar um e-mail.
- Exibir na tela.
- Gerar um relatório.

---

# 3. Exemplo em JavaScript

Imagine uma lista de usuários.

Primeiro filtramos apenas os maiores de idade.

Depois formatamos o nome.

Por último salvamos os dados.

```javascript
const users = [
  { name: "ana", age: 17 },
  { name: "beto", age: 25 },
];

// TESTER
const filterAdults = (users) =>
  users.filter((user) => user.age >= 18);

// TRANSFORMER
const formatNames = (users) =>
  users.map((user) => ({
    ...user,
    name: user.name.toUpperCase(),
  }));

// CONSUMER
const saveUsers = (users) => {
  console.log("Dados salvos:", users);
};

// PIPELINE
const result = formatNames(filterAdults(users));

saveUsers(result);
```

### Fluxo

```text
Usuários
    │
    ▼
Filtrar maiores de idade
    │
    ▼
Colocar nomes em MAIÚSCULO
    │
    ▼
Salvar os dados
```

Cada função possui apenas uma responsabilidade.

---

# 4. Exemplo em React

No React, é comum criar pequenos "pipelines" antes da renderização.

```jsx
function ProductList({ products }) {
  // TESTER
  const availableProducts = products.filter(
    (product) => product.stock > 0
  );

  // TRANSFORMER
  const formattedProducts = availableProducts.map((product) => ({
    ...product,
    priceLabel: `R$ ${(product.price * 0.9).toFixed(2)}`,
  }));

  // CONSUMER
  return (
    <ul>
      {formattedProducts.map((product) => (
        <li key={product.id}>
          {product.name} - {product.priceLabel}
        </li>
      ))}
    </ul>
  );
}
```

### Fluxo

```text
Produtos
    │
    ▼
Filtrar produtos em estoque
    │
    ▼
Aplicar desconto
    │
    ▼
Renderizar na tela
```

Esse tipo de código é muito comum em aplicações React.

---

# 5. Exemplo do dia a dia

Imagine um cadastro de usuários.

O fluxo poderia ser:

```text
Usuário envia formulário
          │
          ▼
Validar campos
          │
          ▼
Verificar se o e-mail já existe
          │
          ▼
Criptografar senha
          │
          ▼
Salvar usuário
          │
          ▼
Enviar e-mail de boas-vindas
```

Cada etapa possui apenas uma responsabilidade.

---

# Características da Arquitetura de Pipeline

| Característica | Avaliação | Observação |
|----------------|-----------|------------|
| 💰 Custo Geral | ⭐ | Baixo custo para desenvolver e manter. |
| 😊 Simplicidade | ⭐⭐⭐⭐ | Fácil de entender e implementar. |
| 🧩 Modularidade | ⭐⭐⭐ | Cada filtro pode ser alterado sem afetar os demais. |
| 📈 Escalabilidade | ⭐ | Não é ideal para sistemas muito grandes. |
| 🧪 Testabilidade | ⭐⭐⭐ | Cada filtro pode ser testado separadamente. |
| 🚀 Facilidade de Deploy | ⭐⭐ | Geralmente toda a aplicação é publicada junta. |
| 🛡️ Tolerância a Falhas | ⭐ | Se um filtro falhar, o fluxo pode parar. |

---

# Vantagens

- Código organizado.
- Cada filtro possui apenas uma responsabilidade.
- Fácil de testar.
- Fácil de reutilizar filtros.
- Baixo acoplamento entre as etapas.
- Fácil de entender.

---

# Desvantagens

- Não é indicado para fluxos muito complexos.
- Pode haver perda de desempenho quando existem muitos filtros.
- Não é ideal para sistemas distribuídos.
- Um erro em um filtro pode interromper todo o processamento.

---

# Quando Utilizar?

✅ Processamento de arquivos.

✅ ETL (Extração, Transformação e Carga de dados).

✅ Processamento de imagens.

✅ Validação de dados.

✅ Fluxos bem definidos.

✅ Transformação de informações.

---

# Quando Evitar?

❌ Sistemas que precisam de muitas comunicações entre componentes.

❌ Aplicações distribuídas muito complexas.

❌ Fluxos com muitas decisões e retornos.

❌ Sistemas que exigem alta escalabilidade.

---

# Resumo

A **Arquitetura de Pipeline (Pipes and Filters)** organiza o processamento em uma sequência de etapas independentes.

Cada **Filtro (Filter)** possui apenas uma responsabilidade, enquanto os **Pipes** transportam os dados de uma etapa para outra.

Esse estilo facilita a organização, os testes e a reutilização do código, além de tornar o fluxo de processamento mais simples de entender.

É uma ótima escolha para aplicações que processam dados passo a passo, como validações, transformações de informações, processamento de arquivos e fluxos de cadastro. Porém, não é a melhor opção para sistemas muito grandes ou que exigem comunicação complexa entre vários componentes.