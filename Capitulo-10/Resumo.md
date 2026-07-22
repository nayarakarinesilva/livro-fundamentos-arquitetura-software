# Capítulo 10 – Estilo arquitetural em camadas

O **Estilo de Arquitetura em Camadas (Layered Architecture)**, também conhecido como **n-tier**, é considerado o padrão mais utilizado em aplicações legadas devido à sua **simplicidade**, **baixo custo inicial** e facilidade de implementação.

---

## 1. O que é a Arquitetura em Camadas?

Imagine um **bolo**, onde cada camada possui uma responsabilidade específica. Normalmente, esse estilo é dividido em **quatro camadas principais**:

### 🖥️ Camada de Apresentação (Presentation)
- Responsável pela interface com o usuário.
- Contém telas, formulários, botões e componentes visuais.

### ⚙️ Camada de Negócio (Business)
- Contém as regras de negócio da aplicação.
- Exemplo:
  - cálculo de descontos;
  - cálculo de impostos;
  - validações.

### 💾 Camada de Persistência (Persistence)
- Responsável pelo acesso aos dados.
- Faz a comunicação entre a aplicação e o banco de dados.

### 🗄️ Camada de Banco de Dados (Database)
- Local onde os dados são armazenados fisicamente.

---

## 2. Camadas Abertas vs. Camadas Fechadas

Um dos conceitos mais importantes do capítulo é o **isolamento entre as camadas**.

### 🔒 Camada Fechada

Uma requisição deve passar obrigatoriamente por todas as camadas.

Fluxo:

```text
Apresentação
      ↓
Negócio
      ↓
Persistência
      ↓
Banco de Dados
```

**Vantagens:**

- Maior isolamento.
- Baixo acoplamento.
- Alterações em uma camada afetam menos as outras.

**Exemplo:**

Se o banco de dados mudar (MySQL → PostgreSQL), a camada de apresentação continua funcionando, pois ela nunca conversa diretamente com o banco.

---

### 🔓 Camada Aberta

Permite que algumas camadas sejam "puladas".

Exemplo:

```text
Apresentação
      ↓
Serviços Compartilhados
```

Sem precisar passar por todas as demais camadas.

É útil para:

- autenticação;
- logs;
- cache;
- serviços compartilhados.

---

## 3. Exemplo Prático

Imagine que sua aplicação utiliza **Angular** e você deseja migrar para **React**.

Como a arquitetura está organizada em camadas:

- Apenas a **Camada de Apresentação** precisa ser alterada.
- As regras de negócio permanecem exatamente iguais.
- O acesso ao banco também continua funcionando.

Isso reduz bastante o impacto da mudança.

---

## 4. O Antipadrão Sinkhole

Um problema bastante comum nesse estilo é o **Sinkhole Antipattern**.

Ele acontece quando uma requisição passa por todas as camadas, mas nenhuma delas realiza processamento.

Exemplo:

```text
Apresentação
      ↓
Negócio
      ↓
Persistência
      ↓
Banco
```

Cada camada apenas chama a próxima, sem adicionar nenhuma lógica.

### Problemas

- Código desnecessário.
- Mais burocracia.
- Menor desempenho.
- Maior quantidade de classes para manter.

> Se cerca de **80% das requisições** apenas atravessam todas as camadas sem processamento, talvez a Arquitetura em Camadas não seja a melhor escolha para o projeto.

---

# Características da Arquitetura em Camadas

| Característica | Avaliação | Observação |
|---------------|-----------|------------|
| 💰 Custo Geral | ⭐ | Baixo custo inicial e fácil de começar. |
| 😊 Simplicidade | ⭐⭐⭐⭐⭐ | Muito fácil de entender e implementar. |
| 🧩 Modularidade | ⭐ | Com o crescimento, as responsabilidades tendem a ficar misturadas. |
| 📈 Escalabilidade | ⭐ | Não escala muito bem para sistemas muito grandes. |
| 🧪 Testabilidade | ⭐⭐ | Pode ser difícil testar uma camada isoladamente. |
| 🚀 Facilidade de Deploy | ⭐ | Pequenas alterações normalmente exigem publicar toda a aplicação. |
| 🛡️ Tolerância a Falhas | ⭐ | Uma falha em uma camada crítica pode comprometer todo o sistema. |

---

# Vantagens

- Fácil de aprender.
- Organização simples.
- Baixo custo de desenvolvimento.
- Bastante utilizada em sistemas corporativos.
- Boa separação de responsabilidades.

---

# Desvantagens

- Pouca escalabilidade.
- Pode gerar muitas classes desnecessárias.
- Deploy costuma envolver toda a aplicação.
- Pode apresentar baixo desempenho devido ao excesso de camadas.
- Alto risco do antipadrão **Sinkhole**.

---

# Quando Utilizar?

✅ Projetos pequenos.

✅ Sistemas internos.

✅ MVPs.

✅ Protótipos.

✅ Aplicações com orçamento reduzido.

---

# Quando Evitar?

❌ Sistemas com milhões de usuários.

❌ Aplicações que exigem alta performance.

❌ Sistemas distribuídos.

❌ Microsserviços.

❌ Projetos que precisam crescer rapidamente.

---

# Resumo

A **Arquitetura em Camadas** é um dos estilos arquiteturais mais tradicionais e utilizados no desenvolvimento de software.

Seu maior diferencial é a **simplicidade**, tornando-a uma excelente escolha para aplicações menores ou projetos com orçamento limitado.

Por outro lado, conforme o sistema cresce, esse estilo tende a apresentar problemas de **escalabilidade**, **desempenho** e **manutenção**, especialmente quando ocorre o antipadrão **Sinkhole**, em que as camadas apenas repassam chamadas sem executar qualquer lógica.

Em geral, é uma ótima opção para sistemas simples, mas deixa de ser a melhor escolha quando a aplicação precisa de alto desempenho, evolução constante ou grande capacidade de crescimento.


# Leitura com clube

<div align="start">
  <img src="./images/image.png" width="600">
</div>