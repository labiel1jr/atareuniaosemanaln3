# Ata da Reunião de Alinhamento Semanal — Desenvolvimento N3

Página web interativa para montar, registrar e exportar a **ata da reunião semanal** da equipe de Desenvolvimento N3. Ela reúne numa só tela os tickets prioritários, a meta semanal x realizado, o desempenho acumulado, os avisos, os BDFix e os bugs recentes, e gera o PDF da ata com um clique.

O site é **um único arquivo `index.html`**, sem backend, sem build e sem dependências para instalar. Os dados de cada seção ficam em arquivos `.txt` versionados neste repositório.

---

## Sumário

- [Funcionalidades](#funcionalidades)
- [Seções da ata](#seções-da-ata)
- [Como usar](#como-usar)
- [Arquivos de dados (.txt)](#arquivos-de-dados-txt)
- [Rotina semanal de atualização](#rotina-semanal-de-atualização)
- [Personalização](#personalização)
- [Estrutura do repositório](#estrutura-do-repositório)
- [Limitações conhecidas](#limitações-conhecidas)

---

## Funcionalidades

- **Tela de abertura**: você marca quem está presente ou ausente e escolhe o responsável pela reunião e quem escreve a ata.
- **Data automática**: a data da reunião é a do dia em que a ata é gerada, e o campo "Novo período" já vem com a semana atual (segunda a sexta).
- **12 seções** editáveis direto na página: é possível adicionar e remover linhas em cada uma.
- **Carga automática dos dados**: ao abrir a página, cada seção lê o `.txt` correspondente que está na mesma pasta.
- **Salvar / Carregar TXT por seção**: exporta a tabela para `.txt` ou importa um `.txt` escolhido na máquina.
- **Indicadores e gráfico**: KPIs e um gráfico de barras em SVG (Meta x Realizado), com zoom pela roda do mouse e reset com duplo clique.
- **Tickets prioritários**: tabela ordenável por coluna, busca de rede com autocompletar e troca do responsável direto na célula.
- **Exportação em PDF**: pelo diálogo de impressão do navegador, com um tema claro próprio para impressão (os botões e os formulários ficam ocultos).
- **Responsivo**: funciona em desktop e em celular.

## Seções da ata

| # | Seção | O que registra | Arquivo de dados |
|---|-------|----------------|------------------|
| 01 | Identificação da Reunião | Data, responsável, autor da ata, participantes e ausentes | — (preenchida na tela de abertura) |
| 02 | Tickets Prioritários da Semana | Ticket, rede, responsável, motivo da prioridade e checkpoint | `tickets_prioritarios_semana.txt` |
| 03 | Meta Semanal × Realizado | Tickets entregues por semana comparados com a meta (15) | `meta_semanal_x_realizado.txt` |
| 04 | Desempenho Acumulado | KPIs e gráfico calculados a partir da seção 03 | — (calculada) |
| 05 | Análise, Comentários e Feedbacks | Pontos de atenção levantados na reunião | `analise_comentarios_feedbacks.txt` |
| 06 | Acompanhamento | Tickets que estão sendo acompanhados, com comentário | `acompanhamento.txt` |
| 07 | Sugestões | Sugestões de melhoria de processo ou de sistema | `sugestoes.txt` |
| 08 | Tickets — Análise Conjunta N3 | Tickets antigos analisados em conjunto pela equipe | `tickets_analise_conjunta_n3.txt` |
| 09 | Avisos | Férias, folgas, feriados e compromissos da equipe | `avisos.txt` |
| 10 | Impactos / Acontecimentos da Semana | O que afetou a produtividade em cada semana | `impactos_acontecimentos_semana.txt` |
| 11 | BDFix — Recentes | Scripts de correção de base aplicados (NUVEM/LOCAL) | `bdfix_recentes.txt` |
| 12 | Bugs Recentes | Bugs identificados, com a descrição e a solução | `bugs_recentes.txt` |

### Indicadores da seção 04

| Indicador | Cálculo |
|-----------|---------|
| Total de períodos | Quantidade de linhas da seção 03 |
| Metas atingidas/superadas | % de semanas em que Realizado ≥ Esperado |
| Abaixo da meta | % de semanas em que Realizado < Esperado |
| Desempenho acumulado geral | Σ Realizado ÷ Σ Esperado × 100 |

Na seção 03, o percentual de cada semana aparece em verde (≥ 100%), amarelo (90–99%) ou vermelho (< 90%).

## Como usar

### 1. Abrir a página

A página precisa ser servida por **HTTP** para que a carga automática dos `.txt` funcione. Aberto direto do disco (`file://`), o navegador bloqueia o `fetch` e as seções ficam vazias ou com os dados de exemplo. Nesse caso ainda dá para usar **📂 Carregar TXT** seção por seção.

Opções:

- **GitHub Pages** (recomendado para a equipe): em *Settings → Pages*, publique a branch `main` na raiz. O site fica em `https://<usuário>.github.io/atareuniaosemanaln3/`.
- **Servidor local**, na pasta do projeto, com um destes comandos:

```bash
python -m http.server 8000
```

```bash
npx serve .
```

  Depois abra `http://localhost:8000`.

### 2. Tela de abertura

1. Marque cada membro como **Presente** ou **Ausente**.
2. Escolha o **Responsável pela reunião** e o **Ata por** (a lista só mostra quem está presente).
3. Clique em **Gerar ata da reunião**.

O botão **↺ Alterar participantes** volta para essa tela a qualquer momento.

### 3. Durante a reunião

- Use o formulário no fim de cada seção para **adicionar** linhas e o botão **Remover** para excluir.
- Nos tickets prioritários, clique no cabeçalho para ordenar e na célula do responsável para trocá-lo.
- Ao terminar, clique em **💾 Salvar TXT** nas seções alteradas para baixar os arquivos atualizados.

### 4. Gerar o PDF

Clique em **⬇ Gerar em PDF** e escolha *Salvar como PDF* no diálogo de impressão.

> **Importante:** tudo o que é editado na página fica só na memória do navegador. Se você recarregar a página sem salvar os TXT, as alterações se perdem.

## Arquivos de dados (.txt)

- Uma linha por registro e campos separados por **`|`** (pipe).
- Codificação **UTF-8**.
- Linhas em branco são ignoradas.
- Não use `|` dentro do texto de um campo.

| Arquivo | Formato da linha | Exemplo |
|---------|------------------|---------|
| `tickets_prioritarios_semana.txt` | `ticket\|rede\|responsável\|motivo\|checkpoint` | `440918\|REDE TUPY\|Rafael Jr\|Urgência Fernando\|Criar arquivo temporário…` |
| `meta_semanal_x_realizado.txt` | `período\|esperado\|realizado` | `21/09/2026 – 25/09/2026\|15\|22` |
| `analise_comentarios_feedbacks.txt` | `responsável\|tipo\|comentário` | `Sem Responsável\|Análise\|Atenção aos tickets de postos estratégicos…` |
| `acompanhamento.txt` | `responsável\|ticket\|comentário` | `Wesley Phillipe\|461463\|Aguardando retorno do cliente` |
| `sugestoes.txt` | `responsável\|ticket\|sugestão` | `Flaubert\|\|Que aconteça esse tipo de reunião também no N2.` |
| `tickets_analise_conjunta_n3.txt` | `responsável\|tickets` | `Wesley Phillipe\|461463 / 419774` |
| `avisos.txt` | texto livre (sugestão: `dd/mm/aaaa - aviso`) | `07/09/2026 - Feriado nacional - Independência do Brasil.` |
| `impactos_acontecimentos_semana.txt` | `semana\|impacto` | `14/09/2026 - 18/09/2026\|Rafael teve troca de máquina…` |
| `bdfix_recentes.txt` | `ticket\|título\|local` | `539704\|Criação de BDFIX para excluir fatura unificada…\|NUVEM` |
| `bugs_recentes.txt` | `ticket\|título\|descrição\|solução` | `537282\|List index out of bounds…\|…\|Realizada a correção…` |

Valores aceitos:

- **Tipo** (análise): `Comentário`, `Análise`, `Feedback`.
- **Local** (BDFix): `NUVEM`, `LOCAL`.
- **Responsável**: um dos membros cadastrados ou `Sem Responsável`.
- **Ticket** em sugestões: opcional (deixe o campo vazio, como em `Nome||texto`).

## Rotina semanal de atualização

1. Abra a página e faça a reunião, editando as seções.
2. Clique em **💾 Salvar TXT** nas seções que mudaram.
3. Substitua os arquivos na pasta do repositório pelos que foram baixados (o navegador pode renomeá-los como `arquivo (1).txt`; corrija o nome).
4. Gere o PDF da ata.
5. Faça o commit e o push:

```bash
git add *.txt
```

```bash
git commit -m "Ata de dd/mm/aaaa"
```

```bash
git push
```

O histórico do Git passa a funcionar como o arquivo das atas anteriores.

## Personalização

As listas ficam no `<script>` do `index.html`:

| O que mudar | Onde (procure por) |
|-------------|--------------------|
| Membros da equipe | `var MEMBERS = [` |
| Meta semanal (padrão 15) | `var ESPERADO = 15;` e o campo desabilitado com `value="15"` na seção 03 |
| Redes (autocompletar dos tickets) | `var REDES = [` |
| Motivos de prioridade | `var MOTIVOS = [` |
| Faixas de cor do percentual | `function pctClass` |
| Cores e fontes | variáveis CSS em `:root` |

## Estrutura do repositório

```
atareuniaosemanaln3/
├── index.html                          # Site completo (HTML + CSS + JS)
├── og-image-ata-n3.png                 # Imagem de pré-visualização para links
├── tickets_prioritarios_semana.txt     # Seção 02
├── meta_semanal_x_realizado.txt        # Seção 03 (alimenta a 04)
├── analise_comentarios_feedbacks.txt   # Seção 05
├── sugestoes.txt                       # Seção 07
├── tickets_analise_conjunta_n3.txt     # Seção 08
├── avisos.txt                          # Seção 09
├── impactos_acontecimentos_semana.txt  # Seção 10
├── bdfix_recentes.txt                  # Seção 11
└── bugs_recentes.txt                   # Seção 12
```

O `acompanhamento.txt` (seção 06) ainda não está no repositório. Enquanto ele não existir, a seção abre vazia.

## Limitações conhecidas

- **Sem persistência automática**: as edições ficam só na memória até você salvar os TXT e fazer o commit.
- **Sem controle de acesso**: publicado no GitHub Pages, o site e os `.txt` (nomes de clientes, tickets e avisos da equipe) ficam visíveis para quem tiver o link. Se o repositório for público, avalie torná-lo privado.
- **`file://` não carrega os TXT automaticamente**: é preciso usar um servidor HTTP.
- **Separador `|`**: um pipe dentro de um texto quebra a linha em campos a mais.
- O `og-image-ata-n3.png` não é referenciado pelo HTML hoje (as meta tags usam SVG embutido, que a maioria das redes sociais não exibe).
