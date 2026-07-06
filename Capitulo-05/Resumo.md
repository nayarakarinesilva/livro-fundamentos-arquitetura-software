# Capítulo 5 – Identificando Características de Arquitetura

Este capítulo aborda como transformar os objetivos do negócio em **características de arquitetura** (os famosos *-ilities*), que orientam as decisões técnicas do sistema.

## 1. Tradução do negócio para características técnicas

Arquitetos e stakeholders costumam falar "línguas" diferentes. Cabe ao arquiteto transformar necessidades de negócio em requisitos técnicos.

**Exemplos:**

- **Time to Market**
  - Agilidade
  - Testabilidade
  - Implantabilidade (*Deployability*)

- **Satisfação do usuário**
  - Performance
  - Disponibilidade
  - Segurança
  - Tolerância a falhas (*Fault Tolerance*)

---

## 2. Características compostas

Algumas características são formadas por outras menores.

**Exemplo:**

- **Agilidade** depende de:
  - Modularidade
  - Testabilidade
  - Implantabilidade

### Atenção

Um erro comum é focar apenas em uma característica (como performance) e ignorar outras igualmente importantes (como disponibilidade e escalabilidade), comprometendo o sucesso da solução.

---

## 3. Características explícitas e implícitas

### Explícitas

São aquelas descritas diretamente nos requisitos ou facilmente deduzidas.

**Exemplo:**
- Um sistema que precisa atender milhões de usuários exige **escalabilidade**.

### Implícitas

Normalmente não são solicitadas pelos usuários, mas são essenciais.

Exemplos:
- Disponibilidade
- Segurança
- Manutenibilidade

---

## 4. O perigo de querer "tudo" – O caso Vasa

O capítulo utiliza o exemplo do navio de guerra sueco **Vasa**.

O rei desejava um navio com muitas funcionalidades (mais conveses e muitos canhões pesados). O resultado foi uma embarcação instável que afundou logo em sua primeira viagem.

### Lição para arquitetura

Tentar atender todas as características possíveis torna o sistema:

- Mais caro
- Mais complexo
- Mais difícil de manter
- Mais propenso ao fracasso

---

## 5. Priorização: o "Top 3"

Quando perguntados, os stakeholders costumam dizer que **"tudo é importante"**.

Por isso, o arquiteto deve utilizar técnicas de priorização para identificar as **três características de arquitetura mais importantes**, pois elas irão orientar:

- As decisões estruturais
- A análise de trade-offs
- O foco do projeto

---

# Resumo

O principal ensinamento deste capítulo é que o arquiteto não deve apenas listar características técnicas, mas entender quais delas realmente atendem aos objetivos do negócio.

Como é impossível otimizar todas as características ao mesmo tempo, é fundamental **priorizar aquelas que geram mais valor**, aceitando os trade-offs necessários para construir uma arquitetura equilibrada.