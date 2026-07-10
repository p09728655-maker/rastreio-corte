# Rastreio do Corte · Patrimar Móveis

Painel estático para **comparar o que o setor Corte fez, por lote e por data**.

Você exporta do sistema o relatório **“Rel Planilhamento por Funcionário” (.xlsx)**,
arrasta o arquivo na tela e o painel monta na hora:

- **Cortado por lote & data** — a matriz consolidada (Data × Lote) que antes era feita na mão.
- **Por funcionário** — quanto cada pessoa cortou, por lote.
- **Prazo por lote** — você informa a **data-meta** de cada lote (quando ele deveria estar
  cortado) e o painel marca **🟢 no prazo / 🟡 em andamento / 🔴 atrasado**. A data-meta
  fica guardada no próprio navegador (`localStorage`).
- **Exportar** — baixa a consolidação em `.csv`.

Tudo roda **no navegador** — nenhum dado é enviado para a internet.

## Arquivos
- `index.html` — o painel (HTML/CSS/JS autossuficiente; usa SheetJS via CDN para ler o `.xlsx`).
- `vercel.json` — `/` → `index.html`.
- `manifest.json` + ícones — PWA / marca Patrimar.

## Publicar (Vercel)
1. Vercel → **Add New → Project** → selecione este repositório.
2. Framework preset: **Other** (site estático). Sem build.
3. Deploy. A cada `push` na `main`, publica sozinho.

## Como o arquivo é lido
O relatório traz, por funcionário, uma linha por operação de corte (ORDEM, QTDE PROD.,
DATA, MÁQUINA, horário) e, logo abaixo de cada operação, uma linha `Lote` com o código
(`LT 129/26`, …). O painel soma a `QTDE PROD.` por **lote** e por **data**. Para comparar
vários dias, basta exportar o relatório com um **período maior**.
