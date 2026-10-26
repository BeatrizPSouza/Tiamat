<img width="1041" height="1080" alt="Apresentação portfólio designer projetos profissional corporativo branco" src="https://github.com/user-attachments/assets/46dc6f5f-1385-402b-aae9-4a3f9be1a914" />


# Tiamat
Repositório do Tiamat, trabalho desenvolvido para o projeto final da disciplina de Aprendizado de Máquina (ATP-203), ministrada por Daniel R. Cassar na Ilum Escola de Ciência (CNPEM), e é de autoria de Beatriz Pessoa de Souza.

### Descrição do projeto

Seu objetivo é criar um modelo de regressão utilizando diferentes algoritmos em um conjunto de dados previamente selecionado, considerando diferentes combinações de hiperparâmetros. O modelo buscará prever a fração de leucócitos presentes em um tumor (*Leukocyte Fraction*), medida usada para distinguir tumores "quentes", muito infiltrados pelo sistema imune e que tendem a responder melhor à imunoterapia, de tumores "frios", a partir de características genômicas do próprio tumor, como carga mutacional, neoantígenos e instabilidade cromossômica.

Os dados vêm da *PanImmune Feature Matrix* (Tabela S1 de Thorsson et al., 2018, *The Immune Landscape of Cancer*, Immunity), que reúne informações de 11.080 pacientes do projeto TCGA. Dos pacientes, 10.448 têm valor para o alvo e entram na modelagem, cobrindo 30 tipos de câncer. São usados 11 atributos: 10 numéricos de caracterização genômica e o tipo de câncer. Os atributos que já descrevem a infiltração imune foram excluídos de propósito, para que o modelo não "descubra" o alvo por circularidade.

Foram comparados cinco algoritmos de regressão, além de um baseline (`DummyRegressor`, que prevê sempre a média): k-NN, regressão linear polinomial com seleção de atributos (RFE), árvore de decisão, floresta aleatória e Gradient Boosting. Cada algoritmo teve 100 combinações diferentes de hiperparâmetros testadas com o Optuna, usando o RMSE da validação cruzada de 5 partições como critério. Os dados foram divididos em treino (80%) e teste (20%), estratificados pelo tipo de câncer, e o conjunto de teste foi usado uma única vez, no final. Todo o pré-processamento (imputação, log, normalização, one-hot) fica dentro de pipelines, ajustados só nos dados de treino de cada partição, para evitar vazamento de dados.

O notebook inclui análise exploratória, comparação dos algoritmos por RMSE, MAE e R², o efeito da redução de dimensionalidade (PCA e RFE) e a interpretação do melhor modelo com importância por permutação e gráficos de dependência parcial. O projeto também registra o uso de ferramentas de IA em sua construção, na seção "Descrição do uso de IA neste trabalho" do notebook.


### Instalação e instruções

Para executar o projeto, é necessário ter o Python 3.13.7 instalado, bem como as bibliotecas numpy, matplotlib e scikit-learn. Em seguida, basta abrir os arquivos Tiamat.ipynb e 1-s2.0-S1074761318301213-mmc2.xlsx pelo Jupyter Notebook, executar todas as células em ordem.


### Tecnologias utilizadas

- **Python**: Linguagem de programação utilizada como base de todo o projeto.
- **Jupyter Notebook**: Ambiente de desenvolvimento interativo no qual o projeto foi desenvolvido e documentado.
- **Pandas**: Biblioteca usada para ler a base de dados (Tabela S1 em Excel), tratar os dados, selecionar colunas e calcular estatísticas descritivas e correlações.
- **Openpyxl**: Biblioteca que o Pandas usa por baixo para ler arquivos `.xlsx`.
- **Numpy**: Biblioteca responsável pela manipulação de arrays numéricos e operações vetorizadas, como a transformação logarítmica `log(1 + x)` e o cálculo do fator de inflação de variância.
- **Matplotlib** (versão 3.10.6): Biblioteca de visualização gráfica usada para gerar os gráficos do projeto.
- **Seaborn**: Biblioteca de visualização estatística, construída sobre o Matplotlib, usada para o estilo dos gráficos, os boxplots e o mapa de calor de correlações.
- **Optuna**: Biblioteca de otimização de hiperparâmetros. Foi usada para realizar 100 tentativas diferentes por algoritmo com o amostrador TPE, e os estudos foram salvos em arquivos SQLite (`.db`).
- **Scikit-learn**: Biblioteca de aprendizado de máquina, da qual foram utilizados:
  - **Divisão e validação:** `train_test_split`, `KFold`, `cross_val_score`.
  - **Baseline:** `DummyRegressor`.
  - **Pré-processamento:** `SimpleImputer`, `StandardScaler`, `OneHotEncoder`, `FunctionTransformer`, `PolynomialFeatures`, `ColumnTransformer`, `Pipeline`.
  - **Redução e seleção de atributos:** `PCA`, `RFE`.
  - **Algoritmos de regressão:** `KNeighborsRegressor`, `LinearRegression`, `DecisionTreeRegressor`, `RandomForestRegressor`, `HistGradientBoostingRegressor`.
  - **Métricas:** `root_mean_squared_error`, `mean_absolute_error`, `r2_score`.
  - **Explicação dos modelos:** `permutation_importance`, `PartialDependenceDisplay`.
- **Biblioteca padrão do Python**: `functools` (função `partial`) e `warnings`.

### Professores

##### Daniel Roberto Cassar
Doutorado: Ciência e Engenharia de Materiais (UFSCar) - Pós-doutorado: UFSCar. Área de atuação: Informática dos materiais


### Autoria

Beatriz Pessoa de Souza discente do curso de bacharelado em ciência e tecnologia na Ilum Escola de Ciência.

### Referência

THORSSON, V. et al. The Immune Landscape of Cancer. Immunity, v. 48, n. 4, p. 812-830, 2018. Dados: Tabela S1 (PanImmune Feature Matrix).

CASSAR, D. R. ATP-203 – Aprendizado de Máquina. Material de aula (notebooks e slides), Ilum Escola de Ciência, 2026.

FACELI, K. et al. Inteligência Artificial: uma abordagem de Aprendizado de Máquina. 2. ed. Rio de Janeiro: LTC, 2021.

PEDREGOSA, F. et al. Scikit-learn: Machine Learning in Python. Journal of Machine Learning Research, v. 12, p. 2825-2830, 2011.

AKIBA, T. et al. Optuna: A Next-generation Hyperparameter Optimization Framework. In: Proceedings of the 25th ACM SIGKDD, 2019.

ANTHROPIC. Claude (assistente de IA), usado conforme descrito na seção abaixo.
Descrição do uso de IA neste trabalho
