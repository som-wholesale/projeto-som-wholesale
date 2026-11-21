# Projeto Final: SOM (Self-Organizing Map) na base Wholesale Customers

Projeto final de **Aprendizado Não Supervisionado**: segmentação de 440 clientes de um atacadista com SOM, comparada com K-Means e PCA.

> 👉 **O que você precisa fazer está no [TAREFAS.md](TAREFAS.md)**: a parte de cada um, o que recebe, o que entrega e o passo a passo.
>
> 📌 Contexto e decisões estão no [PROJETO.md](PROJETO.md), que tem as decisões, a divisão de tarefas, o passo a passo de cada parte, o status e o histórico.

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
└── figuras/                                ← gráficos para relatório e slides
```

## Como usar

```bash
git clone <url-do-repositorio>
cd <pasta>
pip install -r requirements.txt
```

Rode os notebooks em ordem (`01` a `06`). Cada um lê o que o anterior salvou em `dados/`. Versões usadas: Python 3.13, `minisom` 2.3.6, `scikit-learn` 1.9.1, `numpy` 2.5.3 e `pandas` 3.0.5.

Para carregar os dados prontos na sua parte:

```python
import pandas as pd
X_scaled = pd.read_csv("dados/X_scaled.csv").values     # entrada do SOM / K-Means / PCA
df = pd.read_csv("dados/dados_preprocessados.csv")      # rótulos (Canal, Regiao) e valores originais
```

**No Colab:** rode `!git clone <url-do-repositorio>` e depois `%cd <pasta>`.

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
