# Capítulo 21 — Decisões Arquiteturais

O **Capítulo 21**, intitulado **"Decisões Arquiteturais"** (*Architectural Decisions*), trata de como os arquitetos tomam, justificam e documentam decisões técnicas cruciais para o sistema.

O foco do capítulo é evitar escolhas arbitrárias e garantir que o **"porquê" de cada escolha** permaneça claro para toda a equipe ao longo do tempo.

---

## 1. Antipadrões em Decisões Arquiteturais

O livro destaca três erros muito comuns cometidos por equipes ao lidar com decisões de arquitetura:

* **Covering Your Assets (Protegendo as Costas):**

  * Ocorre quando o arquiteto evita ou adia tomar uma decisão por medo de errar.
  * A solução é esperar pelo **"último momento responsável"** (*last responsible moment*), que é o ponto ideal onde o custo de adiar a decisão começa a superar o risco de decidir sem dados suficientes.

* **Groundhog Day (O Dia da Marmota):**

  * Acontece quando a mesma decisão é discutida continuamente em reuniões sem fim porque a equipe não sabe **por que** a escolha original foi feita.

* **Email-Driven Architecture (Arquitetura Movida a E-mail):**

  * Ocorre quando decisões importantes são tomadas ou discutidas em threads de e-mail ou mensagens soltas.
  * Com o tempo, essas decisões podem se perder no histórico, principalmente conforme as pessoas entram ou saem da empresa.

---

## 2. O que torna uma decisão "Arquiteturalmente Significativa"?

Nem toda escolha técnica é uma decisão de arquitetura. Muitas são apenas decisões de design ou implementação de código.

Uma decisão é considerada **arquiteturalmente significativa** quando afeta:

1. **Estrutura**

   * Modifica a organização macro do sistema ou o isolamento dos componentes.

2. **Características não funcionais**

   * Impacta diretamente atributos como:

     * Performance
     * Escalabilidade
     * Segurança

3. **Dependências**

   * Define como as diferentes partes do sistema se conectam e o nível de acoplamento entre elas.

4. **Interfaces**

   * Define contratos, protocolos de comunicação ou gateways de acesso.

---

## 3. Registros de Decisão Arquitetural (ADRs)

Para evitar o antipadrão **Groundhog Day**, o livro recomenda registrar cada decisão importante utilizando **ADRs** (*Architectural Decision Records*).

A estrutura básica de um ADR contém as seguintes seções:

* **Título**

  * Um nome curto e numerado.
  * Exemplo: `ADR 05: Uso de React Query para Gerenciamento de Estado de API`

* **Status**

  * Representa o estado atual da decisão.
  * Exemplos:

    * `Proposed`
    * `Accepted`
    * `Superseded`
    * `RFC (Request for Comments)`

* **Contexto**

  * Explica o problema ou a necessidade de negócio que exige uma decisão.

* **Decisão**

  * Registra qual escolha foi feita e sua justificativa técnica.

* **Consequências**

  * Apresenta os impactos da decisão, incluindo os **trade-offs**:

    * Pontos positivos
    * Pontos negativos

* **Compliance (Governança)**

  * Define como a decisão será verificada e garantida.
  * Isso pode ser feito manualmente ou por meio de testes automatizados, como **fitness functions**.

---

## 4. Exemplo Prático de ADR em JavaScript / React

Um ADR pode ser criado como um arquivo `.md` dentro do projeto.

```md
# ADR 03: Adoção do TanStack Query (React Query) para Estado de Servidor

- **Status:** Accepted
- **Data:** 2026-09-15
- **Autor:** Equipe de Arquitetura Frontend

## Contexto

Nossa aplicação React precisa buscar dados de múltiplas APIs REST.

Estávamos armazenando dados de API dentro do Redux Toolkit global, o que gerou código repetitivo (boilerplate) para tratar estados de carregamento (loading), erro e cache, aumentando o acoplamento.

## Decisão

Decidimos adotar o **TanStack Query (React Query)** para gerenciar todo o estado de servidor:

- Buscas
- Cache
- Revalidação

O **Redux / Context API** será mantido apenas para estados de interface locais (UI) estritamente necessários.

## Consequências

### Positivas

- Redução de código boilerplate para busca de dados.
- Gerenciamento automatizado de cache.
- Revalidação em background.

### Negativas

- Adição de uma nova dependência ao projeto.
- Necessidade de treinamento da equipe na utilização do React Query.

## Compliance

Garantiremos o cumprimento desta decisão criando um teste com a ferramenta **TSArch** no build contínuo.

O objetivo é impedir que requisições HTTP (`axios` ou `fetch`) sejam chamadas diretamente dentro de componentes visuais React sem passar pelos custom hooks.
```

---

## 5. Exemplo de Estrutura de Projeto Frontend (React + ADRs)

O livro recomenda salvar os ADRs no próprio repositório de código do projeto, permitindo que a documentação evolua junto com o código.

Uma possibilidade é utilizar uma pasta `docs/adr/`:

```text
/my-react-app
│
├── /docs
│   └── /adr
│       ├── 0001-arquitetura-de-pasta-feature-based.md
│       ├── 0002-escolha-do-framework-css-tailwind.md
│       └── 0003-adocao-do-react-query.md
│
├── /src
│   ├── /assets
│   │   └── # Imagens e estilos globais
│   │
│   ├── /components
│   │   └── # Componentes UI reutilizáveis
│   │
│   ├── /features
│   │   ├── /auth
│   │   │   └── # Lógica e telas de autenticação
│   │   │
│   │   └── /checkout
│   │       ├── /api
│   │       │   └── # Custom Hooks do React Query
│   │       │
│   │       ├── /components
│   │       │   └── # Componentes locais do Checkout
│   │       │
│   │       └── CheckoutPage.jsx
│   │
│   ├── /services
│   │   └── # Instância do Axios / Configurações HTTP
│   │
│   └── App.jsx
│
├── package.json
└── README.md
```

---

## Resumo

A principal ideia do capítulo é que **decisões arquiteturais não devem existir apenas na cabeça dos desenvolvedores ou perdidas em conversas**.

Uma boa decisão arquitetural deve deixar claro:

* **Qual problema estamos tentando resolver?**
* **Quais opções foram consideradas?**
* **Qual decisão foi tomada?**
* **Por que essa decisão foi escolhida?**
* **Quais são as consequências e trade-offs?**
* **Como podemos garantir que essa decisão seja respeitada?**

> **A documentação da decisão é tão importante quanto a própria decisão**, porque permite que a equipe entenda não apenas *o que foi feito*, mas principalmente **por que foi feito daquela maneira**.
---

# Leitura

![alt text](image.png)