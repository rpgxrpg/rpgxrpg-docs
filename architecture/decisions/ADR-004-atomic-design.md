# ADR-004: Atomic Design como metodologia de componentes

**Status:** Aceito
**Data:** 2026-08-07
**Proprietário ADR:** Luis Henrique S. Carvalho

# Contexto

O rpgxrpg-web parte do zero, sem padrão de organização de componentes definido. O sistema de fichas (Módulo 3 do MVP) é o módulo com maior superfície de UI: personagem, npc e invocação compartilham praticamente os mesmos blocos de campos (atributos, habilidades, antecedentes, XP), variando apenas em regras de edição e em quem pode criar cada tipo. Sem uma metodologia de componentização, esse compartilhamento tende a virar cópia e colagem entre as três telas de ficha, gerando divergência visual e retrabalho a cada ajuste de regra do sistema MXM.

A decisão precisava considerar: reuso de componentes entre os três tipos de ficha, consistência visual entre módulos (fichas, campanhas, painel do Mestre), facilidade de manutenção conforme novas telas são adicionadas, e familiaridade da equipe com a abordagem escolhida.

# Decisão

Adotar Atomic Design (atoms → molecules → organisms → templates → pages) como metodologia de organização de componentes do rpgxrpg-web.

- **Atoms** — elementos mínimos: botão, input, label, badge de atributo.
- **Molecules** — combinações simples: campo de atributo com label + valor + botão de incremento.
- **Organisms** — blocos completos reutilizáveis entre os três tipos de ficha: bloco de atributos, bloco de habilidades, card de arma equipada.
- **Templates** — esqueleto de layout de cada tela (ficha, campanha, painel do Mestre), sem dado real.
- **Pages** — instância real do template com dados carregados da API.

A escolha foi motivada por:

- Familiaridade prévia com a metodologia, sem curva de aprendizado adicional;
- Reuso direto de organisms entre personagem, npc e invocação — hoje a maior fonte potencial de retrabalho do front-end;
- Fronteiras claras entre níveis facilitam decidir onde um novo campo de ficha deve entrar sem duplicar código.

## Alternativas descartadas

- **Organização por tipo de arquivo** (`components/`, `hooks/`, `pages/`): mais simples de iniciar, mas não deixa explícito o relacionamento de composição entre componentes — à medida que os três tipos de ficha crescem, fica difícil saber o que é seguro reutilizar sem quebrar outra tela.
- **Organização por feature** (`features/ficha/`, `features/campanha/`): isola bem o domínio de cada módulo, mas duplica componentes visuais pequenos (inputs, botões, badges) que são idênticos entre features, indo contra o objetivo de reuso entre os tipos de ficha.

# Consequências

## Positivas

- Blocos de ficha (atributos, habilidades, antecedentes) são construídos uma vez como organisms e reaproveitados nos três tipos de ficha;
- Evolução incremental — dá pra somar o Módulo 4 (Painel do Mestre) reaproveitando atoms/molecules já existentes;
- Baixo atrito de adoção, já que a metodologia já foi usada antes em outros projetos.

## Negativas

- Exige disciplina nas dependências unidirecionais entre níveis — atoms não importam molecules, molecules não importam organisms;
- Classificar um componente novo como molecule ou organism pode ficar subjetivo conforme a UI cresce, especialmente nas telas de ficha, que têm muitos blocos parecidos;
- Esforço inicial de montar os atoms/molecules base antes de qualquer tela completa existir.

# Conformidade

- Componentes devem respeitar a hierarquia: atoms não importam molecules/organisms, molecules não importam organisms;
- Atoms e molecules devem ser preferencialmente puros — sem chamada de API direta;
- Chamadas à API do backend ficam encapsuladas nas pages ou nos organisms que efetivamente as consomem;
- Qualquer mudança estrutural relevante nessa organização deve ser formalizada em nova ADR.

# Observações

- Gerenciamento de estado global ainda não foi decidido — fica para uma ADR própria quando a necessidade aparecer (o MVP hoje não exige estado compartilhado complexo entre telas);
- O design system (tokens, paleta) deve ser documentado em `design/` junto do repositório, para os atoms consumirem tokens em vez de valores fixos;
- Esta ADR deve ser revisitada se o volume de telas do painel do Mestre ou do catálogo de armas exigir um nível de composição que o Atomic Design não cubra bem.

# Direcionadores

- **Redução de retrabalho** — reuso de organisms entre os três tipos de ficha evita reescrever os mesmos blocos três vezes;
- **Familiaridade** — metodologia já usada em projetos anteriores, sem curva de aprendizado;
- **Consistência visual** — mesma base de atoms/molecules entre todas as telas do sistema;
- **Evolução incremental** — novas telas (painel do Mestre, catálogo de armas) se apoiam nos níveis já existentes sem reestruturação.
