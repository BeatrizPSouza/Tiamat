<img width="1041" height="1080" alt="Brasão do projeto Tiamat: Casa Yggdrasil" src="https://github.com/user-attachments/assets/46dc6f5f-1385-402b-aae9-4a3f9be1a914" />

# Tiamat
 
Repositório do Tiamat, trabalho desenvolvido para o projeto final da disciplina de Aprendizado de Máquina, ministrada por Daniel R. Cassar na Ilum Escola de Ciência (CNPEM), e é de autoria de Beatriz Pessoa de Souza.
 
## Descrição do projeto
 
O objetivo é criar um modelo de regressão, comparando diferentes algoritmos e diferentes combinações de hiperparâmetros em um conjunto de dados previamente selecionado. O modelo busca prever a fração de leucócitos presentes em um tumor (*Leukocyte Fraction*), medida usada para distinguir tumores "quentes", muito infiltrados pelo sistema imune e que tendem a responder melhor à imunoterapia, de tumores "frios", a partir de características genômicas do próprio tumor, como carga mutacional, neoantígenos e instabilidade cromossômica.
 
Os dados vêm da *PanImmune Feature Matrix* (Tabela S1 de Thorsson et al., 2018, *The Immune Landscape of Cancer*, Immunity), que reúne informações de 11.080 pacientes do projeto TCGA. Dos pacientes, 10.448 têm valor para o alvo e entram na modelagem, cobrindo 30 tipos de câncer. São usados 11 atributos: 10 numéricos de caracterização genômica e o tipo de câncer. Os atributos que já descrevem a infiltração imune foram excluídos de propósito, para que o modelo não "descubra" o alvo por circularidade.
 
Foram comparados cinco algoritmos de regressão, além de um baseline (`DummyRegressor`, que prevê sempre a média): k-NN, regressão linear polinomial com seleção de atributos (RFE), árvore de decisão, floresta aleatória e Gradient Boosting. Cada algoritmo teve 100 combinações diferentes de hiperparâmetros testadas com o Optuna, usando o RMSE da validação cruzada de 5 partições como critério. Os dados foram divididos em treino (80%) e teste (20%), estratificados pelo tipo de câncer, e o conjunto de teste foi usado uma única vez, no final. Todo o pré-processamento (imputação, log, normalização, one-hot) fica dentro de pipelines, ajustados só nos dados de treino de cada partição, para evitar vazamento de dados.
 
O notebook inclui análise exploratória, comparação dos algoritmos por RMSE, MAE e R², o efeito da redução de dimensionalidade (PCA e RFE) e a interpretação do melhor modelo com importância por permutação e gráficos de dependência parcial. O projeto também registra o uso de ferramentas de IA em sua construção, na seção "Descrição do uso de IA neste trabalho" do notebook.
 
**Resultado principal:** o melhor modelo (Gradient Boosting) reduz o RMSE de 0,151 (baseline) para 0,117 no conjunto de teste e explica cerca de 40% da variância da fração de leucócitos. As conclusões e as limitações estão na última seção do notebook.
 
## Como navegar pelo repositório
 
| Arquivo | O que é |
|---|---|
| `Tiamat.ipynb` | Relatório completo do trabalho: introdução, tratamento e análise exploratória dos dados, algoritmos, otimização com o Optuna, resultados, descrição do uso de IA, referências e conclusões. **Comece por aqui.** |
| `1-s2.0-S1074761318301213-mmc2.xlsx` | Base de dados (Tabela S1 de Thorsson et al., 2018). |
| `tiamat_knn.db`, `tiamat_linear_polinomial.db`, `tiamat_arvore.db`, `tiamat_floresta.db`, `tiamat_boosting.db` | Estudos do Optuna, um por algoritmo, com as 100 tentativas de cada um. O notebook os retoma, então a busca de hiperparâmetros não precisa ser refeita. |
| `requirements.txt` | Lista das bibliotecas necessárias. |
| `README.md` | Este arquivo. |
 
## Instalação e instruções
 
1. Tenha o Python 3.13.7 instalado (versão usada no desenvolvimento).
2. Instale as bibliotecas com `pip install -r requirements.txt`.
3. Abra o `Tiamat.ipynb` no Jupyter, com o arquivo `1-s2.0-S1074761318301213-mmc2.xlsx` na mesma pasta, e execute todas as células em ordem.
Com os arquivos `.db` presentes, o notebook apenas recupera os estudos salvos e treina os modelos finais. Sem eles, o Optuna refaz a busca (500 tentativas no total), o que demora bem mais.
 
## Tecnologias utilizadas
 
- **Python**: linguagem de programação utilizada como base de todo o projeto.
- **Jupyter Notebook**: ambiente de desenvolvimento interativo no qual o projeto foi desenvolvido e documentado.
- **Pandas**: biblioteca usada para ler a base de dados (Tabela S1 em Excel), tratar os dados, selecionar colunas e calcular estatísticas descritivas e correlações.
- **Openpyxl**: biblioteca que o Pandas usa por baixo para ler arquivos `.xlsx`.
- **Numpy**: biblioteca responsável pela manipulação de arrays numéricos e operações vetorizadas, como a transformação logarítmica `log(1 + x)` e o cálculo do fator de inflação de variância.
- **Matplotlib** (versão 3.10.6): biblioteca de visualização gráfica usada para gerar os gráficos do projeto.
- **Seaborn**: biblioteca de visualização estatística, construída sobre o Matplotlib, usada para o estilo dos gráficos, os boxplots e o mapa de calor de correlações.
- **Optuna**: biblioteca de otimização de hiperparâmetros. Foi usada para realizar 100 tentativas diferentes por algoritmo com o amostrador TPE, e os estudos foram salvos em arquivos SQLite (`.db`).
- **SQLAlchemy**: biblioteca que o Optuna usa para gravar e retomar os estudos nos arquivos SQLite.
- **Plotly**: biblioteca usada pelo Optuna para os gráficos de histórico de otimização e de importância dos hiperparâmetros.
- **Scikit-learn**: biblioteca de aprendizado de máquina, da qual foram utilizados:
  - **Divisão e validação:** `train_test_split`, `KFold`, `cross_val_score`.
  - **Baseline:** `DummyRegressor`.
  - **Pré-processamento:** `SimpleImputer`, `StandardScaler`, `OneHotEncoder`, `FunctionTransformer`, `PolynomialFeatures`, `ColumnTransformer`, `Pipeline`.
  - **Redução e seleção de atributos:** `PCA`, `RFE`.
  - **Algoritmos de regressão:** `KNeighborsRegressor`, `LinearRegression`, `DecisionTreeRegressor`, `RandomForestRegressor`, `HistGradientBoostingRegressor`.
  - **Métricas:** `root_mean_squared_error`, `mean_absolute_error`, `r2_score`.
  - **Explicação dos modelos:** `permutation_importance`, `PartialDependenceDisplay`.
- **Biblioteca padrão do Python**: `functools` (função `partial`) e `warnings`.
## Brasão do projeto
 
### Casa Yggdrasil: o ciclo do conhecimento
 
> *"O conhecimento nasce da análise dos dados e se renova a cada ciclo."*
 
O brasão do projeto, inspirado na mitologia nórdica, une três ideias: a vida, o câncer e o aprendizado de máquina.
 
- **A árvore** é inspirada em Yggdrasil, a árvore do mundo da mitologia nórdica, e simboliza a complexidade da vida. Suas **raízes** representam a base de dados que sustenta o projeto, e seus **galhos** representam a diversidade dos processos biológicos e dos tipos de câncer (a base usada cobre 30 deles).
- **A fita** que atravessa o tronco representa o câncer, a conscientização e a importância da pesquisa científica. Por estar integrada à árvore, ela simboliza a relação entre a doença e os sistemas biológicos em que ela se desenvolve.
- **A serpente** é inspirada em Jörmungandr, a serpente de Midgard, que na mitologia nórdica envolve o mundo e morde a própria cauda, imagem que lembra o ouroboros. Ela representa os ciclos de aprendizagem e aprimoramento: assim como a serpente retorna ao ponto de origem, o aprendizado de máquina passa por etapas de treinamento, avaliação e refinamento para identificar padrões nos dados.
### Autoria e criação
 
O brasão é de autoria de **Beatriz Pessoa de Souza** (Ilum Escola de Ciência). O desenho foi feito digitalmente por mim no aplicativo **ibis Paint**, sem uso de IA. As imagens listadas abaixo foram usadas apenas como **referência visual** e não foram copiadas ou incorporadas ao desenho.
 
Houve uso de IA apenas para ajudar a elaborar a história por trás do brasão, unindo os conceitos da mitologia aos conceitos presentes no projeto. Foi usado o Microsoft Copilot em 18 out. 2026.
 
### Referências do brasão
 
**Mitologia**
 
- CRAZY ALCHEMIST. *Jörmungandr: The Midgard Serpent*. Crazy Alchemist, 2026. Disponível em: https://www.crazyalchemist.com/bestiary/jormungandr/. Acesso em: 17 out. 2026.
**Referências visuais**
 
- MAGNIFIC (antes Freepik). **[PREENCHER: título da página do vetor]**. Autoria: **[PREENCHER: autor, se a página informar]**. Disponível em: **[PREENCHER: link da página do vetor]**. Acesso em: 16 out. 2026.
- THE NORSE WAY. *Wooden Yggdrasil Wall Art*. Disponível em: https://thenorseway.com/products/wooden-tree-of-life-wall-art. Acesso em: 16 out. 2026.
- **[PREENCHER: designer da fonte, ou use FFONTS.NET se a página não informar]**. *Yggdrasil* (fonte tipográfica). FFonts.net. Disponível em: https://pt.ffonts.net/Yggdrasil.font. Acesso em: 17 out. 2026.
## Professores
 
**Daniel Roberto Cassar**: doutorado em Ciência e Engenharia de Materiais (UFSCar) e pós-doutorado na UFSCar. Área de atuação: informática dos materiais.
 
## Autoria
 
Beatriz Pessoa de Souza, discente do curso de bacharelado em Ciência e Tecnologia na Ilum Escola de Ciência.
 
## Referências
 
THORSSON, V. et al. The Immune Landscape of Cancer. *Immunity*, v. 48, n. 4, p. 812-830, 2018. Dados: Tabela S1 (*PanImmune Feature Matrix*).
 
CASSAR, D. R. *ATP-203 – Aprendizado de Máquina*. Material de aula (notebooks e slides), Ilum Escola de Ciência, 2026.
 
FACELI, K. et al. *Inteligência Artificial: uma abordagem de Aprendizado de Máquina*. 2. ed. Rio de Janeiro: LTC, 2021.
 
PEDREGOSA, F. et al. Scikit-learn: Machine Learning in Python. *Journal of Machine Learning Research*, v. 12, p. 2825-2830, 2011.
 
AKIBA, T. et al. Optuna: A Next-generation Hyperparameter Optimization Framework. In: *Proceedings of the 25th ACM SIGKDD*, 2019.
 
ANTHROPIC. *Claude* (assistente de IA), usado conforme descrito na seção 6 do notebook ("Descrição do uso de IA neste trabalho").
 
MICROSOFT. *Copilot* (assistente de IA), usado em 18 out. 2026 para elaborar a história do brasão, conforme descrito na seção "Brasão do projeto" deste README.
