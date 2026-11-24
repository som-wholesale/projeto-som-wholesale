# Projeto Final: SOM (Self-Organizing Map) na base Wholesale Customers

Projeto final de **Aprendizado Não Supervisionado**: segmentação de 440 clientes de um atacadista com SOM, comparada com K-Means e PCA.

> 👉 **O que você precisa fazer está no [TAREFAS.md](TAREFAS.md)**: a parte de cada um, o que recebe, o que entrega e o passo a passo.
>
> 📌 Contexto e decisões estão no [PROJETO.md](PROJETO.md), que tem as decisões, a divisão de tarefas, o passo a passo de cada parte, o status e o histórico.

## Entrega final

| O que | Arquivo |
|---|---|
| Relatório final (13 páginas) | [entrega/relatorio_final.pdf](entrega/relatorio_final.pdf) |
| Slides da apresentação (9 slides) | [entrega/slides_apresentacao.pdf](entrega/slides_apresentacao.pdf) |
| Slides com as notas de fala de cada integrante | [entrega/slides_apresentacao.pptx](entrega/slides_apresentacao.pptx) |
| Código e resultados | notebooks `01` a `06` (executados, com as saídas) |

**Resultado em uma frase:** um SOM 10×10 (σ = 2,0, lr = 0,3; QE = 0,898, TE = 0,050) separa três perfis de cliente: Horeca de perecíveis, Varejo de mercearia e limpeza e Grandes compradores completos (33% dos clientes e 52% do faturamento). Também marca 22 clientes atípicos. O K-Means direto concorda com o SOM em 88% dos clientes (ARI 0,68), e o PCA mostra os mesmos dois eixos de compra (71% da variância).

Onde está cada item pedido no enunciado (Projeto 2: SOM):

| Pedido do enunciado | Onde está |
|---|---|
| Base multivariada e padronização | `dados/X_scaled.csv` · notebook 01 |
| Mapa treinado | `dados/som.pkl` (e `dados/som_pesos.npy`) · notebook 02 |
| U-Matrix e leitura de regiões e distâncias | `figuras/03_umatrix.png`, `figuras/03_component_planes.png`, `figuras/03_hitmap.png` · notebook 03 |
| Agrupamentos (clusters) | `dados/clusters.csv`, `figuras/04_mapa_clusters.png` · notebook 04 |
| Outliers | `figuras/04_outliers.png` · notebook 04 |
| Comparação com K-Means e PCA | `figuras/05_pca_som_vs_kmeans.png`, `figuras/05_estabilidade.png` · notebook 05 |
| Perfis nomeados e aplicações | `figuras/06_perfis_clusters.png` · notebook 06 |
| Interpretação dos grupos | notebooks 03 a 06 e seções 4 a 8 do relatório |

## Estrutura

```
├── TAREFAS.md                              ← o que cada um precisa fazer
├── PROJETO.md                              ← registro central (decisões, histórico)
├── 01_Amabilly_EDA_preprocessamento.ipynb  ← Parte 1 ✅
├── 02_Roger_treino_som.ipynb               ← Parte 2 ✅
├── 03_Erica_visualizacoes.ipynb            ← Parte 3 ✅
├── 04_Ruan_clusters_outliers.ipynb         ← Parte 4 ✅
├── 05_Herbe_comparacao.ipynb               ← Parte 5 ✅
├── 06_Rafael_perfis.ipynb                  ← Parte 6 ✅
├── dados/
│   ├── Wholesale customers data.csv        ← base original (UCI)
│   ├── X_scaled.csv                        ← 440×6 padronizado, entrada do SOM
│   ├── dados_preprocessados.csv            ← base + Canal/Regiao em texto + colunas log_*
│   ├── som.pkl / som_pesos.npy             ← mapa treinado (Parte 2)
│   ├── clusters.csv                        ← cluster e outlier de cada cliente (Parte 4)
│   └── scaler.pkl                          ← scaler para classificar clientes novos (Parte 6)
├── figuras/                                ← gráficos para relatório e slides (prefixo = nº da parte)
└── entrega/                                ← relatório final e slides (PDF e PPTX)
```

## Como usar

```bash
git clone https://github.com/som-wholesale/projeto-som-wholesale
cd projeto-som-wholesale
pip install -r requirements.txt
```

Rode os notebooks em ordem (`01` a `06`). Cada um lê o que o anterior salvou em `dados/`. Como os arquivos intermediários já estão em `dados/`, também é possível abrir qualquer notebook isolado. Versões usadas: Python 3.13, `minisom` 2.3.6, `scikit-learn` 1.9.1, `numpy` 2.5.3 e `pandas` 3.0.5. Em 24/11 os seis notebooks foram executados de novo, do zero, com Python 3.14, `numpy` 2.5.5 e `pandas` 3.1.1. Todas as saídas, o `clusters.csv` e os pesos do SOM saíram iguais.

Para carregar os dados prontos na sua parte:

```python
import pandas as pd
X_scaled = pd.read_csv("dados/X_scaled.csv").values     # entrada do SOM / K-Means / PCA
df = pd.read_csv("dados/dados_preprocessados.csv")      # rótulos (Canal, Regiao) e valores originais
```

**No Colab:** rode `!git clone https://github.com/som-wholesale/projeto-som-wholesale` e depois `%cd projeto-som-wholesale`.

## Combinados do grupo

1. **Um notebook por parte**, com nome numerado: `02_Roger_treino_som.ipynb`, `03_Erica_visualizacoes.ipynb`, `04_Ruan_clusters_outliers.ipynb`, `05_Herbe_comparacao.ipynb`, `06_Rafael_perfis.ipynb`. Assim ninguém edita o arquivo do outro e não há conflito.
2. **Sempre `git pull` antes de começar** a trabalhar.
3. **Salve as figuras em `figuras/`** com o número da sua parte no nome (ex.: `03_umatrix.png`).
4. **Use `random_seed=42`** em tudo.
5. **Quem precisar do SOM treinado:** o Roger salva o modelo com `pickle` em `dados/som.pkl`.
6. **Ao terminar sua parte**, atualize o seu status e o histórico no `PROJETO.md`.

## Integrantes

| Parte | Pessoa | Etapa |
|---|---|---|
| 1 | Amabilly | Base, EDA e pré-processamento ✅ |
| 2 | Roger | Treino do SOM ✅ |
| 3 | Erica | Visualizações (U-Matrix, component planes, hit map) ✅ |
| 4 | Ruan | Clusters e outliers ✅ |
| 5 | Herbe | Comparação com K-Means e PCA ✅ |
| 6 | Rafael | Perfis, aplicações e consolidação ✅ |

## Base de dados

Cardoso, M. (2013). *Wholesale customers* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5030X (licença CC BY 4.0).
