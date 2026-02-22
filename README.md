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

> 📸 **[SUGESTÃO DE PRINT 1]** > *Insira aqui uma captura de tela limpa mostrando o fluxo completo (a árvore de ações) no Power Automate.*
> 

## 🚧 Desafios Técnicos e Decisões de Produto

Como Technical Product Manager, o maior desafio foi equilibrar a viabilidade técnica com o valor entregue ao usuário final. Destaco dois momentos críticos de engenharia e produto neste projeto:

### 1. Tratamento de Dados e Bug de Falso Positivo (QA)
* **O Incidente:** O conector do Teams retorna múltiplas tags selecionadas como uma string única (ex: `"category2,category20"`). A função nativa `contains` gerava falsos positivos, marcando a `category2` como verdadeira sempre que a `category20` estava presente.
* **A Solução:** Implementei a função `split()` no Power Automate para quebrar a string nas vírgulas e forçar a conversão para um Array rigoroso, garantindo a validação da string exata e a integridade dos dados inseridos no Planner.

> 📸 **[SUGESTÃO DE PRINT 2]** > *Insira aqui um print focado na caixinha de expressão (fx) mostrando o uso da sua fórmula com o `split` e o `contains`.*
> 

### 2. Limitações de Infraestrutura vs. Foco em ROI (Workaround)
* **O Bloqueio:** Para forçar o Planner a mostrar a descrição na capa do card automaticamente, era necessário um comando HTTP PATCH via Microsoft Graph API. No entanto, conectores *Standard* do ecossistema possuem restrições rígidas de URI (exigindo rotas `/groups/`).
* **Decisão de Produto:** Em vez de onerar o projeto exigindo licenciamento Premium ou a criação de um App no Microsoft Entra ID (Azure AD), tomei a decisão de pivotar a interface final. Criei um workaround onde a automação compensa a limitação visual gerando dinamicamente um link rastreável no Teams que injeta o usuário diretamente dentro do card expandido. Redução de complexidade técnica com manutenção da excelente experiência do usuário.

> 📸 **[SUGESTÃO DE PRINT 3]** > *Insira aqui um print da tela do Teams mostrando a mensagem final do Flow Bot com o link bonitinho em azul sendo gerado.*
> 

## 📦 Como Implementar (Deploy)
Para importar esta solução no seu ambiente corporativo:
1. Baixe o pacote `automacao-teams-planner.zip` ou o `definition.json` (já sanitizado) na pasta `/src` ou na aba *Releases*.
2. No Power Automate, navegue até **Importar > Importar Pacote**.
3. Faça o upload e mapeie as suas próprias conexões do MS Teams e MS Planner.
4. Altere as variáveis genéricas (como `[ID_DO_GRUPO]`) para os IDs do seu ambiente.

---
**Desenvolvido por Arlindo Júnior Honorato** *Focado em construir automações robustas, inteligência artificial aplicada e produtos que escalam operações financeiras.* 🔗 [Conecte-se comigo no LinkedIn] (Link_do_seu_perfil)
