<div align="center">

# 🎬 Análise IMDb · Grupo 7

**Análise exploratória do catálogo IMDb respondendo 8 perguntas de negócio sobre o mercado cinematográfico mundial.**

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?style=flat-square&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-3.x-11557C?style=flat-square)](https://matplotlib.org/)
[![NumPy](https://img.shields.io/badge/NumPy-1.x-013243?style=flat-square&logo=numpy&logoColor=white)](https://numpy.org/)
[![Colab](https://img.shields.io/badge/Open%20in-Colab-F9AB00?style=flat-square&logo=googlecolab&logoColor=white)](https://colab.research.google.com/)
[![Dataset](https://img.shields.io/badge/Dataset-Base%20dos%20Dados-2D9CDB?style=flat-square)](https://basedosdados.org/)
[![CEUB](https://img.shields.io/badge/CEUB-Introdução%20à%20Ciência%20de%20Dados-0A66C2?style=flat-square)](#)

</div>

---

## TL;DR

Pegamos o catálogo do IMDb (filmes mais relevantes por ano via Base dos Dados), filtramos por relevância estatística (`vote >= 50.000`) e respondemos 8 perguntas de negócio com 8 visualizações em tema escuro coeso. Stack mínima (pandas + matplotlib + numpy), zero dependência exótica, totalmente reproduzível no Google Colab.

A apresentação final é um HTML standalone (slides com navegação por teclado, sem build, sem servidor).

---

## 📊 Principais descobertas

| # | Pergunta | Achado |
|---|---|---|
| 1 | Quais os 20 filmes melhor avaliados? | Top 20 dominado por clássicos com nota ≥ 8.6 e centenas de milhares de votos. |
| 2 | Quais gêneros dominam? | Drama, Action e Comedy concentram o grosso do catálogo. Top 8 cobre ~80% da distribuição. |
| 3 | Quais filmes lideram a bilheteria? | Top 15 mundial puxado por blockbusters recentes. Bilheteria nem sempre acompanha nota IMDb. |
| 4 | Quais países produzem mais e melhor? | Painel 2×2: produção × nota média × Oscars × bubble de bilheteria. Volume e qualidade nem sempre andam juntos. |
| 5 | A qualidade melhorou ao longo das décadas? | Médias por década, mediana, IQR e volume. Tendência sutil, com aumento de variância nas décadas mais recentes. |
| 6 | Vencedor de Oscar avalia melhor e fatura mais? | Violino + barras duplas + gêneros vencedores + evolução por década. Vencedores têm nota e bilheteria médias superiores. |
| 7 | Qual gênero rende mais bilheteria? | **Action Epic** lidera com **US$ 39,6 bi acumulados** e **~US$ 381M por filme**. |
| 8 | Voto popular = nota alta? | Correlação fraca (**r = 0.15**), mas o **top 10% mais votados** tem nota média **7,08** vs **6,33** dos demais. |

---

## 🖼️ Galeria

<table>
<tr>
<td align="center"><b>Q1 · Top 20 melhor avaliados</b><br/><sub><code>chunk_01_top_rated_movies.png</code></sub></td>
<td align="center"><b>Q2 · Gêneros dominantes</b><br/><sub><code>chunk_03_genres.png</code></sub></td>
</tr>
<tr>
<td align="center"><b>Q3 · Top 15 bilheteria</b><br/><sub><code>chunk_04_box_office.png</code></sub></td>
<td align="center"><b>Q4 · Análise por país</b><br/><sub><code>chunk_07_countries.png</code></sub></td>
</tr>
<tr>
<td align="center"><b>Q5 · Qualidade por década</b><br/><sub><code>chunk_08_decade_quality.png</code></sub></td>
<td align="center"><b>Q6 · Oscar × Mercado</b><br/><sub><code>chunk_09_oscar_analysis.png</code></sub></td>
</tr>
<tr>
<td align="center"><b>Q7 · Bilheteria por gênero</b><br/><sub><code>bar_receita_genero.png</code></sub></td>
<td align="center"><b>Q8 · Votos × Nota</b><br/><sub><code>scatter_votes_vs_rating.png</code></sub></td>
</tr>
</table>

> Os PNGs são gerados automaticamente na execução do notebook (DPI 150, fundo preservado).

---

## 🧱 Stack

```
Python 3.10+
├── pandas         → carga, limpeza, agregação
├── numpy          → operações vetoriais, log, percentis
└── matplotlib     → visualizações com tema escuro custom
    ├── patches    → FancyBboxPatch, legendas customizadas
    ├── ticker     → formatação de eixos (US$, escala log)
    └── lines      → handles de legenda manuais
```

Nada de seaborn, plotly ou bibliotecas de tema. Tudo construído com `rcParams` global + customizações finas por figura. Decisão deliberada: portabilidade total e zero "olho de AI" nas visualizações.

---

## 📂 Estrutura do repositório

```
.
├── codigo_python_imdb.ipynb    # Notebook principal (22 células, 8 análises)
├── apresentacao_imdb.html      # Apresentação standalone (slides, navegação por teclado)
├── chunk_01_top_rated_movies.png
├── chunk_03_genres.png
├── chunk_04_box_office.png
├── chunk_07_countries.png
├── chunk_08_decade_quality.png
├── chunk_09_oscar_analysis.png
├── bar_receita_genero.png
├── scatter_votes_vs_rating.png
└── README.md
```

---

## 🗃️ Dataset

**Origem:** [Base dos Dados](https://basedosdados.org/) · arquivo `world_imdb_movies_top_movies_per_year.csv.gz`

**Granularidade:** um filme por linha, recorte de filmes mais relevantes por ano de lançamento.

### Dicionário de dados utilizado

| Coluna | Tipo | Uso na análise |
|---|---|---|
| `title` | string | Identificação do filme |
| `year` | int | Recorte temporal e agregação por década (1920–2023) |
| `genre` | string (CSV) | Múltiplos gêneros separados por vírgula, explode em linhas |
| `country_origin` | string (CSV) | Múltiplos países, explode em linhas |
| `rating_imdb` | float | Métrica principal de qualidade |
| `vote` | int | Filtro de relevância (≥ 50k) e popularidade |
| `gross_world_wide` | float (USD) | Bilheteria mundial |
| `budget` | float (USD) | Base para cálculo de ROI |
| `oscar` | int | Quantidade de Oscars vencidos |
| `nomination` | int | Quantidade de indicações ao Oscar |
| `win` | int | Total de premiações (qualquer premiação) |

### Filtros base

```python
df = df.dropna(subset=["rating_imdb", "vote", "title"])
df = df[df["vote"] >= 50_000]   # corte de relevância estatística
```

---

## 🔄 Pipeline de análise

```
┌─────────────────────┐
│ CSV.GZ (Base dos    │
│ Dados, IMDb)        │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Carga + Limpeza     │   dropna em colunas-chave
│ (pandas)            │   filtro vote >= 50k
└──────────┬──────────┘
           │
           ├──► explode genre / country (multi-valor)
           ├──► feature engineering (decade, ROI, log-vote)
           │
           ▼
┌─────────────────────┐
│ Agregação por       │   groupby + agg múltiplos
│ pergunta (8x)       │   percentis, médias, medianas
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Visualização        │   tema dark unificado
│ matplotlib          │   1 figura por pergunta
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Export PNG (150 DPI)│   + slide HTML embed
└─────────────────────┘
```

---

## ▶️ Como rodar

### Opção 1: Google Colab (recomendado)

1. Abra `codigo_python_imdb.ipynb` no Colab
2. Faça upload de `world_imdb_movies_top_movies_per_year.csv.gz` para `/content/`
3. <kbd>Runtime</kbd> → <kbd>Run all</kbd>

### Opção 2: Local

```bash
# Clone
git clone <repo-url>
cd <repo>

# Ambiente isolado
python -m venv .venv
source .venv/bin/activate    # Linux/Mac
# .venv\Scripts\activate     # Windows

# Dependências
pip install pandas numpy matplotlib jupyter

# Rodar
jupyter notebook codigo_python_imdb.ipynb
```

Ajuste o path do CSV na primeira célula de carregamento se necessário.

### Apresentação

```bash
# Linux
xdg-open apresentacao_imdb.html
# Mac
open apresentacao_imdb.html
# Windows
start apresentacao_imdb.html
```

Arquivo HTML autocontido. Navegação: setas do teclado ou clique nos números das perguntas no header.

---

## 🎨 Design system

Paleta única aplicada em todas as 8 visualizações via `plt.rcParams.update(...)`, inspirada no GitHub Dark:

| Token | Hex | Uso |
|---|---|---|
| `BG` | `#0d1117` | Fundo da figura |
| `SURFACE` | `#161b22` | Fundo do plot area |
| `BORDER` | `#30363d` | Linhas de grade e spines |
| `TEXT_PRI` | `#e6edf3` | Texto primário |
| `TEXT_SEC` | `#8b949e` | Labels, ticks, eixos |
| `BLUE` | `#378ADD` | Cor primária (dados) |
| `GOLD` | `#ffa657` | Destaque (#1, vencedores Oscar) |
| `CORAL` | `#D85A30` | Tendência, médias, alertas |

Quatro decisões de design propositais:

1. **Tema único em todos os gráficos:** dá continuidade narrativa entre as 8 análises.
2. **Cards de métrica acima do gráfico principal** (Q7 e Q8): destaca KPIs antes do detalhe visual.
3. **Tipografia secundária mais clara que a primária:** hierarquia sem precisar de bold.
4. **Cor de destaque (`GOLD`) reservada para o item de maior interesse:** força o olho a ir direto ao ranking #1.

---

## 🧠 Decisões metodológicas

- **Filtro de 50.000 votos** para Q1, Q2, Q3, Q5, Q6, Q7, Q8: evita que filmes com 200 votos dominem rankings por sorte estatística.
- **Mínimo 30 filmes por país** em Q4: corta caudas longas que distorceriam a média.
- **Mínimo 50 filmes por país** no ranking de qualidade Q4: corte ainda mais conservador para nota média.
- **Janela 1920–2023** em Q5: ignora décadas com volume insuficiente.
- **Escala log₁₀ no eixo de votos** em Q8: votos têm distribuição cauda longa, escala linear esmagaria a maioria contra a origem.
- **Regressão linear simples** em Q8: indica direção e força sem overfitting; correlação Pearson reportada explicitamente.
- **Explode multi-valor** (gênero e país): filmes com múltiplos gêneros/países contam em cada categoria, não em uma "combo string".

---

## ⚠️ Limitações

- O dataset cobre **filmes mais relevantes por ano**, não o catálogo completo do IMDb. Há viés de seleção embutido na fonte.
- `gross_world_wide` é nominal em USD, sem ajuste por inflação. Filmes recentes parecem desproporcionalmente lucrativos comparados aos antigos.
- Filmes sem `budget` reportado não entram no cálculo de ROI (Q3).
- Q8 usa amostra de `n=2000` para evitar overplotting. Métricas (correlação, médias) são calculadas no dataset completo.
- A relação observada em Q8 é correlação, não causalidade. Filmes com muitos votos podem ter notas mais altas porque são bons, ou porque já são conhecidos como bons (viés de auto-seleção do votante).

---

## 🚀 Roadmap

- [ ] Ajustar bilheterias pela inflação (deflator do PIB ou CPI dos EUA)
- [ ] Análise de sobrevivência: tempo até "saída" do catálogo / esquecimento do público
- [ ] Modelo simples de regressão (nota ~ duração + orçamento + ano + gênero) para Q5/Q6
- [ ] Versão interativa em Plotly (mesmo tema) para a apresentação
- [ ] Pipeline reprodutível em dbt + DuckDB para versionar transformações
- [ ] Testes em `pytest` para validar contratos do dataset (schema, ranges, nulos)

---

## 👥 Autores

**Grupo 7 · Introdução à Ciência de Dados**
Centro Universitário de Brasília (CEUB) · 2026

---

## 📄 Licença

Análise e código sob MIT. Dataset original da Base dos Dados sob seus próprios termos de uso.

<div align="center">
<sub>Feito com pandas, café e tema escuro.</sub>
</div>
