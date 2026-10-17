# CLAUDE.md — smart-task-routing-powerautomate

Case de automação Power Automate: Adaptive Card no MS Teams cria tarefas categorizadas no MS Planner e devolve deep link no chat.

## Conteúdo

- `Power_Automate_Case.json`: definição do fluxo exportada e sanitizada (placeholders como `[FLOW_ID]`, `[CONNECTION_ID]`).
- `README.md`: problema de negócio, arquitetura e decisões (ex.: `split()` para evitar falso positivo em tags).

## Como visualizar

- Ler o README no GitHub; inspecionar o JSON com `jq . Power_Automate_Case.json`.
- Não há código executável; o fluxo é importado no Power Automate.

## Observações

- O README cita `automacao-teams-planner.zip` e pasta `/src`, que não existem no repo; o JSON está na raiz.
- Nunca colocar IDs reais de tenant/grupo/conexão no JSON.

## Fluxo de trabalho

- Autonomia para commitar e dar push na main (projeto pessoal).
- Responder sempre em português do Brasil.
