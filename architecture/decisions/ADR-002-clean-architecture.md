# ADR-002 — Adoção de Clean Architecture

**Projeto:** RPGxRPG  
**Status:** Aceito  
**Data:** 2026-03-30  
**Proprietário:** Luis Henrique S. Carvalho  

---

## Contexto

Com a escolha do Express puro (sem NestJS), a estrutura do back-end precisava ser definida manualmente. Sem uma convenção imposta pelo framework, o risco de o código virar uma mistura de regras de negócio, chamadas de banco e lógica HTTP no mesmo lugar é alto.

Era necessário adotar um modelo arquitetural que separasse as responsabilidades de forma clara e que pudesse ser explicado e justificado — não apenas seguido mecanicamente.

---

## Decisão

Foi adotada uma **Clean Architecture em 4 camadas**, com duas entradas paralelas na camada de Apresentação.
```
Presentation (HTTP + WebSocket)
    ↓
Application (casos de uso)
    ↓
Domain (regras de negócio puras)
    ↓
Infrastructure (banco, ORM, integrações externas)
```

### Regra fundamental

As dependências apontam sempre **para dentro**. O Domain não conhece o Prisma. O Application não conhece o Express. Cada camada só enxerga a camada imediatamente abaixo dela.

### Presentation — duas entradas

- `http/controllers` — recebe requisições HTTP e chama o Application;
- `websocket/handlers` — recebe eventos WebSocket e chama o mesmo Application.

As duas entradas compartilham toda a lógica abaixo. HTTP e WebSocket são apenas canais de entrada diferentes para os mesmos casos de uso.

### Domain

Coração do sistema. Contém regras puras: calcular dano, validar custo de Aura, processar sucessos líquidos. Nenhuma dependência de framework ou banco de dados.

### Infrastructure

Tudo que é externo: repositórios com Prisma, envio de e-mail, integrações futuras. É a única camada que pode conhecer o Prisma diretamente.

---

## Consequências

### Positivas

- Regras de negócio testáveis isoladamente, sem precisar subir banco ou servidor;
- Troca de framework ou ORM afeta apenas a camada de Infrastructure;
- A separação clara torna o código mais legível e navegável conforme o projeto cresce.

### Negativas

- Mais arquivos e pastas para funcionalidades simples — há um custo de verbosidade real;
- Exige disciplina para não "atalhar" dependências entre camadas.

---

## Conformidade

- Nenhuma regra de negócio deverá residir em controllers ou handlers;
- O Domain não poderá importar nada de Infrastructure ou de bibliotecas externas;
- Middleware de autenticação (JWT) pertence à camada de Presentation e não deverá vazar para o Application ou Domain;
- Qualquer mudança de abordagem arquitetural deverá ser formalizada em nova ADR.

---

## Direcionadores

- **Aprendizado intencional:** a separação forçada de camadas obriga o raciocínio sobre onde cada responsabilidade vive;
- **Testabilidade:** Domain isolado permite testes unitários sem dependências externas;
- **Escalabilidade futura:** a separação de canais (HTTP/WebSocket) na Presentation facilita a adição de features em tempo real sem reestruturar o núcleo.
