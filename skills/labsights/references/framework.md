# Framework LabSights 2.0 — Referência Interpretativa

Este framework define **como pensar** sobre os dados do cliente. Ele não mudou com o LabSights 3.0 — o que mudou foi só a forma de entrega (Dashboard HTML interativo, não mais texto/slides). Use este documento para decidir o que a narrativa de cada camada deve dizer.

## Filosofia central

A maioria das agências mostra **gráficos soltos**. O cliente vê números, mas não entende a história. O objetivo do LabSights é transformar o relatório em uma **narrativa de marketing**: poucos números, mais interpretação.

O cliente não pensa em métricas. Ele pensa em perguntas:
- "As pessoas estão vendo minha empresa?"
- "Elas estão se interessando?"
- "Isso está virando cliente?"

O LabSights responde exatamente essas três perguntas.

---

## Estrutura: 3 Camadas do Funil

### 🔵 CAMADA 1 — VISIBILIDADE
**Pergunta:** A marca está sendo vista?
**Posição no funil:** Topo

KPIs:
- Visualizadores (pessoas únicas alcançadas)
- Visualizações (total de impressões de conteúdo)
- Crescimento de seguidores

Modelo de leitura narrativa:
> "Neste mês [CLIENTE] alcançou [X] pessoas únicas, com [Y] visualizações de conteúdo, fortalecendo a presença da marca e ampliando o reconhecimento [do segmento] na região."

O cliente entende: **"Estamos aparecendo."**

---

### 🟡 CAMADA 2 — INTERESSE
**Pergunta:** As pessoas estão prestando atenção?
**Posição no funil:** Meio

KPIs:
- Interações com conteúdo (curtidas, comentários, compartilhamentos, salvamentos)
- Visitas à página / perfil

Modelo de leitura narrativa:
> "O conteúdo gerou [X] interações e [Y] visitas à página, indicando que o público não apenas viu a marca, mas demonstrou interesse em conhecer melhor [o produto/serviço]."

O cliente entende: **"As pessoas estão olhando."**

---

### 🔴 CAMADA 3 — INTENÇÃO
**Pergunta:** As pessoas querem falar com a empresa?
**Posição no funil:** Fundo / Resultado comercial

KPIs:
- Cliques no link
- Conversas iniciadas (WhatsApp, DMs, formulários)
- Custo por conversa (quando há tráfego pago)
- Métricas de campanha (ver `dashboard_data_contract.md` para a estrutura de `campanhas`)

Modelo de leitura narrativa:
> "As campanhas geraram [X] cliques em links e [Y] conversas iniciadas, convertendo o interesse em potenciais oportunidades de negócio."

O cliente entende: **"Estamos gerando contatos."**

**Nota importante:** nem toda campanha ativa no mês gera conversas — uma campanha de reconhecimento de marca, por exemplo, é medida por alcance e frequência, não por conversas. Isso é normal e não deve ser tratado como "dado faltando" — reflita isso na narrativa e na métrica de resultado principal daquela campanha específica.

---

## ⚡ Velocidade do Funil

Indicador que mostra **onde o marketing está travando**: topo, meio ou fundo.

O funil é sempre um **array dinâmico de estágios** — o número de estágios e quais métricas aparecem variam mês a mês, conforme o objetivo da campanha ativa naquele ciclo. Nunca trate isso como uma estrutura fixa de N campos.

Diagnóstico:
- Queda grande no **meio** → problema de **conteúdo** (não está engajando)
- Queda grande no **final** → problema de **oferta ou anúncio** (não está convertendo)

Decisão estratégica que a narrativa deve sugerir:
- Investir em **conteúdo**, **tráfego** ou **conversão**?

---

## Profundidade por plano da Sevenlab

| Plano | Camadas | Extras |
|---|---|---|
| **Visibilidade** | Camada 1 + Camada 2 | Sem fundo de funil |
| **Posicionamento** | Camadas 1 + 2 + 3 | Padrão completo |
| **Expansão** | Camadas 1 + 2 + 3 | + Análise de campanhas |
| **Referência** | Camadas 1 + 2 + 3 | + Funil completo + Comparativo mensal + Insights estratégicos |

---

## Regra de ouro

> Relatório bom mostra **dados**.
> Relatório excelente mostra **causa e efeito**.
> Quando você organiza números em uma jornada, o cliente entende o valor do marketing sem você precisar explicar.
