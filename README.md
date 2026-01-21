# Projeto 6 - Análise e Predição de Níveis de Ruído Urbano

## 📋 Descrição do Projeto

Este projeto realiza análise exploratória e modelagem preditiva de níveis de ruído urbano, utilizando dados de sensores distribuídos em áreas urbanas. O objetivo é identificar os principais fatores que influenciam os níveis de ruído e construir modelos de regressão para prever os níveis de decibéis.

## 🎯 Objetivo

Construir modelos de regressão para prever níveis de ruído urbano (`decibel_level`) com base em características ambientais, temporais, geográficas e de tráfego, fornecendo insights para planejamento urbano e mitigação de ruído.

## 📊 Dataset

O dataset contém **2.000 registros** de medições de ruído urbano com as seguintes variáveis:

### Variáveis Independentes (Features)

**Geográficas:**
- **latitude**: Latitude da medição
- **longitude**: Longitude da medição
- **sensor_id**: Identificador do sensor

**Temporais:**
- **datetime**: Data e hora da medição
- **hour**: Hora do dia (0-23)
- **day_of_week**: Dia da semana (0=Segunda, 6=Domingo)
- **is_weekend**: Indicador de fim de semana (0/1)
- **holiday**: Indicador de feriado (0/1)

**Climáticas:**
- **temperature_c**: Temperatura em graus Celsius
- **humidity_%**: Umidade relativa do ar em %
- **wind_speed_kmh**: Velocidade do vento em km/h
- **precipitation_mm**: Precipitação em mm

**Tráfego e Infraestrutura:**
- **traffic_density**: Densidade de tráfego
- **vehicle_count**: Contagem de veículos
- **honking_events**: Eventos de buzina
- **near_airport**: Proximidade de aeroporto (0/1)
- **near_highway**: Proximidade de rodovia (0/1)
- **near_construction**: Proximidade de construção (0/1)

**Características da Área:**
- **population_density**: Densidade populacional
- **park_proximity**: Proximidade de parque (0/1)
- **industrial_zone**: Zona industrial (0/1)
- **school_zone**: Zona escolar (0/1)
- **public_event**: Evento público (0/1)
- **noise_complaints**: Número de reclamações de ruído

### Variável Dependente (Target)
- **decibel_level**: Nível de ruído em decibéis (dB)

## 🔧 Tecnologias Utilizadas

- **Python 3.x**
- **Pandas**: Manipulação e análise de dados
- **NumPy**: Computação numérica
- **Matplotlib & Seaborn**: Visualização de dados
- **Scikit-learn**: Modelagem e avaliação de machine learning
- **XGBoost**: Algoritmo de gradient boosting

## 📁 Estrutura do Projeto

```
Machine_Learning - projeto_6/
│
├── data/
│   └── urban_noise_levels.csv    # Dataset original
│
├── projeto_6.ipynb               # Notebook principal
│
└── README.md                     # Este arquivo
```

## 🚀 Como Executar

### Pré-requisitos

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost jupyter
```

### Executando o Notebook

1. Clone o repositório ou baixe os arquivos
2. Certifique-se de que o arquivo `urban_noise_levels.csv` está na pasta `data/`
3. Abra o Jupyter Notebook:
   ```bash
   jupyter notebook projeto_6.ipynb
   ```
4. Execute as células sequencialmente

## 📈 Metodologia

### 1. Coleta e Preparação dos Dados
- Carregamento do dataset
- Verificação de qualidade (valores faltantes, duplicatas)
- Conversão de tipos de dados

### 2. Análise Exploratória de Dados (EDA)
- Estatísticas descritivas
- Distribuição da variável alvo (decibel_level)
- Matriz de correlação
- Análise temporal: ruído por hora do dia e dia da semana
- Visualizações de padrões

### 3. Pré-processamento
- Conversão de datetime
- Seleção de features
- Divisão dos dados: 80% treino / 20% teste
- Padronização: StandardScaler para todas as features

### 4. Modelagem

Três modelos foram implementados e comparados:

#### 4.1 Regressão Linear (Baseline)
- Modelo linear simples para estabelecer linha de base

#### 4.2 Random Forest (Otimizado)
- Ensemble de árvores de decisão
- Otimização com RandomizedSearchCV
- Hiperparâmetros testados:
  - `n_estimators`: [100, 200, 300]
  - `max_features`: ['sqrt', 'log2']
  - `max_depth`: [None, 10, 20, 30]
  - `min_samples_split`: [2, 5, 10]
  - `min_samples_leaf`: [1, 2, 4]

#### 4.3 XGBoost Regressor
- Gradient boosting avançado
- Configuração: 100 estimadores, learning_rate=0.1, max_depth=5

### 5. Avaliação

Métricas utilizadas:
- **MAE (Mean Absolute Error)**: Erro médio absoluto em decibéis
- **RMSE (Root Mean Squared Error)**: Raiz do erro quadrático médio
- **R² (Coefficient of Determination)**: Proporção da variância explicada

## 📊 Resultados

| Modelo | MAE | RMSE | R² |
|--------|-----|------|----|
| Regressão Linear | 8.38 | 10.45 | -0.01 |
| Random Forest (Otimizado) | 8.37 | 10.44 | -0.01 |
| XGBoost | 8.72 | 10.86 | -0.09 |

### Análise dos Resultados

Os resultados indicam **desempenho preditivo baixo** em todos os modelos:

- **R² Negativo**: Os modelos performaram pior que uma baseline ingênua (média)
- **Erros Elevados**: MAE ~8.4 dB é alto considerando a faixa de 33-97 dB
- **Consistência**: Todos os modelos apresentaram resultados similares

### Importância das Features (XGBoost)

As features mais importantes identificadas foram:
1. **traffic_density**: Densidade de tráfego
2. **hour**: Hora do dia
3. **longitude**: Coordenada geográfica
4. **latitude**: Coordenada geográfica
5. **vehicle_count**: Contagem de veículos

## 🔍 Reflexão Crítica

### Interpretação dos Resultados

Nenhum dos modelos conseguiu realizar previsões precisas. O R² negativo indica que os modelos foram piores que simplesmente usar a média como previsão.

### Limitações Identificadas

**Principal Limitação - Qualidade das Features:**
- As features disponíveis não contêm informação suficiente para prever ruído com precisão
- Correlações lineares muito fracas entre features e target
- Faltam variáveis cruciais como:
  - Tipo de pavimento
  - Barreiras acústicas
  - Velocidade do vento em nível do solo
  - Contagem de veículos em tempo real
  - Tipo de via (residencial, comercial, etc.)

### Insights Valiosos

O resultado negativo é, em si, um insight importante: modelar ruído urbano requer dados mais ricos e específicos do que os disponíveis no dataset atual.

## 🔮 Sugestões de Melhoria Futura

1. **Enriquecimento do Dataset**
   - Coletar dados sobre barreiras acústicas
   - Incluir tipo de pavimento e características das vias
   - Adicionar dados meteorológicos mais detalhados

2. **Engenharia de Features Avançada**
   - Criar variáveis de interação (ex: traffic_density × hour)
   - Features baseadas em geolocalização
   - Agregações espaciais e temporais

3. **Modelos Espaço-Temporais**
   - Redes Neurais Convolucionais (CNNs)
   - Redes de Grafos (GNNs)
   - Modelos de séries temporais (LSTM, Prophet)

4. **Análise de Segmentação**
   - Modelar separadamente por tipo de área (residencial, comercial, industrial)
   - Considerar padrões específicos por região geográfica

## 📝 Observações

- Dataset limpo sem valores faltantes ou duplicatas
- 2.000 medições de 50 sensores diferentes
- Faixa de ruído: 33.2 - 97.4 dB
- Análise exploratória revelou padrões temporais fracos
- Correlações entre features e target são muito baixas

## 👨‍💻 Autor

Projeto desenvolvido por Lucas para fins de estudo e aprendizado.

## 📚 Fonte dos Dados

- **Kaggle**: [Urban Noise Levels Dataset](https://www.kaggle.com/datasets/khushikyad001/urban-noise-levels)
- **Zenodo**: [Noise levels due to commercial and leisure activities](https://zenodo.org/records/15186086)

## 📄 Licença

Este projeto é de código aberto e está disponível para fins educacionais.

---

**Nota**: Este projeto demonstra a importância da qualidade e riqueza dos dados em problemas de machine learning. Resultados negativos também são aprendizados valiosos.
