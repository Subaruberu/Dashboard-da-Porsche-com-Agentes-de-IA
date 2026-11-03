# 🚗 Dashboard de Vendas Porsche — com Agentes de IA

Dashboard em **HTML em arquivo único** (`index.html`) que responde perguntas de negócio sobre as vendas de Porsche, com indicadores, gráficos e filtros, a partir da base sanitizada de 100 negócios.

## ❓ Perguntas de negócio (as três escolhidas)

| # | Pergunta | Resposta | Evidência |
|---|---|---|---|
| 01 | **Qual a receita total e o volume de vendas?** | Indicadores no topo: **vendas**, **receita total** e **ticket médio** | Cards de KPI |
| 02 | **Qual modelo de Porsche gerou mais receita?** | Gráfico de barras horizontais (Top 10 por receita) | Ranking por modelo |
| 03 | **Onde se concentra a receita?** | Gráfico de barras (Top 12 estados) | Faturamento por estado |

**Por que essas três:** a primeira fecha o ciclo (o "como estamos" — volume × receita × ticket); a segunda aponta o produto com maior faturamento (decisão de mix/premium); a terceira direciona o comercial (onde concentrar atenção/equipe). São três perguntas que um gestor faria, e cada uma com um gráfico ou indicador dedicado — nada de "amontoado de gráficos".

## 📋 O prompt e os refinamentos

### Prompt (Canvas) — versão 1

> Crie uma dashboard em HTML, em arquivo único, com indicadores e gráficos de barra, a partir de um dataset de vendas com as colunas: modelo, cidade, estado, ano, preço e método de pagamento.
>
> Requisitos:
> - Indicadores no topo: total de vendas e receita total.
> - Três perguntas de negócio respondidas por indicadores/gráficos:
>   1. Qual a receita total e quantas vendas ocorreram? (indicadores)
>   2. Qual modelo de Porsche gerou mais receita? (gráfico de barras)
>   3. Onde a receita se concentra? (gráfico de barras)
> - Filtros: modelo, cidade, ano e método de pagamento, que recomputem indicadores e gráficos.
> - Visual: vermelho Porsche (#ED1C24) sobre fundo claro, fonte Inter, cartões brancos com borda suave.

### Refinamentos (por linguagem natural)

1. Substituí o gráfico de pizza por **barras horizontais** no ranking de modelos (mais legível para top-k).
2. No gráfico geográfico, adicionei **barras verticais com rótulos rotacionados** (Top 12 estados).
3. Ajustei o indicador final para **ticket médio** (receita ÷ vendas).
4. Padronizei a formatação monetária em **USD** via `Intl.NumberFormat`.

### Prompt final (equivalente à versão entregue)

> Gere o HTML descrito acima, com os refinamentos aplicados, e devolva o código completo do dashboard.

## 🧼 Tratamento da base antes de ir para a IA

- Mantidas **apenas as colunas sanitizadas** (`SaleDateSanitized`, `PorscheModelSanitized`, `ModelYearSanitized`, `SalesPriceSanitized`, `PayMethodSanitized`, `CitySanitized`, `StateSanitized`, `DeliveryStatusSanitized`).
- **Preço convertido para número** (ex.: `'$79,500.00'` → `79500.0`, `'USD 112.750'` → `112750.0`).
- **Ano convertido para número** (`'twenty twenty four'` → `2024`).
- Valores **colados como texto numérico**, sem fórmula quebrada.
- Registros com data sanitizada = `INVALID` identificados (24 ocorrências na base).

## 🤖 Ferramentas utilizadas

- **ChatGPT com Canvas** — caminho principal: prompt, geração do dashboard e refinamentos em linguagem natural.
- **Agente com skill** — caminho alternativo: agente que recebeu a base e devolveu a primeira versão (também usado como contraponto).

## 🌐 Dashboard publicado

- **Endereço:** `https://<SEU_USUARIO>.github.io/porsche-vendas/`
- Habilitado via **GitHub Pages** (Settings → Pages → Source: `main` / `/ (root)`).

## ▶️ Como executar localmente

```bash
# clonar o repositório
git clone <url-do-repo>
cd porsche-vendas

# servir localmente
python -m http.server 8000
# abrir http://localhost:8000
```

Ou abrir o `index.html` diretamente no navegador (não há dependências externas).

## 📁 Estrutura do repositório

- `index.html` — dashboard (arquivo único, com dados embarcados).
- `README.md` — este arquivo.
- `8683bed0-cc33-4e06-bca9-04db9c31f9e2.xlsx` — base sanitizada de origem (100 vendas).

## 🔎 Filtros

- **Modelo** — recalcula KPIs e gráficos.
- **Cidade** — recalcula KPIs e gráficos.
- **Ano** — recalcula KPIs e gráficos.
- **Método de pagamento** — recalcula KPIs e gráficos.

## 📸 Evidências

Validação executada com o dashboard rodando em navegador real (servidor local + aba do browser):

- Página carregou sem erros de console (`0` erros);
- KPIs renderizados: **100 vendas**, **receita total US$ 12.827.801**, **ticket médio US$ 128.278,01**;
- Gráficos renderizados: 10 barras (Top modelos) e 12 barras (Top estados);
- Filtro aplicado e testado: ano = `2024` → **29 vendas**, receita **US$ 4.740.800**, ticket **US$ 163.475,86**.

---

Bons estudos e bom projeto 🚀
