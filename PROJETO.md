# PROJETO.md: Projeto Final de Aprendizado Não Supervisionado (SOM)

> **Para que serve este arquivo:** registro central do projeto. Quem retomar o trabalho deve ler este arquivo primeiro e atualizá-lo sempre que algo mudar: decisões, status de cada parte e o histórico no final.

---

## 1. Contexto

- **Disciplina:** Aprendizado Não Supervisionado (projeto final em grupo).
- **Opções dadas pelo professor:**
  1. Autoencoder: compressão, reconstrução e detecção de anomalias.
  2. **SOM (Self-Organizing Map): segmentação e visualização** ← **ESCOLHIDO**
  3. GAN: geração sintética e avaliação de qualidade.
- **Por que o SOM:** é o mais fácil e rápido. Treina em segundos sem GPU, a biblioteca `minisom` já entrega U-Matrix, BMUs e métricas, e a comparação com K-Means e PCA é simples no `sklearn`. O Autoencoder seria a 2ª opção; a GAN foi descartada por ter treino instável e avaliação subjetiva.

## 2. Enunciado do professor (Projeto 2: SOM)

- Escolher uma base com múltiplas variáveis numéricas ou vetorizadas.
- Padronizar os dados e treinar um Self-Organizing Map.
- Interpretar regiões do mapa, distâncias, agrupamentos e outliers.
- Comparar os achados com K-Means, PCA ou outra técnica auxiliar.
- Nomear os perfis encontrados e discutir possíveis aplicações.

**Entrega exigida:** mapa treinado, U-Matrix, clusters/perfis e interpretação dos grupos.

## 3. Decisões técnicas

| Item | Decisão | Status |
|---|---|---|
| Base de dados | **Wholesale Customers** (UCI/Kaggle): 440 clientes, 6 variáveis de gasto anual (Fresh, Milk, Grocery, Frozen, Detergents_Paper, Delicassen), mais `Channel` e `Region` | ✅ Usada em todas as partes |
| Pré-processamento | `np.log1p` nas 6 variáveis de gasto, depois `StandardScaler` | ✅ Feito (Parte 1) |
| Outliers no pré-processamento | **Não remover.** Ficam todos os 440 clientes; a detecção é feita depois pelo SOM (Ruan) | ✅ Definido (Parte 1) |
| Biblioteca | `minisom` 2.3.6 + `scikit-learn` 1.9.1 + `pandas`/`matplotlib`/`seaborn` | ✅ Usado |
| Tamanho do mapa | 10×10 (regra: neurônios ≈ 5·√N; com N = 440, cerca de 105). Nenhum neurônio ficou vazio | ✅ Usado |
| Hiperparâmetros | **`sigma=2.0`, `learning_rate=0.3`**, `pca_weights_init`, 5000 iterações, com QE = 0,898 e TE = 0,050. Critério: TE médio (5 sementes) ≤ 0,10; depois menor QE; empate (diferença menor que o desvio entre sementes) decidido pelo menor TE | ✅ Definido (Parte 2) |
| Semente | `random_seed=42` em tudo (SOM e K-Means) | Definido |
| Ambiente | Notebooks por parte (o primeiro é `01_Amabilly_EDA_preprocessamento.ipynb`); roda local ou no Colab (baixa a base da UCI se o CSV não estiver em `dados/`) | Em uso |
| Repositório | **GitHub público:** https://github.com/som-wholesale/projeto-som-wholesale (branch `main`; criado como `Mabynha20/projeto-som-wholesale`). Cada pessoa cria o próprio notebook numerado (`02_Roger_...`, `03_Erica_...` etc.), faz `git pull` antes de começar e atualiza o PROJETO.md ao terminar. Combinados completos no README | ✅ Criado |
| Paleta dos gráficos | Azul `#2a78d6` (série única e Horeca), laranja `#eb6834` (Varejo), cinza `#52514e` (referências). Manter nas outras partes | Definido (Parte 1) |
| Métricas | Erro de quantização, erro topográfico, silhouette, Adjusted Rand Index e estabilidade entre sementes (ARI em 10 sementes) | ✅ Usado |
| Outliers (no SOM) | Erro de quantização individual > percentil 95 (1,87): 22 clientes. Conferido com a cerca de Tukey sobre o erro (23 clientes, concorda em 439 de 440), porque o percentil 95 sozinho sempre marca 5% | ✅ Feito (Parte 4) |
| Passagem entre as partes | Roger salva o mapa em `dados/som.pkl`. Ruan salva `dados/clusters.csv` (`bmu_x`, `bmu_y`, `cluster`, `erro_quant`, `outlier`; uma linha por cliente). Herbe usa o k e os clusters do Ruan | ✅ Definido (TAREFAS.md) |
| Clusters | K-Means sobre os **pesos dos neurônios**, **k = 3** (cotovelo em 3; silhouette prefere k = 2 por só 0,027, e k = 2 repete o `Channel`) | ✅ Feito (Parte 4) |
| U-Matrix | Escala de cinza do claro ao escuro: **escuro = fronteira**, claro = grupo (o texto antigo dizia o contrário) | ✅ Corrigido (Parte 3) |
| Scaler | O `StandardScaler` da Parte 1 foi recriado (reproduz o `X_scaled`) e salvo em `dados/scaler.pkl`, para classificar clientes novos | ✅ Feito (Parte 6) |

## 4. Integrantes e divisão de tarefas

| # | Pessoa | Etapa | Entrega | Status |
|---|---|---|---|---|
| 1 | **Amabilly** | Base, análise exploratória e pré-processamento | `X_scaled` pronto e EDA com gráficos | ✅ Concluído |
| 2 | **Roger** | Treino do SOM e ajuste de parâmetros | Mapa treinado e tabela de testes (QE/TE) | ✅ Concluído |
| 3 | **Erica** | Visualizações do mapa | U-Matrix, component planes, hit map, sobreposição de `Channel` | ✅ Concluído |
| 4 | **Ruan** | Clusters no mapa e outliers | k escolhido, mapa colorido por cluster, lista de outliers | ✅ Concluído |
| 5 | **Herbe** | Comparação com K-Means e PCA | Gráficos PCA 2D, ARI, silhouette, tabela de prós e contras | ✅ Concluído |
| 6 | **Rafael** | Perfis, aplicações e consolidação | Perfis nomeados, aplicações, conclusão, relatório e slides | ✅ Concluído |

Cada pessoa escreve no relatório a parte que fez e apresenta essa parte (cerca de 2-3 minutos cada).

Legenda de status: ⬜ Não iniciado · 🟨 Em andamento · ✅ Concluído

## 5. Arquivos do projeto

```
projeto final/
├── PROJETO.md                              ← este arquivo
├── TAREFAS.md                              ← o que cada um faz (para os colegas)
├── README.md / requirements.txt / .gitignore
├── 01_Amabilly_EDA_preprocessamento.ipynb  ← Parte 1 (executado, com saídas)
├── 02_Roger_treino_som.ipynb               ← Parte 2
├── 03_Erica_visualizacoes.ipynb            ← Parte 3
├── 04_Ruan_clusters_outliers.ipynb         ← Parte 4
├── 05_Herbe_comparacao.ipynb               ← Parte 5
├── 06_Rafael_perfis.ipynb                  ← Parte 6
├── dados/
│   ├── Wholesale customers data.csv        ← base original (UCI)
│   ├── X_scaled.csv                        ← 440×6, log1p + StandardScaler (entrada do SOM)
│   ├── dados_preprocessados.csv            ← base original + Canal/Regiao em texto + colunas log_*
│   ├── som.pkl / som_pesos.npy             ← mapa treinado (Parte 2); os pesos são o plano B se o pickle não abrir
│   ├── clusters.csv                        ← bmu_x, bmu_y, cluster, erro_quant, outlier (Parte 4)
│   └── scaler.pkl                          ← StandardScaler da Parte 1, para clientes novos (Parte 6)
├── figuras/                                ← EDA (Parte 1) e figuras das Partes 2 a 6 (prefixo = nº da parte)
└── entrega/                                ← relatorio_final.pdf, slides_apresentacao.pdf e .pptx (com notas de fala)
```

Como as outras partes carregam os dados:
```python
import pandas as pd
X_scaled = pd.read_csv("dados/X_scaled.csv").values      # entrada do SOM / K-Means / PCA
df = pd.read_csv("dados/dados_preprocessados.csv")       # rótulos (Canal, Regiao) e valores originais
```
A ordem das linhas é a mesma em todos os arquivos (índice 0 a 439).

## 6. Passo a passo por etapa

### 6.1 Amabilly: dados e pré-processamento ✅
Feito em `01_Amabilly_EDA_preprocessamento.ipynb`. **Principais achados (para o relatório e os slides):**
- **Qualidade:** a base está limpa. Não há ausentes, duplicatas nem gastos ≤ 0.
- **Assimetria forte:** em todas as variáveis a média é maior que a mediana. A assimetria vai de 2,6 (`Fresh`) a 11,2 (`Delicassen`). Depois do `log1p`, fica entre −1,6 e −0,2.
- **Escalas diferentes:** `Fresh` tem média de cerca de 12 mil e `Delicassen`, de cerca de 1,5 mil. Por isso o `StandardScaler`.
- **Correlação, dois blocos:**
  - `Grocery` × `Detergents_Paper` = 0,92 (0,80 após o log) e `Milk` × `Grocery` = 0,73. É o bloco "mercearia/varejo".
  - `Fresh` × `Frozen` = 0,35. É o bloco "perecíveis/Horeca", mais fraco.
- **Channel:** 298 Horeca (68%) e 142 Varejo (32%), desbalanceado.
  - O Varejo gasta cerca de 14,6× mais em `Detergents_Paper`, 4,6× mais em `Grocery` e 3,6× mais em `Milk` (medianas).
  - O Horeca gasta cerca de 1,6× mais em `Fresh` e 1,9× mais em `Frozen`.
  - Deve aparecer bem separado no SOM.
- **Outliers:** pelo IQR, 42 clientes têm ao menos um valor fora do intervalo após o log. Eles foram mantidos. Os candidatos mais fortes são clientes com gasto quase zero em `Fresh`, `Delicassen` ou `Detergents_Paper` (valores de 3 a 10).
- **Hipótese para o Rafael:** o SOM deve separar um eixo "mercearia/limpeza" de um eixo "frescos/congelados".

### 6.2 Roger: treino do SOM
1. Instalar com `pip install minisom`.
2. Mapa 10×10; testar `sigma` em {1.0, 1.5, 2.0} e `learning_rate` em {0.3, 0.5}.
3. Comparar as combinações pelo erro de quantização (QE) e pelo erro topográfico (TE).
4. Escolher a melhor configuração e documentar a escolha numa tabela.

```python
from minisom import MiniSom
som = MiniSom(10, 10, X_scaled.shape[1], sigma=1.5, learning_rate=0.5, random_seed=42)
som.pca_weights_init(X_scaled)
som.train_random(X_scaled, 5000)
print(som.quantization_error(X_scaled), som.topographic_error(X_scaled))
```

### 6.3 Erica: visualizações
1. **U-Matrix:** `som.distance_map()`. Com a escala de cinza do claro ao escuro, regiões escuras indicam fronteiras entre grupos e regiões claras indicam grupos.
2. **Component planes:** `som.get_weights()[:, :, i]`, um mapa por variável.
3. **Hit map:** `som.activation_response(X_scaled)`.
4. Sobrepor `Channel` (Horeca vs. Varejo) no mapa para validar a separação. A coluna `Canal` já está em texto em `dados_preprocessados.csv`.
5. Escrever uma interpretação curta de cada gráfico.

### 6.4 Ruan: clusters e outliers
1. Rodar K-Means sobre os pesos: `som.get_weights().reshape(-1, n_features)`.
2. Escolher k pelo cotovelo e pelo silhouette (k = 2 a 6).
3. Atribuir a cada cliente o cluster do seu BMU (`som.winner(x)`).
4. Identificar os outliers pelo erro de quantização individual:

```python
erro = np.linalg.norm(X_scaled - som.quantization(X_scaled), axis=1)
outliers = np.where(erro > np.percentile(erro, 95))[0]
```
5. Pintar o mapa com as cores dos clusters.

### 6.5 Herbe: comparação
1. Rodar K-Means direto em `X_scaled`, com o mesmo k.
2. Rodar PCA em 2 componentes e fazer dois gráficos de dispersão (cores do SOM e cores do K-Means).
3. Calcular `adjusted_rand_score` entre os dois agrupamentos e o silhouette de cada um.
4. Discutir: o SOM preserva a topologia e mostra transições; o K-Means só particiona; o PCA é linear.
5. Montar uma tabela de prós e contras.

### 6.6 Rafael: perfis e consolidação
1. Calcular o perfil de cada cluster pela **mediana** em valores reais (`base.groupby("cluster")[GASTOS].median()`; a média seria puxada pelos clientes gigantes) e fazer um gráfico de barras.
2. Nomear os perfis. Hipóteses a confirmar com os dados:
   - "Restaurantes e hotéis" (alto Fresh/Frozen)
   - "Mercados e varejo" (alto Grocery/Detergents_Paper)
   - "Pequenos compradores" (gasto baixo em tudo)
   - "Clientes atípicos" (outliers)
3. Discutir aplicações: marketing direcionado, logística, estoque, detecção de clientes anômalos.
4. Escrever a conclusão e as limitações (tamanho da base, sensibilidade a hiperparâmetros, escolha de k).
5. Consolidar o notebook final e os slides.

### 6.7 Principais achados das Partes 2 a 6 (para o relatório e os slides)
- **Parte 2:** QE e TE andam em sentidos opostos (sigma 1,0 tem QE 0,71, mas TE até 0,36). Escolha: sigma 2,0 e lr 0,3, com QE = 0,898 e TE = 0,050. 20 mil iterações melhoram o QE em menos de 3%.
- **Parte 3:** mercearia/limpeza cresce da esquerda para a direita e perecíveis de baixo para cima. Horeca à esquerda e Varejo à direita, com 92% de pureza por neurônio, sem o canal entrar no treino. A U-Matrix não tem "ilhas": só a linha de baixo é bem separada.
- **Parte 4:** k = 3 (203 / 93 / 144 clientes). Silhouette ≤ 0,33 em qualquer k, ou seja, os dados são um contínuo. 22 outliers: 17 com gasto ≤ 50 numa categoria e 19 Horeca.
- **Parte 5:** o K-Means direto concorda com o SOM em 88% dos clientes (ARI 0,68). Silhouette de 0,26 (K-Means) contra 0,22 (SOM). PCA: 71% da variância em 2 componentes, com os mesmos dois eixos. Estabilidade em 10 sementes: K-Means 0,98 e SOM 0,71; os perfis se mantêm, as fronteiras não.
- **Parte 6:** perfis **Horeca de perecíveis** (46% dos clientes, 29% do faturamento), **Varejo de mercearia e limpeza** (21%, 20%) e **Grandes compradores completos** (33%, 52%), mais 22 clientes atípicos. "Pequenos compradores" só aparecem com k = 4, como subgrupo do Horeca. A região não diferencia os perfis (p = 0,85).

## 7. Cronograma e dependências

| Fase | Quem | Depende de |
|---|---|---|
| Dias 1-2 | Amabilly | Ninguém (✅ concluído) |
| Dias 2-3 | Roger | Amabilly (`X_scaled`, já disponível) |
| Dias 3-5 | Erica, Ruan e Herbe em paralelo | Roger (mapa treinado) |
| Dias 5-6 | Rafael | Ruan (clusters) e Herbe (comparação) |
| Dia 7 | Todos | Revisão e ensaio da apresentação |

Enquanto espera as outras etapas, o Rafael pode adiantar a estrutura dos slides e a teoria do SOM.

## 8. Checklist da entrega final

- [x] Base escolhida, EDA e pré-processamento (`X_scaled`)
- [x] Mapa SOM treinado (com hiperparâmetros documentados)
- [x] U-Matrix com interpretação
- [x] Component planes e hit map
- [x] Clusters e perfis nomeados
- [x] Outliers identificados e discutidos
- [x] Comparação com K-Means e PCA (ARI e silhouette)
- [x] Aplicações possíveis
- [x] Conclusão e limitações
- [x] Notebooks 01 a 06 rodando em sequência, do início ao fim
- [x] Relatório escrito e slides da apresentação (com notas de fala por integrante)

## 9. Próximos passos, pendências e dúvidas em aberto

- **Todos:** cada um revisa o próprio notebook e as notas de fala do seu slide, e confere se consegue explicar cada número. O professor pode perguntar sobre qualquer parte.
- **Formato da entrega (definido):** notebooks separados, executados, mais o relatório e os slides em PDF na pasta `entrega/`. O `.pptx` traz as notas de fala.
- Opcional: validar os perfis com alguém da área comercial e conferir os 22 clientes atípicos.

## 10. Histórico de atualizações

| Data | O que mudou | Por quê |
|---|---|---|
| 2026-11-05 | Projeto 2 (SOM) escolhido; base, divisão de tarefas, passo a passo e cronograma definidos. Nenhum código feito ainda. | Planejamento inicial. |
| 2026-11-09 | **Parte 1 (Amabilly) concluída.** Base baixada da UCI para `dados/`. Criado e executado `01_Amabilly_EDA_preprocessamento.ipynb` (qualidade, descritivas, histogramas, boxplots, correlação, gastos por canal, log1p, padronização). Gerados `X_scaled.csv`, `dados_preprocessados.csv` e 7 figuras em `figuras/`. Achados registrados na seção 6.1. | Desbloquear o treino do SOM (Roger). |
| 2026-11-09 | Decisão: **outliers não são removidos** no pré-processamento. | A base é pequena (440), clientes grandes são um perfil real e a detecção de outliers é tarefa do SOM (Ruan). |
| 2026-11-09 | Criado o `TAREFAS.md`, com a parte de cada um (o que recebe, o que entrega, passo a passo com código e cronograma), e linkado no README. Definida a passagem de arquivos entre as partes (`som.pkl`, `clusters.csv`). O Herbe passa a depender também do Ruan. | Os colegas precisam saber exatamente o que fazer e em que formato entregar para a etapa seguinte. |
| 2026-11-09 | Convites de colaborador enviados para Erica, Rafael, Herbe e Ruan; o do Roger está pendente. | Dar permissão de envio (push) para cada um subir a própria parte. |
| 2026-11-09 | Criado o repositório público no GitHub (https://github.com/Mabynha20/projeto-som-wholesale), com README (como usar e combinados do grupo), `requirements.txt` e `.gitignore`. Também substitui o Colab como forma principal de compartilhar. | Dar acesso aos colegas para continuarem as partes deles, com histórico e sem sobrescrever o trabalho do outro. |
| 2026-11-09 | Definida a paleta padrão dos gráficos e a estrutura de pastas (`dados/`, `figuras/`). Notebook roda local ou no Colab. | Manter um visual consistente entre as partes e facilitar o compartilhamento. |
| 2026-11-21 | **Partes 2 a 6 concluídas.** Criados e executados os notebooks `02` a `06`. Gerados `som.pkl`, `som_pesos.npy`, `clusters.csv`, `scaler.pkl` e as figuras das Partes 2 a 6. Achados na seção 6.7. | Fechar o pipeline completo do enunciado. |
| 2026-11-21 | Hiperparâmetros escolhidos por critério explícito (TE ≤ 0,10, depois menor QE, com 5 sementes): sigma 2,0 e lr 0,3. O `sort_values(["TE", "QE"])` do plano escolheria sigma 3,0 se ele estivesse na grade. | QE e TE brigam entre si; sem regra, a escolha seria arbitrária. |
| 2026-11-21 | k = 3 (cotovelo) em vez de k = 2 (silhouette, diferença de só 0,027). Outliers conferidos com a cerca de Tukey. Descrição da U-Matrix corrigida (escuro = fronteira). | k = 2 só repetiria o canal; o percentil 95 sozinho é circular. |
| 2026-11-21 | Relatório escrito e slides (14, com notas de fala por integrante) criados. | Entrega final. |
| 2026-11-21 | Slides refeitos para a gravação de 10 minutos: 9 slides, uma mensagem por slide, roteiro cronometrado nas notas e construção compatível com o Canva. | O tempo total é de 10 minutos. |
| 2026-11-24 | Criada a pasta `entrega/` com o relatório e os slides (PDF e PPTX). Os arquivos gerados pelas Partes 2 a 6 foram movidos para `dados/` e `figuras/`. Os 6 notebooks foram executados de novo, do zero, e reproduziram todas as saídas. | Entrega final autocontida no repositório. Os notebooks 03 a 06 não abriam sozinhos, porque procuravam em `dados/` arquivos que estavam na raiz. |
