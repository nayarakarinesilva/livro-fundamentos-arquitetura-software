# Capítulo 26 — Interseções de Arquitetura

O capítulo mostra que arquitetura de software não é apenas escolher um estilo arquitetural. Ela precisa estar **alinhada com código, infraestrutura, dados, processos, equipes e negócio**.

---

## 1. Arquitetura e Implementação

O código precisa respeitar as decisões arquiteturais.

### Principais pontos:

- **Preocupações operacionais:** uma otimização local não pode prejudicar escalabilidade ou disponibilidade.
- **Integridade estrutural:** a organização de pastas e dependências deve refletir a arquitetura.
- **Restrições arquiteturais:** regras que determinam como os componentes podem se comunicar.

> A arquitetura definida no papel precisa continuar existindo no código.

---

## 2. Arquitetura e Infraestrutura

A infraestrutura precisa suportar as necessidades da aplicação.

Por exemplo, uma aplicação que precisa escalar deve ter uma infraestrutura preparada para lidar com o aumento de usuários e tráfego.

É nesse contexto que práticas como **DevOps, automação e CI/CD** ajudam a aproximar desenvolvimento, arquitetura e operações.

---

## 3. Arquitetura e Dados

A estrutura dos dados também precisa estar alinhada à arquitetura.

Um exemplo é o **Microservices**:

- cada serviço deve ter maior autonomia;
- compartilhar um único banco pode criar forte acoplamento;
- o modelo **database-per-service** ajuda a manter o isolamento entre serviços.

---

## 4. Arquitetura e Governança

As regras arquiteturais podem ser verificadas automaticamente.

Exemplos:

- testes de arquitetura;
- **fitness functions**;
- CI/CD;
- ferramentas como TSArch.

Isso ajuda a evitar que o código se afaste gradualmente da arquitetura definida.

---

## 5. Arquitetura e Pessoas

A arquitetura também está relacionada à organização das equipes.

A **Lei de Conway** sugere que a estrutura de comunicação de uma organização tende a influenciar a estrutura dos sistemas que ela desenvolve.

Por isso, a divisão dos times deve considerar os limites e responsabilidades do próprio software.

Além disso, a arquitetura precisa acompanhar o **momento do negócio**, como crescimento, redução de custos ou mudança de mercado.

---

## 6. Arquitetura e IA Generativa

A IA generativa pode participar da arquitetura de duas formas:

- como **funcionalidade do próprio produto**;
- como **ferramenta de apoio ao arquiteto**, ajudando a analisar alternativas, revisar ADRs e explorar possíveis soluções.

A IA pode ajudar no processo, mas as decisões arquiteturais continuam dependendo do contexto e dos objetivos do sistema.

---

## 7. Exemplo em React

Imagine um catálogo com milhares de produtos.

Uma abordagem problemática seria carregar tudo no navegador:

```jsx
const [products, setProducts] = useState([]);

useEffect(() => {
  fetchAllProducts().then(setProducts);
}, []);
```

Para poucos produtos pode funcionar, mas em um catálogo muito grande pode consumir memória e prejudicar a aplicação.

Uma solução mais alinhada seria utilizar **paginação ou busca sob demanda**:

```jsx
const { data } = useProducts({
  page,
  limit: 20,
});
```

A ideia principal é:

> **Uma decisão de implementação não deve prejudicar os objetivos arquiteturais do sistema.**

---

## Resumo

- Arquitetura precisa estar alinhada com o **código**.
- Código, infraestrutura e dados devem trabalhar em conjunto.
- **Restrições arquiteturais** ajudam a proteger decisões importantes.
- **Fitness functions e testes automatizados** ajudam a preservar a arquitetura.
- A estrutura dos **times** também influencia a arquitetura.
- O negócio e seu momento devem ser considerados nas decisões.
- IA generativa pode apoiar arquitetos na análise e documentação.

> **Uma boa arquitetura não existe isoladamente: ela precisa funcionar em conjunto com todo o ecossistema do sistema.**

---

# Leitura

![alt text](image.png)
