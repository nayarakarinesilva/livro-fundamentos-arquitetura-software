# Capítulo 25 — Habilidades de Negociação e Liderança

O capítulo mostra que ser um bom arquiteto não envolve apenas conhecimento técnico. **Liderança, comunicação, negociação e colaboração** são fundamentais para trabalhar com diferentes pessoas e tomar decisões que façam sentido para o projeto.

---

## 1. Negociação e Facilitação

O arquiteto precisa negociar constantemente com diferentes grupos:

- **Stakeholders:** entender a necessidade real por trás de pedidos exagerados e encontrar soluções viáveis.
- **Outros arquitetos:** quando houver divergências técnicas, usar **POCs e dados reais** em vez de apenas discutir opiniões.
- **Desenvolvedores:** explicar o motivo das decisões e incentivar o time a participar e testar alternativas.

> **Demonstração vence discussão.**

---

## 2. O Arquiteto como Líder

### Evitar complexidade acidental

Nem todo problema precisa de uma solução complexa. O arquiteto deve buscar a solução **mais simples que resolva o problema**, evitando tecnologias e estruturas desnecessárias.

### Os 4 Cs da liderança

- **Comunicação**
- **Colaboração**
- **Clareza**
- **Concisão**

### Pragmatismo + visão

O arquiteto precisa equilibrar:

- **Visão:** pensar na evolução futura do sistema.
- **Pragmatismo:** considerar prazo, orçamento, tecnologia e capacidade atual da equipe.

---

## 3. Liderar sem impor

A forma de comunicação influencia diretamente o engajamento da equipe.

Em vez de dar ordens:

> "Você precisa usar cache aqui."

É melhor incentivar a discussão:

> "Você já considerou usar cache aqui? O que acha?"

Assim, o desenvolvedor participa da decisão e pode apresentar outras soluções.

---

## 4. Preservar o Flow da Equipe

O arquiteto também precisa proteger o tempo de concentração dos desenvolvedores.

Isso significa:

- evitar reuniões desnecessárias;
- não interromper constantemente quem está trabalhando;
- acompanhar a equipe sem controlar cada detalhe;
- criar espaço para que os desenvolvedores mantenham o **flow**.

---

## 5. Exemplo em React

Imagine um problema simples de desempenho no carrinho.

Uma solução poderia ser adicionar várias bibliotecas e camadas de gerenciamento de estado sem necessidade.

Uma abordagem mais pragmática seria primeiro investigar o problema e considerar soluções simples, como:

```jsx
const total = useMemo(() => {
  return items.reduce((sum, item) => sum + item.price, 0);
}, [items]);
```

A ideia não é que `useMemo` seja sempre a solução, mas que a equipe **entenda o problema antes de adicionar complexidade**.

---

## Resumo

- Arquitetura também envolve **pessoas e comunicação**.
- Negocie entendendo a necessidade real.
- Use **POCs e dados** para resolver divergências técnicas.
- Envolva os desenvolvedores nas decisões.
- Evite **complexidade acidental**.
- Equilibre visão de longo prazo com pragmatismo.
- Comunique-se com **clareza e concisão**.
- Respeite o **flow** e o tempo de concentração da equipe.

> **Um bom arquiteto não apenas define soluções: ele ajuda a equipe a construir boas soluções em conjunto.**

---

# Leitura

![alt text](image.png)
