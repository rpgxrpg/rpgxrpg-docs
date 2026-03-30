# 🎴 RPGxRPG — Documentação

Documentação central do **RPGxRPG**, uma aplicação web e mobile para gerenciar campanhas,
fichas de personagem e sessões de um RPG de mesa baseado no universo de *Hunter x Hunter*,
de Yoshihiro Togashi.

O sistema de regras — chamado **MXM (Marcos X Marcos)** — foi construído sobre a base do
*Vampiro: A Máscara 5ª Edição* (White Wolf Publishing), adaptado para o universo de HxH.

---

## 🗂️ Repositórios

| Repositório | Descrição |
|---|---|
| 📄 [`rpgxrpg-docs`](.) | Documentação, ADRs, contratos de API e sistema de regras |
| ⚙️ `rpgxrpg-backend` | API REST + WebSocket (Node.js, TypeScript, Express, PostgreSQL) |
| 🌐 `rpgxrpg-web` | Frontend web (React + TypeScript) |
| 📱 `rpgxrpg-mobile` | Frontend mobile (Flutter) |

---

## 📁 Estrutura deste repositório
```
rpgxrpg-docs/
├── 📐 architecture/    # Visão geral da arquitetura e ADRs
├── 🔌 api/             # Contrato das rotas e payloads
├── 🗄️  database/       # Documentação do schema do banco
├── 🃏 game-system/     # Sistema de regras MXM e fichas de personagem
└── 🎯 mvp/             # Escopo e requisitos do MVP
```

---

## 🧱 Sobre o projeto

Este projeto é desenvolvido como **portfólio** e **ferramenta real de jogo**, com foco em
boas práticas de engenharia de software: Clean Architecture, separação de responsabilidades,
decisões documentadas e evolução incremental.

> 💡 Para entender as decisões técnicas tomadas ao longo do projeto, veja a pasta [`architecture/decisions`](./architecture/decisions/).
