## 💡 Relacionando com arquiteturas

Os conceitos de modularidade também aparecem nas arquiteturas mais conhecidas.

### Monólito

Uma única aplicação onde todas as funcionalidades ficam juntas.

Exemplo:

```text
Sistema

├── Usuários
├── Produtos
├── Pedidos
└── Pagamentos
```

É mais simples de desenvolver e normalmente é a melhor escolha para sistemas pequenos ou no início de um projeto.

---

### Monólito Modular

Continua sendo uma única aplicação, porém organizada em módulos bem separados.

Exemplo:

```text
Sistema

├── modulo-usuarios
├── modulo-produtos
├── modulo-pedidos
└── modulo-pagamentos
```

Cada módulo possui sua própria responsabilidade, o que aumenta a organização e facilita a manutenção.

---

### Microserviços

Cada módulo torna-se uma aplicação independente.

Exemplo:

```text
API Usuários

API Produtos

API Pedidos

API Pagamentos
```

Cada serviço possui seu próprio deploy e pode até ter seu próprio banco de dados.

Essa arquitetura é mais indicada para sistemas grandes, mas também aumenta bastante a complexidade.

---

### Relação com a modularidade

A modularidade é importante em qualquer arquitetura.

Uma prática comum é:

Monólito → Monólito Modular → Microserviços

Ou seja, primeiro organizamos bem o código em módulos. Somente quando o sistema cresce e realmente existe necessidade, alguns módulos podem ser transformados em microserviços.