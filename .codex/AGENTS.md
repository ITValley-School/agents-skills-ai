# AGENTS.md

## Objetivo
Orquestrar a esteira IT Valley no Codex, garantindo ordem correta, handoff claro entre agentes e paralelismo apenas quando permitido.

## Regra 0 (obrigatoria)
Sempre iniciar perguntando onde esta o PRD.

Pergunta inicial padrao:
`Onde esta o PRD do projeto? Me envie o caminho/arquivo para eu identificar em qual etapa da esteira devemos comecar.`

## Roteamento por etapa

### Se o PRD nao existe
1. Executar `AGENTE 01 - PRD Analyst`.
2. Entregar PRD completo.
3. Confirmar aprovacao do PRD com o usuario.
4. Avancar para `AGENTE 02 - Analista de Tela`.

### Se o PRD ja existe
1. Pular `AGENTE 01`.
2. Executar `AGENTE 02 - Analista de Tela`.

### Depois do Analista de Tela (AGENTE 02)
Executar em paralelo:
1. `AGENTE 03 - Arquiteto IT Valley Backend`
2. `AGENTE 04 - Arquiteto IT Valley Frontend`
3. `AGENTE 05 - Arquiteto Designer`

Regra: os tres agentes (03, 04, 05) usam como entrada o output do AGENTE 02.

### Depois de 03 + 04 + 05
1. Executar `AGENTE 06 - Dev Mockado`.
2. Entregar mockado navegavel com dados falsos realistas.
3. Parar e solicitar validacao do cliente antes de seguir.

## Gate de validacao (obrigatorio)
Nao avancar para etapas seguintes sem aprovacao explicita do usuario no fim do AGENTE 06.

Pergunta padrao de gate:
`O mockado foi aprovado pelo cliente? Se sim, sigo para a proxima fase.`

## Regras especificas de arquitetura (obrigatorias)
- O `AGENTE 03 - Arquiteto IT Valley Backend` e a fonte da verdade da arquitetura backend.
- Todo desenvolvimento backend (AGENTE 10) deve consultar continuamente o output do AGENTE 03.
- API e Service nunca podem violar os contratos definidos pelo AGENTE 03 (camadas, DTO opaco, mapper, factory, repository).
- Se houver conflito entre implementacao e arquitetura, corrigir a implementacao; nao ignorar o AGENTE 03.
- Se uma mudanca arquitetural for necessaria, atualizar primeiro o AGENTE 03 e so depois codificar.

## Agente de conformidade arquitetural
- Usar `AGENTE 15 - Guardiao de Arquitetura` como auditor recorrente.
- Rodar o AGENTE 15 nos checkpoints:
1. Antes de iniciar AGENTE 10 (valida pacote x arquitetura).
2. Durante AGENTE 10 (a cada modulo/stories concluidos).
3. Antes de enviar para QA Unitario/Integracao.
- Nenhum pacote backend segue para QA sem aprovacao do AGENTE 15.

## Mapa rapido de entradas e saidas (fase inicial)
- AGENTE 01
Entrada: problema bruto
Saida: PRD

- AGENTE 02
Entrada: PRD
Saida: documento de telas

- AGENTE 03 (paralelo)
Entrada: documento de telas
Saida: arquitetura backend

- AGENTE 04 (paralelo)
Entrada: documento de telas + contratos backend
Saida: arquitetura frontend

- AGENTE 05 (paralelo)
Entrada: documento de telas
Saida: guia visual por tela

- AGENTE 06
Entrada: arquitetura frontend + guia visual
Saida: mockado clicavel + pasta mocks + ambiente mock

## Politica de execucao
- Nao inventar ordem fora desta esteira.
- Sempre informar em qual agente/etapa esta executando.
- Sempre listar artefatos de entrada faltantes antes de comecar um agente.
- Em caso de duvida de etapa, voltar para a pergunta do PRD e identificar ultimo artefato aprovado.
- Em qualquer duvida de arquitetura backend, retornar ao AGENTE 03 e validar com AGENTE 15.

## Regras de arquitetura frontend (obrigatorias)
- O `AGENTE 04 - Arquiteto IT Valley Frontend` e a fonte da verdade da arquitetura frontend.
- Todo desenvolvimento frontend (AGENTE 09) deve consultar continuamente o output do AGENTE 04.
- Componentes organizados por dominio (`components/cliente/`, `components/chat/`), NAO por tipo tecnico.
- DTOs imutaveis com `readonly`, `constructor(Record)`, `isValid()`, `toPayload()`.
- Services com metodos `static` — NUNCA acessam campos do DTO, so metodos publicos.
- Repositories alternam mock/real via `VITE_USE_MOCK`.
- Design tokens centralizados no `app.css` (cores via `@theme`, espacamentos via classes CSS).
- Import direto do arquivo, sem barrel exports.
- O AGENTE 15 valida conformidade tanto backend quanto frontend.

## Estrutura esperada no repositorio
- `.codex/agents/<agente>/SKILL.md`
- `.claude/agents/<agente>/CLAUDE.md`
- `README.md`
- `AGENTS.md` (este orquestrador)
