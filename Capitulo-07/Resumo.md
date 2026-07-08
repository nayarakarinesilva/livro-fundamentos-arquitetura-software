# Capítulo 7 - O Escopo das Características de Arquitetura

Neste capítulo, os autores mostram que **nem todas as partes de um sistema precisam ter as mesmas características**.

Em vez de pensar no sistema inteiro como um único bloco, devemos separá-lo em partes menores. Assim, cada parte recebe apenas o que realmente precisa, deixando o sistema mais simples, mais barato e mais fácil de manter.

---

# O que é um Quantum de Arquitetura?

O **Quantum de Arquitetura** é o conceito mais importante deste capítulo.

Ele é a **menor parte do sistema que consegue funcionar sozinha**.

Se duas partes sempre precisam uma da outra para funcionar (por exemplo, usam o mesmo banco de dados), elas fazem parte do mesmo quantum.

### Exemplo

- **Monólito:** normalmente todo o sistema é um único quantum.
- **Microserviços:** cada serviço funciona de forma independente e possui seu próprio quantum.

---

# Por que o escopo é importante?

Nem todas as partes do sistema precisam das mesmas características.

Por exemplo:

- A tela onde o cliente faz um pedido precisa ser rápida e suportar muitos acessos (escalabilidade).
- A tela de relatórios internos não precisa suportar tantos acessos, mas precisa proteger bem os dados (segurança).

Se tentarmos fazer o sistema inteiro ter todas essas características, o projeto fica mais caro e mais complicado.

Por isso, o ideal é dar para cada parte apenas o que ela realmente precisa.

---

# Como isso ajuda na escolha da arquitetura?

Depois de entender as necessidades de cada parte, fica mais fácil escolher a arquitetura.

### Monólito

É uma boa opção quando praticamente todo o sistema precisa das mesmas características.

### Microserviços

São uma boa opção quando cada parte possui necessidades diferentes.

Assim, cada serviço pode crescer e evoluir sem afetar os outros.

---

# Tipos de acoplamento

O capítulo também fala sobre dois tipos de acoplamento.

## Acoplamento Estático

É quando duas partes do sistema estão ligadas porque compartilham alguma coisa.

Por exemplo:

- o mesmo banco de dados;
- o mesmo código;
- a mesma biblioteca.

Esse tipo de ligação ajuda a definir o tamanho de um quantum.

---

## Acoplamento Dinâmico

É a forma como as partes do sistema conversam enquanto ele está funcionando.

Existem duas formas:

### Síncrono

Um serviço envia uma solicitação e espera a resposta.

**Exemplo:**

```
Serviço A → Serviço B
      (espera responder)
```

### Assíncrono

O serviço envia uma mensagem e continua trabalhando sem esperar uma resposta.

**Exemplo:**

```
Serviço A → Fila → Serviço B
```

---

# A regra do "Depende"

Mais uma vez, os autores reforçam que **não existe uma arquitetura perfeita para todos os sistemas**.

Tudo depende do que o sistema precisa.

O importante é juntar as partes que possuem necessidades parecidas e separar as que possuem necessidades diferentes.

Assim, cada parte pode crescer e mudar sem deixar o sistema inteiro mais complicado.

---

# Resumindo

- Um **Quantum de Arquitetura** é a menor parte do sistema que funciona sozinha.
- Nem todas as partes do sistema precisam das mesmas características.
- Definir o escopo ajuda a evitar custos e complexidade desnecessários.
- O **acoplamento estático** mostra quais partes estão ligadas entre si.
- O **acoplamento dinâmico** mostra como essas partes se comunicam.
- Quando diferentes partes possuem necessidades diferentes, uma arquitetura de microserviços pode ser uma boa escolha.

> **O principal aprendizado deste capítulo é:** não tente fazer todo o sistema ter as mesmas características. Separe o sistema em partes menores (quantums) e dê a cada uma apenas o que ela realmente precisa.