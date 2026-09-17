# Capítulo 27 — As Leis da Arquitetura de Software — Revisitadas

O último capítulo revisita os principais princípios de arquitetura e mostra que **não existem decisões perfeitas**. O papel do arquiteto é entender o contexto, avaliar alternativas e compreender as consequências de cada escolha.

---

## 1. Primeira Lei — Tudo é um Trade-off

Toda decisão arquitetural possui vantagens e desvantagens.

Não existe uma solução perfeita ou uma **"bala de prata"**. O objetivo é entender os trade-offs e escolher a alternativa mais adequada ao contexto.

### Exemplo

**Biblioteca compartilhada:**

- alta performance;
- baixa latência;
- mas alterações podem exigir novo build e deploy dos consumidores.

**Serviço compartilhado:**

- atualização disponível imediatamente;
- pode ser usado por diferentes tecnologias;
- mas adiciona latência e dependência da rede.

### Corolários

- Se parece que não existe trade-off, provavelmente ele ainda não foi identificado.
- Trade-offs precisam ser **reavaliados**, pois tecnologia, equipe e negócio mudam.

---

## 2. Segunda Lei — O "Porquê" é mais importante que o "Como"

Saber **como** implementar algo é importante, mas arquitetura precisa explicar **por que** aquela solução foi escolhida.

Isso evita o **Out of Context Antipattern**: copiar uma arquitetura ou tecnologia usada por outra empresa sem entender o contexto e os motivos originais.

Por isso, documentar decisões com **ADRs** ajuda a preservar o contexto e o raciocínio por trás das escolhas.

> **O código mostra como. A arquitetura explica por quê.**

---

## 3. Terceira Lei — As Decisões Estão em um Espectro

As decisões arquiteturais geralmente não são simplesmente:

> "A ou B"

Existem várias possibilidades entre os extremos.

Por exemplo:

```text
Pouca abstração ──────────────── Muita abstração

Fetch direto ─── Service ─── Hook ─── React Query
```

A escolha depende do contexto, como:

- tamanho do projeto;
- experiência da equipe;
- complexidade;
- requisitos do sistema;
- necessidades futuras.

---

## 4. Exemplo em React

Imagine diferentes formas de lidar com dados:

- `fetch` diretamente nos componentes → simples, mas pode gerar duplicação;
- estado global para tudo → centralizado, mas pode aumentar complexidade;
- uma solução intermediária → pode oferecer cache e revalidação sem transformar todo o estado da aplicação em estado global.

O ponto principal não é escolher uma tecnologia específica.

É entender:

> **Qual problema estamos tentando resolver e quais trade-offs essa escolha traz?**

---

## Resumo Final do Capítulo

As três leis podem ser resumidas assim:

1. **Trade-offs:** toda decisão possui consequências.
2. **Why:** entender o motivo da decisão é tão importante quanto saber implementá-la.
3. **Espectro:** soluções normalmente ficam entre diferentes extremos.

### 🎓 Principal aprendizado do livro

> **Arquitetura de software é muito mais sobre tomar boas decisões dentro de um contexto do que encontrar uma solução perfeita.**

O arquiteto precisa entender **tecnologia, negócio, pessoas, restrições e consequências**, documentando o raciocínio para que as decisões continuem fazendo sentido ao longo do tempo.
