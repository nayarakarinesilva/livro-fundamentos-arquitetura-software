# Exemplo: Arquitetura em Camadas

Um projeto React organizado em camadas pode ficar assim:

```text
src/
│
├── pages/           ← Apresentação
├── components/
│
├── services/        ← Regras de negócio
│
├── repositories/    ← Acesso aos dados
│
├── api/
│
└── App.jsx
```

Cada pasta possui uma responsabilidade específica.

- **pages/components:** telas e interface do usuário.
- **services:** regras de negócio e validações.
- **repositories:** busca e salva dados na API.
- **api:** configuração da comunicação com o backend.

---

## Fluxo da requisição

Quando um usuário abre a tela de usuários:

```text
Users.jsx
     │
     ▼
UserService.js
     │
     ▼
UserRepository.js
     │
     ▼
API / Banco de Dados
```

A resposta retorna pelo mesmo caminho.

---

## Camada Fechada

Toda requisição passa por todas as camadas.

```text
Apresentação
      │
      ▼
Negócio
      │
      ▼
Persistência
      │
      ▼
Banco de Dados
```

---

## Camada Aberta

Algumas funcionalidades podem ser acessadas diretamente.

```text
Apresentação
     ├── Service
     ├── Cache
     └── Logger
```

---

## Sinkhole (Antipadrão)

Acontece quando as camadas apenas repassam a chamada, sem fazer nenhum processamento.

```text
Tela
 │
 ▼
Service
 │
 ▼
Repository
 │
 ▼
Banco
```

Se `Service` e `Repository` apenas chamam a próxima camada, elas estão adicionando complexidade sem trazer benefício.
