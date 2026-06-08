# TRABALHO FINAL: PROJETO "DADOS PARA O BEM COMUM"
**Detecção de Fraude em Transações Online para Proteção do Consumidor Digital**

**Autores (Grupo 3):**
* Alexandre Takiguchi
* Raul Fabiano
* Gabriel Marconatto
* Enzo B. S

---

## Resumo
O crescimento exponencial do comércio eletrônico trouxe consigo um aumento proporcional nas fraudes financeiras digitais, impactando negativamente a confiança do consumidor e a sustentabilidade de pequenos negócios. Este trabalho apresenta uma abordagem estruturada em Ciência e Engenharia de Dados para identificar padrões de comportamento fraudulento em transações online. Utilizando o framework CRISP-DM e um conjunto de dados público de transações do Kaggle com mais de 150 mil registros, implementamos rotinas de limpeza de dados, anonimização, normalização Minmax e codificação One-hot. A análise exploratória revelou que as transações fraudulentas representam 9,37% da base de dados e não mostram correlação linear forte com a idade do usuário ou com o valor da compra individual, sugerindo que comportamentos fraudulentos possuem padrões mais complexos. Este estudo visa servir como base para sistemas de prevenção ativa de fraudes e apoiar políticas de Ciência de Dados Responsável.

**Palavras-chave:** Detecção de Fraude, CRISP-DM, Engenharia de Dados, Ciência de Dados Responsável, Estatística Descritiva.

---

## 1. Introdução

### Contextualização e Justificativa Social
O ambiente de negócios digitais tornou-se o principal motor de consumo moderno. No entanto, a incidência de transações fraudulentas representa um obstáculo crítico. Fraudes de compras online não geram apenas prejuízos financeiros diretos às empresas (através do *chargeback*), mas também causam danos psicológicos e financeiros aos consumidores finais, cujas identidades e cartões são usurpados. A solução desse problema apoia o "Bem Comum" ao fortalecer a segurança do ecossistema econômico digital, protegendo indivíduos vulneráveis e reduzindo custos operacionais que seriam repassados aos consumidores.

### Definição do Tipo de Tarefa
Este projeto enquadra-se essencialmente em duas vertentes:
1. **Tarefa Descritiva:** Inicialmente aplicada para entender os perfis de consumo, a proporção de fraudes na base de dados, a distribuição etária e as plataformas tecnológicas mais visadas pelos fraudadores.
2. **Tarefa Preditiva (Proposta):** A estruturação de engenharia de dados realizada prepara a base para algoritmos de classificação binária supervisionada (onde a classe alvo é `is_fraud`), permitindo prever em tempo real se uma nova transação é legítima ou fraudulenta.

---

## 2. Metodologia

### Fonte de Dados e Ferramentas
* **Fonte dos Dados:** O conjunto de dados utilizado foi extraído da plataforma **Kaggle** (dataset *Fraud Detection* por Prerit Saxena), composto por 151.112 registros de transações contendo variáveis como ID do usuário, data de cadastro, data da compra, valor da transação, ID do dispositivo, navegador utilizado, canal de marketing (source), gênero, idade, endereço IP e marcador de fraude (`is_fraud`).
* **Ferramentas:** Linguagem de programação **Python 3.12** utilizando as bibliotecas **Pandas** para manipulação e limpeza de dados, **Matplotlib** e **Seaborn** para visualização gráfica, e **Scikit-learn** para processos de normalização (MinMaxScaler).

### Trajetória CRISP-DM
O projeto seguiu a metodologia padrão CRISP-DM:
1. **Entendimento do Negócio:** Mapeamento do impacto da fraude no comércio eletrônico.
2. **Entendimento dos Dados:** Exploração das estatísticas básicas e correlações no conjunto de dados original.
3. **Preparação dos Dados:** Limpeza de caracteres irrelevantes, hashing de identificadores sensíveis para privacidade, normalização e codificação categórica.
4. **Modelagem e Avaliação (Proposta):** Preparação do dataframe final para algoritmos de machine learning.

---

## 3. Análise Exploratória e Visualização Estatística

### Medidas de Tendência Central e Dispersão
As principais variáveis numéricas contínuas (`purchase_value` e `age`) foram submetidas à análise estatística descritiva:

| Métrica Estatística | Valor da Compra (`purchase_value`) | Idade Real (`age`) |
| :--- | :---: | :---: |
| **Média** | USD 36,94 | 33,14 anos |
| **Mediana** | USD 35,00 | 33,00 anos |
| **Moda** | USD 36,00 | 32,00 anos |
| **Amplitude** | USD 111,00 | 67,00 anos (mín: 18, máx: 85) |
| **Variância** | 335,46 | 74,27 |
| **Desvio Padrão** | USD 18,32 | 8,62 anos |

Para a idade, a proximidade entre média (33,14) e mediana (33,00) indica uma distribuição simétrica, conforme ilustrado no histograma de distribuição:
* **[figura_1_histograma_idade.png](imagens/figura_1_histograma_idade.png)** (Reflete a distribuição etária dos usuários na plataforma, concentrada entre 25 e 40 anos).

A proporção de transações marcadas como fraudulentas no dataset foi quantificada em **9,37%** (14.151 transações de fraude contra 136.961 legítimas), demonstrando um desbalanceamento clássico de dados de fraude comercial:
* **[figura_2_proporcao_fraude.png](imagens/figura_2_proporcao_fraude.png)** (Gráfico de barras ilustrando o desbalanceamento das classes `is_fraud`).

A análise tecnológica revelou que os navegadores **Chrome** e **IE** são os mais utilizados nas transações, enquanto **SEO** e **Direct** são as principais fontes de aquisição de tráfego:
* **[figura_3_uso_tecnologia.png](imagens/figura_3_uso_tecnologia.png)** (Distribuição volumétrica das plataformas de acesso).

### Análise de Outliers
Utilizando o critério estatístico de IQR (Amplitude Interquartil) nos Boxplots, identificou-se que não existem outliers na variável `age`, mas há uma presença expressiva de compras de valores elevados que se situam acima do limite superior calculado de **USD 90,00**:
* **[figura_4_boxplots_outliers.png](imagens/figura_4_boxplots_outliers.png)** (Identificação gráfica de valores atípicos em ambas as variáveis).

Embora existam compras acima de USD 90,00, a maior compra registrada foi de **USD 120,00**. O gráfico de dispersão revela que as fraudes (em vermelho) ocorrem uniformemente em qualquer faixa de idade e valor de transação, sem agrupamento visual isolado:
* **[figura_5_dispersao_idade_valor.png](imagens/figura_5_dispersao_idade_valor.png)** (Cruzamento bidimensional de idade e valor da compra classificado pelo marcador de fraude).

### Análise Bivariada e Multivariada (Correlação e Covariância)
A tabela a seguir apresenta os coeficientes de **Correlação de Pearson** e de **Covariância** para as principais métricas do negócio:

#### Matriz de Correlação (Pearson)
| Variável | `purchase_value` | `age` | `sex_encoded` | `is_fraud` |
| :--- | :---: | :---: | :---: | :---: |
| **`purchase_value`** | 1,000 | 0,002 | -0,002 | 0,001 |
| **`age`** | 0,002 | 1,000 | -0,003 | 0,007 |
| **`sex_encoded`** | -0,002 | -0,003 | 1,000 | 0,002 |
| **`is_fraud`** | 0,001 | 0,007 | 0,002 | 1,000 |

* **[figura_6_matriz_correlacao.png](imagens/figura_6_matriz_correlacao.png)** (Representação visual das correlações de Pearson).

#### Matriz de Covariância
| Variável | `purchase_value` | `age` | `sex_encoded` | `is_fraud` |
| :--- | :---: | :---: | :---: | :---: |
| **`purchase_value`** | 335,463 | 0,332 | -0,019 | 0,006 |
| **`age`** | 0,332 | 74,272 | -0,013 | 0,017 |
| **`sex_encoded`** | -0,019 | -0,013 | 0,250 | 0,000 |
| **`is_fraud`** | 0,006 | 0,017 | 0,000 | 0,085 |

* **[figura_7_matriz_covariancia.png](imagens/figura_7_matriz_covariancia.png)** (Visualização da dispersão conjunta de covariância das variáveis).

**Interpretação:** As matrizes comprovam que a fraude (`is_fraud`) possui correlação linear praticamente nula com o valor da compra (0,001) e com a idade (0,007). Isso demonstra que regras simples baseadas em limites de valor ou idade não são suficientes para capturar fraudadores, necessitando de análises de engenharia de dados mais complexas (como comportamento de cadastro rápido).

---

## 4. Engenharia de Dados

### Qualidade e Limpeza de Dados
* **Valores Ausentes:** O dataset foi verificado e não apresentou nenhum registro nulo original nas 11 colunas, eliminando a necessidade de imputação de média/mediana.
* **Valores Inconsistentes:** Foi desenvolvida uma rotina para garantir que caracteres não alfanuméricos fossem purificados em colunas cruciais como `device_id`, `source`, `browser`, `sex` e `user_id`. Corrigiu-se um bug do projeto original que impedia a gravação dessas alterações diretamente no DataFrame.
* **Duplicidade:** Não foram identificadas linhas completamente duplicadas na base de dados. Porém, foram encontradas **8.056 compras suspeitas** de bots realizadas simultaneamente no mesmo instante e no mesmo dispositivo, o que servirá como forte feature para futuros modelos preditivos.

### Transformação de Dados e Anonimização
* **Anonimização (Privacidade):** Para assegurar a proteção de dados (em linha com os princípios da LGPD), aplicamos um algoritmo de hash criptográfico **SHA-256 encurtado para 8 caracteres** nas colunas `ip_address` e `device_id`. Isso protege a identidade real dos usuários e de seus dispositivos, enquanto mantém a capacidade técnica de identificar transações repetidas originadas de um mesmo IP ou dispositivo. A coluna `sex` foi mapeada em inteiros (`sex_encoded`) e a coluna `age` foi mascarada através de uma transformação linear multiplicativa para fins de exibição acadêmica.
* **Normalização:** Aplicou-se a **Normalização Minmax** nas variáveis contínuas `purchase_value` e `age`, redimensionando seus valores para uma escala entre `[0, 1]`. Este passo é fundamental para garantir que modelos preditivos (como Regressão Logística, KNN ou Redes Neurais) não priorizem uma coluna apenas por sua magnitude numérica padrão.
* **Codificação Categórica (One-hot encoding):** Navegadores (`browser`) e fontes de aquisição de tráfego (`source`) foram transformados em variáveis binárias *dummies* via codificação One-hot, viabilizando o uso dessas características em algoritmos estatísticos e preditivos digitais.

---

## 5. Conclusão e Ciência de Dados Responsável

### Interpretação dos Resultados
Os resultados sugerem que a ocorrência de fraudes eletrônicas não obedece a um perfil demográfico (idade, gênero) ou econômico (valor da compra) simples e linear. Em vez disso, a atividade fraudulenta é de natureza comportamental e sistêmica (como evidenciado pelas 8.056 transações suspeitas originadas de mesmos dispositivos simultaneamente).

### Ciência de Dados Responsável (Ética e Vieses)
* **Justiça (Fairness) e Vieses:** Evitou-se o uso direto de características protegidas como gênero (`sex`) e idade em análises discriminatórias. O projeto assegura que a tomada de decisão para classificar transações fraudulentas se baseie em indicadores de segurança técnica e comportamento transacional, mitigando o risco de bloquear clientes com base em perfis demográficos.
* **Privacidade dos Dados:** A aplicação de hash criptográfico unidirecional para IPs e IDs de dispositivos cumpre o princípio de minimização de dados e proteção à privacidade do cidadão.
* **Transparência:** O tratamento detalhado da engenharia de dados garante a reprodutibilidade técnica, permitindo auditorias e revisões sistemáticas contra possíveis decisões discriminatórias automatizadas.

---

## Referências
1. **ABNT NBR 6022:** Informação e documentação - Artigo em publicação periódica científica impressa - Apresentação.
2. **ABNT NBR 10520:** Informação e documentação - Citações em documentos - Apresentação.
3. **ABNT NBR 6023:** Informação e documentação - Referências - Elaboração.
4. **Pandas Development Team (2026):** *Pandas: Powerful Python data analysis toolkit*, versão 3.0.3. Disponível em: <https://pandas.pydata.org>.
5. **Matplotlib Developers (2026):** *Matplotlib: A 2D graphics environment*, versão 3.10.9. Disponível em: <https://matplotlib.org>.
6. **Scikit-learn Developers (2026):** *Machine Learning in Python*, versão 1.9.0. Disponível em: <https://scikit-learn.org>.
7. **Kaggle Datasets:** Saxena, Prerit. *Fraud Detection Dataset*. Disponível em: <https://www.kaggle.com/datasets/preritsaxena/fraud-detection>.
