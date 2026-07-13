# 📚 Capítulo 8: Pensamento Baseado em Componentes

Este capítulo explica que um arquiteto de software enxerga o sistema como um conjunto de **componentes**, ou seja, partes que trabalham juntas para realizar as funções da aplicação. Enquanto o desenvolvedor costuma pensar em classes e funções, o arquiteto pensa em como esses componentes se relacionam.

## 1. O que são Componentes?

Componentes são as principais partes de um sistema, cada uma responsável por uma função específica.

**Exemplo:**
- Gerenciamento de Usuários
- Processamento de Pagamentos
- Controle de Estoque

### 🏠 Analogia

Imagine que o sistema é uma casa:
- Cozinha → prepara comida.
- Quarto → serve para descansar.
- Banheiro → tem outra função.

Cada cômodo tem uma responsabilidade. Com os componentes acontece a mesma coisa.

No código, eles normalmente aparecem como **pastas**, **módulos** ou **namespaces**.

---

## 2. Arquitetura Lógica x Arquitetura Física

### Arquitetura Lógica
Mostra **como o sistema é organizado** e como seus componentes se relacionam, sem pensar na tecnologia utilizada.

### Arquitetura Física
Mostra **onde o sistema será executado**, como:
- Servidores
- Banco de dados
- Interfaces
- Microserviços ou monólito

> 💡 Os autores recomendam criar primeiro a arquitetura lógica. Assim a equipe organiza melhor o sistema antes de decidir como ele será implantado.

---

## 3. Como identificar os componentes

Criar componentes é um processo de melhoria contínua. Algumas formas de começar são:

### Pelo fluxo do sistema (Workflow)
Criar componentes seguindo os passos que o usuário executa.

**Exemplo:**
Login → Carrinho → Pagamento.

### Pelos atores
Pensar em quem usa o sistema e nas ações que cada pessoa realiza.

**Exemplo:**
- Cliente faz pedidos.
- Administrador gerencia produtos.

### Evite a "Armadilha da Entidade"

Não crie componentes apenas com base nas tabelas do banco de dados.

❌ Exemplo:
- Pedido
- Cliente
- Produto

Isso pode gerar componentes enormes, com responsabilidades demais, dificultando a manutenção.

---

## 4. Refinando os componentes

No começo, os componentes são apenas uma ideia. Depois eles vão sendo ajustados.

O arquiteto deve:

- adicionar as funcionalidades (histórias de usuário);
- verificar se cada componente tem apenas uma responsabilidade;
- dividir componentes que fazem muitas coisas;
- avaliar se alguma necessidade, como escalabilidade, exige separar um componente em partes menores.

---

## 5. Acoplamento entre componentes

Os componentes devem depender o mínimo possível uns dos outros.

### Acoplamento Estático
Acontece quando um componente chama outro diretamente.

### Acoplamento Temporal
Acontece quando um componente só pode funcionar depois que outro termina.

### Lei de Demeter (Princípio do Menor Conhecimento)

Cada componente deve conhecer apenas o necessário sobre os outros.

Quanto menor essa dependência, mais fácil será alterar, testar e manter o sistema.

---

# Resumindo

O arquiteto deve dividir o sistema em componentes com responsabilidades bem definidas. Esses componentes devem ser organizados de forma que dependam o mínimo possível uns dos outros, tornando o software mais fácil de entender, manter e evoluir.