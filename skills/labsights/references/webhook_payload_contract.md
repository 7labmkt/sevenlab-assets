# Contrato do Payload — Webhook de Publicação (n8n)

Workflow: `LabSights — Publicação de Dashboard` (ID `Gz1FLmShuAVmXYZ4`)
Endpoint: `POST https://n8n.7labmarketing.com.br/webhook/labsights-publicar`

## Payload atual

```json
{
  "cliente_nome": "",
  "cliente_slug": "",
  "cpanel_token": "",
  "drive_folder_id": "",
  "html_content": "<html>...",
  "mes_referencia": "AAAA-MM",
  "dashboard_data_json": { "...": "o objeto DASHBOARD_DATA completo, como objeto JSON (não precisa ser string)" }
}
```

Campos:

- **`cliente_nome`**: nome do cliente por extenso, usado para compor o nome do arquivo PDF salvo no Drive (`LabSights [Nome do Cliente] — [Mês Extenso] [Ano].pdf`)
- **`cliente_slug`**: identificador de URL (ex: `belle-imoveis`), usado para o caminho de publicação (`dados.7labmarketing.com.br/labsights/{slug}/`) e para nomear a pasta no cPanel
- **`cpanel_token`**: token de autenticação do proxy PHP. **Em processo de remoção** — ver seção "Pendência de segurança" abaixo. Enquanto a credencial do n8n não estiver conectada, este campo ainda é obrigatório.
- **`drive_folder_id`**: ID da pasta `Relatórios` do cliente dentro de `HD Sevenlab`, no Google Drive. **Sempre resolvido pelo Claude antes de montar o payload** — nunca deixe o n8n adivinhar ou buscar isso sozinho
- **`html_content`**: o Dashboard HTML completo, já com os dados do `DASHBOARD_DATA` injetados, pronto para publicação
- **`mes_referencia`**: formato `AAAA-MM` (ex: `2026-07`)
- **`dashboard_data_json`** *(novo, opcional)*: o objeto `DASHBOARD_DATA` inteiro (o mesmo montado no Passo 6 da skill), enviado como objeto JSON — o n8n converte automaticamente para base64 e o proxy PHP salva como `AAAA-MM.json`, ao lado do `.html`. **Sempre inclua este campo** — é o que permite montar a `tendencia` de meses futuros sem reprocessar HTML antigo. Se omitido, o motor publica normalmente, só sem salvar o histórico estruturado daquele mês.

`mes_nome_extenso` é opcional no payload — se omitido, o próprio node "Preparar HTML (base64)" calcula em português a partir de `mes_referencia`.

## O que o motor faz com esse payload

```
Webhook recebe o payload
  → Preparar HTML (base64): gera html_base64, calcula mes_nome_extenso se faltar,
                             converte dashboard_data_json para dashboard_data_base64
  → [ramo A] Gerar PDF (PDFShift) → Salvar PDF no Drive do Cliente
  → [ramo B] Publicar no cPanel (arquiva o mês anterior automaticamente,
                                   publica o novo .html, salva o .json companheiro se enviado)
  → Juntar Resultados (Merge)
  → Confirmar Publicação: retorna { ok, cliente_slug, mes_publicado, url_publica, pdf_salvo_drive }
```

A URL pública retornada segue o padrão: `https://dados.7labmarketing.com.br/labsights/{cliente_slug}/`

O proxy retorna também `dashboard_data_salvo: true/false`, confirmando se o `.json` companheiro foi gravado com sucesso naquele mês.

## Como ler o histórico de meses anteriores (a partir de agora)

Para montar a `tendencia` de até 5 meses num Dashboard novo, busque os arquivos `AAAA-MM.json` já publicados na pasta do cliente (`dados.7labmarketing.com.br/labsights/{slug}/AAAA-MM.json`) via `web_fetch` — cada um contém o `DASHBOARD_DATA` completo daquele mês, já estruturado. Meses anteriores à implementação desta pendência (antes desta correção) não terão `.json` disponível — nesses casos, monte a tendência só com os dados que você tiver à mão.

## Pendência de segurança (em resolução)

O `cpanel_token` está sendo migrado do payload para uma credencial Header Auth dentro do próprio n8n ("cPanel Proxy Token"). Assim que essa credencial estiver conectada ao node "Publicar no cPanel", o campo `cpanel_token` deixa de ser necessário no payload — este documento será atualizado para refletir a remoção.

**Enquanto essa migração não for concluída:** o token deve ser tratado como qualquer outro segredo do projeto — nunca exposto em texto solto fora do necessário.

