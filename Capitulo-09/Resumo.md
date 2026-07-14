# Capítulo 9 – Fundamentos

O Capítulo 9 apresenta os conceitos básicos necessários para entender os estilos de arquitetura que serão vistos nos próximos capítulos.

## 1. Estilo x Padrão de Arquitetura

É importante saber a diferença entre os dois:

- **Estilo de Arquitetura:** mostra como o sistema é organizado de forma geral (ex.: arquitetura em camadas e microserviços).
- **Padrão de Arquitetura:** é uma solução para resolver um problema específico dentro de um estilo (ex.: CQRS e Saga).

---

## 2. Padrões Fundamentais

O capítulo apresenta alguns modelos importantes:

- **Big Ball of Mud (Grande Emaranhado de Lama):** sistema sem organização, onde tudo depende de tudo. Isso dificulta a manutenção e aumenta o risco de erros.
- **Arquitetura Unitária:** todo o sistema roda em um único lugar. É comum em sistemas embarcados.
- **Cliente/Servidor:** separa a interface (frontend) da parte que processa os dados (backend).

---

## 3. Como dividir a arquitetura

Existem duas formas principais de organizar um sistema:

### Particionamento Técnico
Os componentes são separados pela função técnica.

Exemplos:
- Apresentação
- Negócio
- Persistência (Banco de Dados)

Esse é o modelo mais comum em sistemas monolíticos tradicionais.

### Particionamento por Domínio
Os componentes são separados pelas áreas de negócio ou funcionalidades.

Exemplos:
- Vendas
- Financeiro
- Estoque

Esse modelo é muito usado em **Monólitos Modulares** e **Microserviços**.

> Atualmente, essa é a abordagem mais utilizada.

---

## 4. Monolítico x Distribuído

Os estilos de arquitetura podem ser divididos em dois grupos:

### Monolítico
- Todo o sistema é implantado como uma única aplicação.
- Exemplos: Arquitetura em Camadas e Microkernel.

### Distribuído
- O sistema é dividido em várias aplicações que se comunicam pela rede.
- Exemplos: Microserviços e Arquitetura Orientada a Eventos.

---

## 5. Falácias da Computação Distribuída

Ao criar sistemas distribuídos, muitas pessoas fazem suposições que nem sempre são verdadeiras.

As principais são:

- A rede nunca falha.
- A comunicação é instantânea.
- A internet sempre tem velocidade suficiente.
- A rede é segura.
- A estrutura da rede nunca muda.
- Existe apenas um administrador.
- A comunicação não tem custo.
- Todos os sistemas da rede funcionam da mesma forma.

Além disso, os autores lembram que também é um erro pensar que:

- Versionar serviços é simples.
- Fazer rollback sempre funciona.

---

## 6. Organização das equipes

A arquitetura também influencia como as equipes trabalham.

O capítulo apresenta o conceito de **Team Topologies**, que organiza as equipes de acordo com o valor que elas entregam, como:

- Equipes responsáveis por uma área do negócio.
- Equipes que criam plataformas para apoiar outras equipes.

---

# Resumo

O Capítulo 9 mostra que escolher um estilo de arquitetura vai muito além da tecnologia. Essa decisão influencia:

- Como o sistema será dividido.
- Como as equipes irão trabalhar.
- A dificuldade de manter o sistema.
- Os desafios de comunicação, principalmente em arquiteturas distribuídas.