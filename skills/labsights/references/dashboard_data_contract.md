# Contrato de Dados — `DASHBOARD_DATA`

Este é o objeto JSON que a skill `/labsights` deve montar e injetar no `dashboard-model.html` através do placeholder literal `{{DASHBOARD_DATA_JSON}}`. É o único ponto de contato entre a interpretação do Claude e o template visual — o HTML nunca muda entre clientes ou meses, só este objeto muda.

## Estrutura completa

```json
{
  "cliente": {
    "nome": "", "saudacao": "Olá, [Primeiro Nome]!", "slug": "", "plano": "",
    "mes_referencia": "AAAA-MM", "mes_curto": "Nome do mês", "mes_extenso": "Nome do mês de AAAA",
    "titulo_mes": "Título/sinopse curta do mês", "sinopse": "Parágrafo de contexto do mês"
  },
  "navegacao": { "mes_anterior_label": "ex: Dez/Jan 2025-26" },
  "camadas": {
    "visibilidade": {
      "ativa": true, "tag": "Branding", "subtitulo_tag": "opcional",
      "narrativa": "", "kpis": [ { "label": "", "valor": "" } ],
      "tendencia": { "titulo": "", "labels": [], "valores": [] },
      "top_post": null
    },
    "interesse": { "...": "mesma estrutura de visibilidade" },
    "intencao": {
      "ativa": true, "tag": "Leads", "subtitulo_tag": "opcional",
      "narrativa": "", "kpis": [ { "label": "Conversas iniciadas (total)", "valor": "" } ],
      "campanhas": [
        {
          "nome": "", "objetivo": "",
          "serie_temporal": { "titulo": "", "labels": [], "valores": [] },
          "metricas": { "alcance": "", "impressoes": "", "valor_investido": "", "custo_por_resultado": "" },
          "granularidade": "semanal | diaria"
        }
      ]
    }
  },
  "velocidade_funil": {
    "estagios": [ { "label": "", "valor": "", "valor_abs": "opcional" } ],
    "diagnostico": ""
  },
  "conclusao": {
    "narrativa_final": "",
    "funcionou_bem": ["insight, NUNCA número cru"],
    "atencao": ["insight, NUNCA número cru"],
    "recomendacoes": ["ação concreta"]
  }
}
```

## Regras de preenchimento

- **`velocidade_funil.estagios` é sempre um array dinâmico** — nunca uma estrutura fixa de 2 ou 3 campos. O número de estágios e as métricas específicas variam mês a mês conforme o objetivo da campanha ativa (ex: campanha de reconhecimento não gera "conversas", e isso é normal).
- **Camadas ativas (`ativa: true/false`) são fixas por plano contratado** (Visibilidade / Posicionamento / Expansão / Referência) — ver tabela "Profundidade por plano" em `framework.md`. Isso só muda se o cliente migrar de plano.
- **Conclusão nunca repete números já mostrados** — só insights/significado (ex: "os anúncios de aluguel seguem como principal motor de conversas", não "96 conversas geradas").
- **`campanhas` é sempre um array**, mesmo com 1 campanha só — o template já sabe renderizar em grid responsivo (`auto-fit`), sem carrossel (decisão consciente: carrossel gerava bugs de alinhamento sem ganho real para 1-2 campanhas).
- Cada campanha carrega sua própria série temporal (`serie_temporal`) e a métrica de resultado principal — a **granularidade do gráfico deve refletir a fonte real do dado** (`"semanal"` se vier do Diário de Performance; `"diaria"` se vier direto do Meta Marketing MCP), nunca fingir precisão que não existe.
- `top_post` fica `null` quando não houver print do post orgânico colado pelo usuário — o template omite o card nesse caso. Quando houver, inclui a imagem como asset (`top_post.imagem_asset`).

## Onde vive o template

`dashboard-model.html` mora em `7labmkt/sevenlab-assets/modelos-html/labsights/dashboard-model.html`, no GitHub. A skill deve buscar esse arquivo, localizar o placeholder literal `{{DASHBOARD_DATA_JSON}}` e substituí-lo pelo objeto montado — nunca embutir dados de um cliente real diretamente no arquivo-modelo do GitHub (isso corromperia o template para todos os próximos clientes).

## Histórico de meses anteriores

O motor agora salva um `.json` companheiro (`AAAA-MM.json`) ao lado de cada `.html` publicado, contendo o `DASHBOARD_DATA` completo daquele mês — ver `webhook_payload_contract.md` para o campo `dashboard_data_json`. Use esses arquivos (via `web_fetch` na URL pública do cliente) para montar a `tendencia` de até 5 meses em Dashboards futuros, em vez de reprocessar HTML antigo. Meses publicados antes desta correção não terão `.json` disponível — nesses casos, monte a tendência só com os dados que você tiver à mão.
