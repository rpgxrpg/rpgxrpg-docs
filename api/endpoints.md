# 🔌 Contrato da API — RPGxRPG

Este documento descreve os endpoints da API REST do RPGxRPG. É a fonte de verdade para
comunicação entre backend e frontends (web e mobile).

> **Status:** 🚧 Em construção — endpoints são documentados conforme o backend avança.

---

## 📌 Convenções

**Base URL:** `http://localhost:3000` (desenvolvimento)

**Autenticação:** rotas protegidas exigem o header:
```
Authorization: Bearer <token>
```

**Formato:** todas as requisições e respostas usam `Content-Type: application/json`.

**Padrão de resposta de erro:**
```json
{
  "error": "mensagem descritiva do erro"
}
```

---

## 🔐 Módulo 1 — Autenticação

### `POST /auth/register`
Cria uma nova conta de usuário.

**Body:**
```json
{
  "nome_usuario": "string",
  "email": "string",
  "senha": "string"
}
```

**Resposta 201:**
```json
{
  "message": "Conta criada. Verifique seu e-mail para ativar."
}
```

**Erros:**
| Status | Motivo |
|---|---|
| 400 | Campos obrigatórios ausentes ou inválidos |
| 409 | E-mail já cadastrado |

---

### `POST /auth/login`
Autentica um usuário e devolve um token JWT.

**Body:**
```json
{
  "email": "string",
  "senha": "string"
}
```

**Resposta 200:**
```json
{
  "token": "string"
}
```

**Erros:**
| Status | Motivo |
|---|---|
| 401 | Credenciais inválidas |
| 403 | E-mail ainda não verificado |

---

## 🗺️ Módulo 2 — Campanhas

### `POST /campaigns`
🔒 Cria uma nova campanha. O usuário autenticado vira automaticamente o Mestre.

**Body:**
```json
{
  "titulo": "string",
  "pontos_atributos": "integer",
  "pontos_habilidades": "integer",
  "pontos_antecedentes": "integer"
}
```

**Resposta 201:**
```json
{
  "id": "integer",
  "titulo": "string",
  "status": "ativa"
}
```

---

### `POST /campaigns/:id/invite`
🔒 Convida um jogador pelo e-mail. Apenas o Mestre pode convidar.

**Body:**
```json
{
  "email": "string"
}
```

**Resposta 200:**
```json
{
  "message": "Convite enviado."
}
```

**Erros:**
| Status | Motivo |
|---|---|
| 404 | Usuário não encontrado |
| 403 | Apenas o Mestre pode convidar |

---

### `GET /campaigns/:id`
🔒 Retorna os dados de uma campanha.

**Resposta 200:**
```json
{
  "id": "integer",
  "titulo": "string",
  "status": "string",
  "mestre": {
    "id": "integer",
    "nome_usuario": "string"
  }
}
```

---

## 🧾 Módulo 3 — Fichas

### `POST /campaigns/:id/sheets`
🔒 Cria uma nova ficha dentro de uma campanha.

**Body:**
```json
{
  "nome": "string",
  "tipo_ficha": "personagem | npc | invocacao",
  "atributos": {},
  "habilidades": {}
}
```

**Resposta 201:**
```json
{
  "id": "integer",
  "nome": "string",
  "tipo_ficha": "string",
  "status_aprovacao": "pendente"
}
```

---

### `GET /campaigns/:id/sheets`
🔒 Lista todas as fichas de uma campanha.

**Resposta 200:**
```json
[
  {
    "id": "integer",
    "nome": "string",
    "tipo_ficha": "string",
    "status_aprovacao": "string"
  }
]
```

---

### `GET /sheets/:id`
🔒 Retorna os dados completos de uma ficha.

**Resposta 200:** dados completos da ficha (a definir conforme schema evolui)

---

### `PUT /sheets/:id`
🔒 Edita uma ficha. A alteração entra na fila de aprovação do Mestre.

**Body:** campos alterados da ficha

**Resposta 200:**
```json
{
  "message": "Alteração enviada para aprovação."
}
```

---

## 👁️ Módulo 4 — Painel do Mestre

### `GET /campaigns/:id/approvals`
🔒 Lista as fichas com alterações pendentes de aprovação.

**Resposta 200:**
```json
[
  {
    "sheet_id": "integer",
    "nome": "string",
    "tipo_ficha": "string",
    "alterado_em": "datetime"
  }
]
```

---

### `POST /sheets/:id/approve`
🔒 Aprova a versão pendente de uma ficha. Apenas o Mestre.

**Resposta 200:**
```json
{
  "message": "Ficha aprovada."
}
```

---

### `POST /sheets/:id/reject`
🔒 Rejeita a versão pendente e reverte para o estado anterior. Apenas o Mestre.

**Resposta 200:**
```json
{
  "message": "Alteração rejeitada. Ficha revertida."
}
```

---

### `POST /campaigns/:id/xp`
🔒 Distribui XP para um personagem da campanha. Apenas o Mestre.

**Body:**
```json
{
  "personagem_id": "integer",
  "pontos_xp": "integer"
}
```

**Resposta 200:**
```json
{
  "message": "XP distribuído."
}
```

---

> 🔒 = rota protegida, exige token JWT no header.
