# 📚 Capítulo 3: Modularidade

Neste capítulo, os autores explicam a importância da modularidade na arquitetura de software. Embora não exista uma fórmula para definir uma boa modularidade, ela é fundamental para criar sistemas organizados, fáceis de entender e de manter ao longo do tempo.

## Modularidade x Granularidade

Um dos primeiros conceitos apresentados é a diferença entre esses dois termos:

- **Modularidade:** -> consiste em dividir um sistema em módulos independentes, agrupando funcionalidades relacionadas.

- **granularidade:** -> refere-se ao tamanho desses módulos, funções ou serviços.

Uma granularidade inadequada pode resultar em arquiteturas confusas e difíceis de manter.


## Os três pilares da modularidade

Para avaliar a qualidade da modularidade, o capítulo apresenta três conceitos principais:

### Coesão
A coesão mede o quanto os elementos de um módulo estão relacionados entre si.

- Quanto maior a coesão, melhor.
- Um módulo coeso possui elementos que trabalham para um mesmo objetivo.
- É apresentada a métrica LCOM (Lack of Cohesion in Methods), utilizada para medir a falta de coesão em uma classe.
- Um valor alto de LCOM pode indicar que a classe possui responsabilidades demais e deveria ser dividida.

## Acoplamento

O acoplamento representa o nível de dependência entre os módulos.

Existem dois tipos principais:

- **Acoplamento aferente (Ca):** -> quantidade de módulos que dependem daquele módulo.
- **Acoplamento eferente (Ce):** -> quantidade de módulos dos quais ele depende.

O capítulo também apresenta as métricas de Robert C. Martin:

- **Instabilidade (I):** -> mede o quanto um módulo pode ser afetado por mudanças.
- **Abstração (A):** -> representa a proporção de interfaces e classes abstratas.

- **Distância da Sequência Principal (D):** -> avalia se existe um equilíbrio entre abstração e estabilidade.

O objetivo é manter um sistema equilibrado, evitando módulos muito rígidos ou excessivamente abstratos.

## Conascência

A conascência descreve situações em que uma alteração em um componente exige mudanças em outro para que o sistema continue funcionando corretamente.

Ela pode ser:

- **Estática:** -> identificada diretamente no código-fonte.
- **Dinâmica:** -> percebida apenas durante a execução da aplicação.

O capítulo destaca duas recomendações importantes:

- Preferir formas mais fracas de conascência sempre que possível.
- Quanto maior a distância entre os componentes, menor deve ser a dependência entre eles.

## Módulos e Componentes

No final do capítulo, os autores explicam que, na prática, os módulos são chamados de componentes, que representam os blocos de construção de um sistema.

A identificação correta desses componentes é essencial para construir uma arquitetura bem organizada e será aprofundada nos próximos capítulos.

De modo geral, o capítulo mostra que uma boa modularidade depende do equilíbrio entre:

- Alta coesão;
- Baixo acoplamento;
- Uso adequado da conascência.

Esses conceitos contribuem para o desenvolvimento de sistemas mais organizados, reutilizáveis e fáceis de evoluir e manter ao longo do tempo.