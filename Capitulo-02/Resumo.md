# 📚 Capítulo 2: Pensamento arquitetural

Esse capítulo traz aspectos de como pensar como um arquiteto. Onde é preciso entender o que é arquitetura e design. Trata-se da mudança de mentalidade necessária para um desenvolvedor se tornar um arquiteto, focando em enxergar o sistema sob um ponto de vista estrutural e estratégico.

## 1. Arquitetura versus design
A arquitetura de software tem menos a ver com a apareência de um sistema e mais com sua estrutura, enquanto design tem mais a ver com a aparecência e menos com a estrutura.

## Decisões estratégicas versus táticas

- **Decisões táticas** -> Decisões de design são táticas, de curto prazo e mais independentes;

- **Decisões estratégicas** -> Decisões de arquitetura são estratégicas, geralmente são de longo prazo  e exigem muito planejamento.

## Nível de esforço

> “Arquitetura é aquilo que é difícil de mudar.”
(Martin Fowler)

Se uma mudança exige um esforço mínimo, ela está mais para o lado do design; se exige semanas de planejamento e impacto profundo, é arquitetura.

## 2. Amplitude Técnica e a Pirâmide do Conhecimento

Para um arquiteto, a amplitude de conhecimento (breadth) é mais importante do que a profundidade (depth)
. O capítulo apresenta uma pirâmide com três níveis de conhecimento técnico:

- **O que você sabe:** Tecnologias nas quais você é especialista (Profundidade);

- **O que você sabe que não sabe:** Tecnologias nas quais você tem consciência, e entende para que serve, mas não domina (Amplitude);

- **O que você não sabe que não sabe:** Envolve toda a gama de tecnologias, ferramentas, frameworks e linguagens que você nem sabe que existem.

Para um arquiteto, a amplitude é mais importante do que a profundidade. Isso exige sacrificar parte da profundidade técnica para expandir a amplitude.

## 3. Técnicas para se manter atualizado

- **A regra dos 20 minutos** -> A ideia é dedicar pelao menos 20 minutos por dia para aprender algo novo ou explorar tendências, a dica é fazer isso preferencialmente logo cedo.

- **Radar Tecnológico Pessoal** -> Criar um radar inspirado no da Thoughtworks para formalizar a avaliação de novas ferramentas, linguagens e técnicas.

## 4. Análise Profunda de Trade-offs
O capítulo reforça que "não existem respostas certas ou erradas na arquitetura — apenas trade-offs"

Para ilustrar, os autores usam um exemplo de sistema de leilão comparando o uso de Filas (ponto a ponta) versus Tópicos (pub/sub). Enquanto tópicos oferecem melhor extensibilidade e desacoplamento, eles trazem riscos de segurança e dificuldades de monitoramento que as filas mitigam.

Pensar como arquiteto significa analisar não apenas os benefícios, mas também as consequências negativas de cada escolha

## 5. Compreensão dos Drivers de Negócio

Arquitetos devem ser capazes de traduzir os objetivos de sucesso do negócio (ex: tempo de lançamento no mercado) em características técnicas da arquitetura, como escalabilidade, performance e disponibilidade.

## 6. Equilíbrio com a Programação

Apesar de focar na estratégia, o arquiteto deve manter habilidades práticas, escrevendo código de qualidade de produção sempre que possível, evitando criar "arquiteturas de torre de marfim" que não funcionam na prática
