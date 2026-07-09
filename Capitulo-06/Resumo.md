# Resumo – Capítulo 6: Medindo e Governando Características de Arquitetura

Neste capítulo, os autores mostram que não basta dizer que um sistema precisa ser rápido, escalável ou seguro. Essas características precisam ser medidas para saber se realmente estão sendo atendidas.

## Transformando ideias em números

Os autores explicam que termos como **performance**, **escalabilidade** e **testabilidade** são muito genéricos. Para saber se um sistema está saudável, é preciso criar métricas.

Eles apresentam três tipos de medidas:

- **Medidas operacionais:** verificam como o sistema funciona na prática, como tempo de resposta e desempenho.
- **Medidas estruturais:** analisam a qualidade do código. Um exemplo é a **Complexidade Ciclomática**, que mede quantos caminhos de decisão um método possui. Quanto mais `if`, `else` e outras decisões, mais difícil o código tende a ser para entender, testar e manter.
- **Medidas de processo:** avaliam como a equipe desenvolve o software, como cobertura de testes, tempo de deploy e quantidade de implantações bem-sucedidas.

## Fitness Functions

O conceito mais importante do capítulo é o de **Fitness Function**.

Uma Fitness Function é qualquer mecanismo que verifica se a arquitetura continua seguindo as regras definidas.

Ela não é uma ferramenta específica. Pode ser um teste automatizado, uma análise de código ou qualquer outra verificação que confirme que a arquitetura continua saudável.

A ideia é que a arquitetura seja validada constantemente, em vez de depender apenas da atenção das pessoas.

## Governança automatizada

Os autores defendem que muitas regras da arquitetura devem ser verificadas automaticamente.

Alguns exemplos são:

- Impedir dependências cíclicas entre módulos.
- Garantir que cada camada da aplicação respeite sua responsabilidade.
- Criar testes que falhem quando alguém quebrar uma regra da arquitetura.
- Monitorar sistemas em produção para verificar se continuam seguros e resilientes.

Assim, o arquiteto não precisa revisar cada linha de código manualmente. O próprio processo identifica quando alguma regra foi quebrada.

## O ponto que mais gostei: o checklist

Uma ideia apresentada no capítulo, inspirada no livro **The Checklist Manifesto**, foi o uso de **checklists**.

Achei interessante porque muitas vezes os erros não acontecem por falta de conhecimento, mas porque esquecemos de verificar detalhes importantes.

Um checklist funciona como uma lista de conferência antes de finalizar uma tarefa. Ele ajuda a evitar erros simples e garante que pontos importantes sejam revisados.

Por exemplo, antes de abrir um Pull Request, eu poderia verificar:

- Removi `console.log`?
- O código ficou simples e legível?
- Evitei `if` desnecessários ou muito aninhados?
- Os nomes das funções estão claros?
- Existem testes para a alteração?
- O commit descreve corretamente o que foi feito?

Percebi que esse tipo de checklist pode ser criado de acordo com os erros e feedbacks que recebemos ao longo do tempo. Assim, ele deixa de ser apenas uma lista de tarefas e passa a ser uma ferramenta para melhorar a qualidade do código e evitar repetir os mesmos erros.

## Conclusão

A principal mensagem do capítulo é que uma boa arquitetura não deve depender apenas da experiência das pessoas. Ela precisa ser:

- Ser **mensurável**, para sabermos se continua atendendo aos objetivos.
- Ser **governada**, de preferência por meio de verificações automáticas.

Além disso, gostei da ideia dos checklists, pois eles mostram que pequenas verificações feitas de forma consistente podem evitar muitos problemas e ajudar tanto desenvolvedores quanto arquitetos a manter a qualidade do software.

## Discusão do livro:

![alt text](image.png)