# Resumo – Arquitetura Microkernel (Plug-in)

O **Microkernel** (ou **Arquitetura de Plug-in**) é um estilo de arquitetura usado em sistemas que precisam ser **fáceis de personalizar e expandir**.

Um bom exemplo é o **VS Code** ou o **Google Chrome**. Eles possuem um sistema básico que funciona sozinho e permitem instalar extensões (plug-ins) para adicionar novas funcionalidades.

---

# 1. Como funciona?

A arquitetura é dividida em duas partes:

## Core (Núcleo)

É a parte principal do sistema.

Responsável pelo funcionamento básico e pelas funções essenciais.

O ideal é que quase nunca precise ser alterado.

## Plug-ins

São módulos independentes que adicionam novas funcionalidades.

Cada plug-in pode ter sua própria regra de negócio sem modificar o Core.

Isso deixa o sistema mais organizado e fácil de manter.

---

# 2. Como o Core encontra os Plug-ins?

O Core utiliza dois recursos principais:

## Registro (Registry)

É uma lista onde ficam cadastrados todos os plug-ins disponíveis.

Quando o sistema precisa executar uma função, procura nessa lista.

## Contrato (Interface)

É um conjunto de regras que todo plug-in deve seguir.

Assim, qualquer novo plug-in pode ser adicionado sem alterar o Core.

---

# 3. Exemplo em JavaScript (Node.js)

```javascript
// Core
const LoginSystem = {
  plugins: {},

  register(name, plugin) {
    this.plugins[name] = plugin;
  },

  login(type) {
    this.plugins[type].login();
  }
};

// Plug-ins
const GoogleLogin = {
  login() {
    console.log("Login com Google");
  }
};

const FacebookLogin = {
  login() {
    console.log("Login com Facebook");
  }
};

// Registro
LoginSystem.register("google", GoogleLogin);
LoginSystem.register("facebook", FacebookLogin);

// Uso
LoginSystem.login("google");
```

### O que acontece?

- O **Core** possui uma lista de plug-ins.
- O **FedexPlugin** é registrado nessa lista.
- Quando o Core precisa calcular o frete, ele chama o plug-in correspondente.
- Se amanhã surgir outro tipo de frete, basta criar um novo plug-in.

---

# 4. Exemplo em React

```jsx
function Dashboard({ modules }) {
  const registry = {
    users: <Users />,
    rewards: <Rewards />,
    events: <Events />
  };

  return (
    <>
      {modules.map((module) => (
        <div key={module}>
          {registry[module]}
        </div>
      ))}
    </>
  );
}
```

### O que acontece?

O Dashboard funciona como o **Core**.

Cada componente (`WeatherPlugin`, `StockPlugin`) é um plug-in.

Dependendo do que estiver ativo, o Dashboard mostra apenas os componentes registrados.

---

# 5. Vantagens e Desvantagens

| Característica | Avaliação | Explicação |
|---------------|-----------|------------|
| **Custo** | ⭐⭐⭐⭐⭐ | Barato e simples de desenvolver. |
| **Simplicidade** | ⭐⭐⭐⭐ | Muito fácil de entender e implementar. |
| **Modularidade** | ⭐⭐⭐ | As funcionalidades ficam separadas em plug-ins. |
| **Facilidade de evolução** | ⭐⭐⭐ | É simples adicionar ou remover plug-ins. |
| **Testes** | ⭐⭐⭐ | Cada plug-in pode ser testado separadamente. |
| **Escalabilidade** | ⭐ | Não é a melhor opção para sistemas com milhões de usuários. |
| **Tolerância a falhas** | ⭐ | Se o Core parar, todo o sistema para. |

---

# Quando usar?

Use o **Microkernel** quando:

- O sistema precisa ser personalizado para diferentes clientes.
- Novas funcionalidades serão adicionadas com frequência.
- Você quer manter o sistema organizado e fácil de manter.

Exemplos:

- VS Code (extensões)
- Google Chrome (extensões)
- Sistemas ERP com módulos opcionais
- Plataformas que permitem instalar novos recursos

---

# Quando não usar?

Evite esse estilo quando o sistema precisa atender um número muito grande de usuários ao mesmo tempo.

Nesses casos, arquiteturas como **Microserviços** costumam ser mais indicadas.

---

# Resumo rápido

- O **Core** contém apenas as funções principais.
- Os **Plug-ins** adicionam novas funcionalidades.
- O **Registry** guarda os plug-ins disponíveis.
- Os **Contratos (Interfaces)** garantem que todos os plug-ins funcionem da mesma forma.
- É uma arquitetura simples, organizada e fácil de expandir, mas não é a melhor escolha para sistemas que precisam de alta escalabilidade.