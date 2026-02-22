# ⚙️ Smart Triage: Automação de Microsoft Teams para Planner

![Power Automate](https://img.shields.io/badge/Power_Automate-0066FF?style=for-the-badge&logo=powerautomate&logoColor=white)
![Microsoft Teams](https://img.shields.io/badge/Microsoft_Teams-6264A7?style=for-the-badge&logo=microsoft-teams&logoColor=white)
![Microsoft Planner](https://img.shields.io/badge/Microsoft_Planner-31752F?style=for-the-badge&logo=microsoft&logoColor=white)

Este projeto é uma solução de automação assíncrona desenvolvida para eliminar gargalos operacionais e de triagem, integrando a captura de dados via Cartões Adaptáveis (Adaptive Cards) no MS Teams com a gestão de tarefas automatizada no MS Planner.

## 🎯 O Problema de Negócio (Contexto)
Em operações de back-office complexas (como processos financeiros e de seguros), a entrada de dados não estruturados via chat gera perda de informações, lentidão na triagem e falta de visibilidade. A equipe precisava de uma interface rápida para registrar requisições no Teams e garantir que elas fossem categorizadas corretamente e transformadas em tarefas acionáveis, sem esforço manual.

## 💡 A Solução e Arquitetura
Criei um fluxo no Power Automate que atua como um *middleware* inteligente. Ele intercepta as respostas do Teams, processa arrays de dados dinâmicos e se comunica com a Graph API para orquestrar a criação do card no Planner.

**Fluxo de Execução:**
1. O usuário preenche um Adaptive Card direto no chat do Teams.
2. O Power Automate captura o JSON de resposta.
3. A automação processa as tags selecionadas e cria a tarefa no Planner.
4. O robô retorna uma notificação rica em HTML no Teams com um *Deep Link* direto para o card criado.

<img width="903" height="1588" alt="tmp_29ccb96c-91ad-4e71-8bca-c8ee5b82c030" src="https://github.com/user-attachments/assets/2d14e706-939c-4e82-bb4a-25dc027c24b4" />

## 🚧 Desafios Técnicos e Decisões de Produto

Como Technical Product Manager, o maior desafio foi equilibrar a viabilidade técnica com o valor entregue ao usuário final. Destaco dois momentos críticos de engenharia e produto neste projeto:

### 1. Tratamento de Dados e Bug de Falso Positivo (QA)
* **O Incidente:** O conector do Teams retorna múltiplas tags selecionadas como uma string única (ex: `"category2,category20"`). A função nativa `contains` gerava falsos positivos, marcando a `category2` como verdadeira sempre que a `category20` estava presente.
* **A Solução:** Implementei a função `split()` no Power Automate para quebrar a string nas vírgulas e forçar a conversão para um Array rigoroso, garantindo a validação da string exata e a integridade dos dados inseridos no Planner.

<img width="1386" height="851" alt="tmp_5843d8d5-1877-4a3a-a0fe-37848f3ebafa" src="https://github.com/user-attachments/assets/2cb4e2e5-4569-4de3-95f9-66ac4fe7ffbc" />


### 2. Limitações de Infraestrutura vs. Foco em ROI (Workaround)
* **O Bloqueio:** Para forçar o Planner a mostrar a descrição na capa do card automaticamente, era necessário um comando HTTP PATCH via Microsoft Graph API. No entanto, conectores *Standard* do ecossistema possuem restrições rígidas de URI (exigindo rotas `/groups/`).
* **Decisão de Produto:** Em vez de onerar o projeto exigindo licenciamento Premium ou a criação de um App no Microsoft Entra ID (Azure AD), tomei a decisão de pivotar a interface final. Criei um workaround onde a automação compensa a limitação visual gerando dinamicamente um link rastreável no Teams que injeta o usuário diretamente dentro do card expandido. Redução de complexidade técnica com manutenção da excelente experiência do usuário.

<img width="424" height="287" alt="tmp_7f8ea58d-5637-42fd-b832-706875eac0d2" src="https://github.com/user-attachments/assets/cc356708-8611-4e6c-94d5-9a66dda24ca5" />


## 📦 Como Implementar (Deploy)
Para importar esta solução no seu ambiente corporativo:
1. Baixe o pacote `automacao-teams-planner.zip` ou o `definition.json` (já sanitizado) na pasta `/src` ou na aba *Releases*.
2. No Power Automate, navegue até **Importar > Importar Pacote**.
3. Faça o upload e mapeie as suas próprias conexões do MS Teams e MS Planner.
4. Altere as variáveis genéricas (como `[ID_DO_GRUPO]`) para os IDs do seu ambiente.

---
​👤 Autor​Arlindo Júnior Honorato Technical Product Manager | Automação | IA aplicada a Produtos Financeiros e Eficiência de Backoffice
