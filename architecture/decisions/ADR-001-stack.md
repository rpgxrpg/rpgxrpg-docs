# ADR-001 — Escolha de Stack Tecnológica

**Projeto:** RPGxRPG  
**Status:** Aceito  
**Data:** 2026-03-30  
**Proprietário:** Luis Henrique S. Carvalho  

---

## Contexto

O projeto RPGxRPG é composto por três frentes: uma API back-end, um cliente web e um aplicativo mobile. A stack deveria considerar o objetivo duplo do projeto — ser uma ferramenta real de jogo **e** um portfólio técnico — além da necessidade de rodar em múltiplos computadores com ambiente reproduzível.

Outro fator determinante: o projeto é desenvolvido individualmente, com foco deliberado em aprender a raciocinar sobre decisões de arquitetura. Por isso, frameworks que abstraem demais foram evitados.

As opções avaliadas foram:

- **Back-end:** NestJS vs Express puro (ambos Node.js + TypeScript)
- **Mobile:** Flutter vs React Native
- **Banco:** vide ADR-002

---

## Decisão

Foi decidido utilizar **Express** para o back-end, **Flutter** para o mobile e **React** para o web, todos com TypeScript onde aplicável.

### Express com TypeScript (back-end)

- Controle total sobre a estrutura do projeto, sem convenções impostas pelo framework;
- Permite raciocinar sobre cada camada da arquitetura diretamente, sem "mágica" de injeção de dependência;
- NestJS foi explicitamente descartado neste projeto por ser usado em outros contextos universitários — a intenção aqui é aprender os fundamentos que o NestJS abstrai.

### Flutter (mobile)

- Cross-platform nativo para Android e iOS a partir de uma única base de código;
- Dart tem curva de aprendizado baixa para quem já conhece linguagens tipadas;
- Performance próxima de código nativo sem necessidade de bridges JavaScript.

### React com TypeScript (web)

- Ecossistema consolidado com bibliotecas maduras;
- Componentização natural para interfaces de fichas e painéis;
- Amplamente adotado no mercado, reforçando o valor do portfólio.

### Alternativas descartadas

- **NestJS:** descartado intencionalmente para forçar o aprendizado das camadas que ele abstrairia;
- **React Native:** Flutter foi preferido por oferecer melhor performance e não depender de bridge JavaScript.

---

## Consequências

### Positivas

- Controle total sobre decisões de arquitetura, maximizando o aprendizado;
- TypeScript em toda a stack reduz erros em tempo de execução por tipagem estática;
- Docker garante reprodutibilidade do ambiente entre múltiplas máquinas de desenvolvimento.

### Negativas

- Express puro exige mais configuração inicial do que NestJS;
- Manter duas frentes de front (React + Flutter) exige disciplina para não duplicar lógica de negócio.

---

## Conformidade

- O back-end deverá ser desenvolvido em Express com TypeScript, sem adoção de outros frameworks Node;
- O banco de dados deverá rodar via Docker para garantir portabilidade;
- Qualquer alteração significativa na stack deverá ser formalizada em nova ADR.

---

## Direcionadores

- **Aprendizado intencional:** escolhas que maximizam o entendimento dos fundamentos de engenharia;
- **Portabilidade:** Docker garante que o ambiente funcione igual em qualquer máquina;
- **Valor de portfólio:** stack moderna e amplamente adotada no mercado.
