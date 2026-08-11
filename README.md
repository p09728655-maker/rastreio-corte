# Rastreio do Corte · Patrimar Móveis

Painel estático para **comparar o que o setor Corte fez, por lote e por data**.

Você exporta do sistema o relatório **“Rel Planilhamento por Funcionário” (.xlsx)**,
arrasta o arquivo na tela e o painel monta na hora:

- **Cortado por lote & data** — a matriz consolidada (Data × Lote) que antes era feita na mão.
  Logo abaixo do nº do lote vem a linha **Programada**, com a data em que ele deveria estar cortado
  (verde se fechou até a meta, vermelho se passou), e a célula do dia da meta fica **marcada** na
  coluna do lote — dá pra bater a data prevista com os dias em que ele realmente saiu.
- **Por funcionário** — quanto cada pessoa cortou, por lote.
- **Peças cortadas por lote** — **qual peça** saiu de cada lote (código + descrição), quanto
  foi cortado × o planejado, **Programada × Realizada** com o desvio em dias (`−4 d` = cortou quatro
  dias antes da meta; `+2 d` = depois), máquina e operador. Para isso, carregue **também**
  o relatório **“Rel Planilhamento por Produto”** (botão na seção Peças, ou arraste na tela) —
  o painel cruza os dois pelo **nº da ORDEM**. Tem busca de peça (código/descrição).
- **Peças cortadas em cada dia** — o **rastreio da data mostrando a peça**: abra um dia e veja
  exatamente **quais peças saíram naquela data** (código, descrição, lote, quanto foi cortado,
  máquina e operador). Tem dois modos: **lista por dia** (padrão, mais fácil de ler) e
  **matriz data × peça** (uma coluna por dia, agrupada por lote). Usa os mesmos dois relatórios
  da seção Peças. A ordem das peças é **por lote e código** (padrão) ou **maior quantidade** —
  o seletor vale para as duas seções de peça e para os arquivos exportados.
  Como um dia de fábrica tem centenas de peças, as duas seções de peça **abrem recolhidas**:
  aparece só o **resumo** (o dia com total e lotes; o lote com nº de peças e o corte de cada dia)
  e você abre o que quiser. Tem **filtro de dia** e botão **Expandir/Recolher tudo**. Recolhida,
  a matriz vira um **Lote × Data** compacto. As linhas de peça só são montadas quando o grupo abre,
  então a tela continua leve mesmo com o mês inteiro carregado — e o PDF sai com o que estiver aberto.
- **Dias de corte (o lote saiu de uma vez ou foi picado?)** — na tabela **Prazo por lote**, uma
  coluna diz em **quantos dias** cada lote foi cortado e **quanto saiu em cada dia**. Quando o lote
  ficou **parado no meio** — teve dia em que a fábrica cortou outras coisas e ele não andou — o
  painel marca `N dias · parou P` em vermelho. Fim de semana e feriado não contam como parada,
  porque o cálculo usa só os dias em que houve produção no arquivo. Dois KPIs no topo: **Lotes em
  +1 dia** e **Lotes que pararam**.
- **Prazo por lote** — você informa a **data-meta** de cada lote (quando ele deveria estar
  cortado) e o painel marca **🟢 no prazo / 🟡 em andamento / 🔴 atrasado**. A data-meta
  fica guardada no próprio navegador (`localStorage`). O status **compara a data programada com a
  data realizada** (último corte) e diz o desvio em dias: 🟢 **Adiantado** (cortou antes da meta),
  🟢 **No dia**, 🔴 **Atrasado**, 🟡 **Em andamento** (ainda não cortou e a meta não venceu).
  Um lote cortado adiantado é marcado como adiantado mesmo que a meta ainda esteja no futuro.
- **MDF e MDP têm datas diferentes** — o lote tem **duas** datas de corte na Programação, porque o
  **MDF corta antes: ele vai pra pintura**. O painel lê as duas (colunas A e B), descobre o material
  de cada peça pela descrição (`... 670X430X15 MDP 2 BRANCO`) e **cobra cada peça contra a data do
  material dela**. O status do lote herda o material em pior situação, e a tabela de prazo mostra os
  dois selos. Comparar peça de MDF com a data do MDP dava atraso/adiantamento falso.
- **Ficha de corte (o papel que vai pra máquina)** — botão **🖶 ficha** em cada lote, ou
  **Fichas de corte (PDF)** pra gerar todas de uma vez. Uma folha A4 por lote, **separada em MDF e
  MDP — o MDF primeiro, que é a ordem em que a máquina precisa ler** — com: as duas datas previstas,
  planejado × cortado × **falta**, status, os dias em que o lote saiu, e a tabela de peças
  (código, descrição, **prevista**, **realizada** com o desvio em dias, planejado, cortado, falta)
  com uma **caixa pro operador marcar** e rodapé de assinatura/turno. Quando o cortado passa do
  planejado, a coluna Falta mostra o excedente (`+240`) em vez de esconder atrás de um traço. As peças **ainda não cortadas** entram na ficha — são exatamente as que o
  operador precisa ver. O rodapé diz de onde vieram os dados e avisa se houve corte cuja ordem não
  está no relatório por Produto.
- **Imprimir MDF e MDP separado** — como os dois **cortam em dias diferentes**, o padrão da ficha é
  **uma folha de MDF e outra de MDP**: cada folha é **fechada em si** (prevista, status, dias de
  corte, % no prazo, planejado × cortado × falta e subtotais só daquele material) e traz, em
  destaque, o aviso de **o que ficou na outra folha** — pra ninguém achar que aquela folha é o lote
  inteiro. O seletor **“Ficha”** (seção *Peças*) escolhe: `MDF e MDP em folhas separadas` (padrão),
  `Só MDF`, `Só MDP` — é assim que se gera **um PDF de cada** — ou `MDF e MDP na mesma folha` (o
  formato antigo). Com vários lotes em `separadas`, saem **todas as folhas de MDF primeiro e depois
  as de MDP**, que é a ordem em que a fábrica corta. Peça sem `MDF`/`MDP` na descrição não some:
  vai numa folha própria (*Sem material na descrição*). No detalhe de uma célula da matriz os
  botões **🖶 MDF / 🖶 MDP / 🖶 separado / 🖶 tudo junto** imprimem direto o lote daquela célula.
- **Desvio por peça (uma lista só)** — a ficha é *por lote*; este é o contrário: **uma folha
  contínua com as peças de todos os lotes**, cada uma com lote, ordem, material, **prevista ×
  realizada**, o **desvio em dias úteis**, planejado × cortado × falta — ordenada do **pior atraso
  pro maior adiantamento**, com resumo no topo (quantas atrasadas / no dia / adiantadas, desvio
  médio ponderado pelo cortado e a pior peça). O seletor **“Desvio”** escolhe entre **só as peças
  fora da data prevista** (padrão) e **todas**. Botão na seção *Peças* (usa os lotes filtrados na
  tela) e na **matriz** (usa todos os lotes do arquivo).
- **Filtro de material na matriz** — o seletor **“Material”** (ao lado de *Operador*) deixa a matriz
  mostrando **só o corte de MDF**, **só o de MDP** ou só o que não diz o material na descrição.
  Tudo na tela passa a ser daquele material: quantidades, totais, “Lotes no dia”, “Antecipado
  (pulmão)”, a lista de peças que abre ao clicar numa quantidade e o que você imprimir dali
  (ficha da coluna e relatório de desvio). Precisa do **relatório por Produto** carregado — é a
  descrição da peça que diz o material —, e sem ele o seletor fica **desligado** com o aviso, em
  vez de mostrar uma matriz vazia.
- **Imprimir a coluna do lote** — na matriz, clicar no **cabeçalho do lote** (o `LT` no topo da
  coluna) imprime a **ficha daquele lote** direto, sem passar pela aba *Peças*.
- **Todo impresso no mesmo padrão** — relatório, invasão de dias, ficha de corte e desvio por peça
  saem com o **mesmo cabeçalho dos painéis de PPCP**: faixa escura, tarja *PPCP — Planejamento e
  Controle da Produção* em âmbar, título em branco e o logo num chip branco. Nas folhas que passam
  de uma página, a **faixa de identificação** (`LT 130/26 · MDP · prevista 14/07/26 · 18 peça(s)`)
  e a régua de colunas **se repetem no alto de cada página** — a folha 2 não sai mais como uma
  tabela solta, sem lote nem material.
- **Travar uma linha** — nas tabelas largas (matriz **data × lote** e **por funcionário**), clicar
  numa linha **trava** ela: fica marcada de ponta a ponta e continua marcada enquanto você rola pro
  lado, então dá pra ir até a coluna do lote lá na direita sem perder de vista de que dia (ou de
  quem) é aquele número. Clicar de novo destrava; clicar em outra linha muda a marca de lugar. Nos
  números sublinhados o clique continua abrindo as peças do dia.
- **Clique numa quantidade da matriz** — abre quais peças saíram naquele dia naquele lote, com
  máquina e operador, e diz **a quantos dias da data programada** aquilo foi cortado (serve pra
  explicar corte solto fora da janela do lote, tipo aproveitamento de chapa).
- **Exportar** — baixa a consolidação em `.csv` (com o detalhe por peça e o que saiu em cada dia)
  ou `.xlsx` (uma aba por operador + abas **Peças por lote**, **Checklist por lote**, **Peças por dia**
  e **Data x Peça**). A aba **Checklist por lote** é a versão planilha da ficha: um bloco por lote com
  planejado / cortado / **falta** e coluna `OK` pra marcar — pra quem prefere imprimir do Excel.

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

### Peça × lote (dois relatórios)
O relatório “por Funcionário” **não traz a descrição da peça** — ele só tem o lote. A peça
está no **“Rel Planilhamento por Produto”**, onde cada bloco é um **PRODUTO = a peça**
(código + descrição) com suas operações (ORDEM, QTDE ORDEM, QTDE PROD., …). Como os dois
relatórios compartilham o **nº da ORDEM**, o painel liga `ORDEM → peça` (do Produto) com
`ORDEM → lote` (do Funcionário) e monta o **“Peças cortadas por lote”**. Exporte os dois do
**mesmo período**. Só conta como cortada a operação com `QTDE PROD. > 0`. Nada é enviado à
internet — o cruzamento acontece no navegador.

**Dá pra carregar vários relatórios por Produto.** Eles se **somam** (`ORDEM → peça`), então se o
Corte pega a semana inteira e o Produto do sistema sai de um dia só, é só carregar um de cada dia.
Toda ordem cortada que não estiver em nenhum Produto carregado aparece como **“sem peça”**, e o
painel avisa **exatamente em que dias** isso aconteceu — é o sinal de que faltou carregar aquele
período. O botão **“Limpar peças carregadas”** zera o conjunto.

### Rastreio da data com a peça
Cada operação do relatório por Funcionário já traz **ORDEM + DATA + lote**. Com o `ORDEM → peça`
do relatório por Produto, o painel sabe **peça + lote + data + quantidade** de cada corte — é isso
que a seção **“Peças cortadas em cada dia”** mostra. Os totais por dia batem, peça por peça, com a
linha “Total dia” da matriz por lote. O relatório do sistema às vezes gruda o rótulo
`Total de horas:` na coluna da máquina; nesses casos o painel usa o **código** da máquina (`SEC01`).

### Programação: duas datas por lote (MDF e MDP)
Na planilha da Programação, a coluna **C** é o nº do lote e as **duas primeiras colunas são as datas
de corte**: **A = MDF** e **B = MDP**. São duas porque o **MDF corta antes — ele vai pra pintura**.
Antes de usar essa ordem, o parser procura no cabeçalho da planilha uma célula com `MDF`/`MDP` e
mapeia pela posição real; só cai no padrão `A=MDF, B=MDP` se não achar. A mensagem ao carregar diz
qual dos dois caminhos foi usado (`colunas pelo cabeçalho` ou `pelo padrão A=MDF, B=MDP`) — confira
ali antes de confiar nos prazos.

O material de cada peça sai da **descrição** (`... 670X430X15 MDP 2 BRANCO`), e é assim que cada peça
é cobrada contra a data certa. Peça sem `MDF`/`MDP` na descrição (perfil, papelão) fica sem material
e cai na data mais tarde do lote. Datas digitadas na mão também são **por material** — a tabela de
prazo tem um campo pra cada. Dados salvos no formato antigo (uma data só) migram sozinhos para MDP,
que era a coluna que o painel lia antes.

Como as duas datas caem em dias diferentes, **a ficha de corte sai separada por material** (uma
folha de MDF e outra de MDP, cada uma com os números só dela) — veja o seletor **“Ficha”** na seção
*Peças*. As peças sem material na descrição saem numa folha própria em vez de sumirem da impressão.
