#  Segmentação de Clientes e Análise de Associação em Shopping Centers

##  Visão Geral do Projeto
Este projeto realiza uma análise detalhada dos dados de clientes de um shopping center para identificar segmentos distintos de clientes e descobrir regras de associação entre atributos dos clientes. O objetivo é fornecer insights acionáveis para estratégias de marketing e gestão de relacionamento com o cliente.

A análise emprega três técnicas principais:
1. **K-Means Clustering:** Para particionar clientes em grupos distintos com base em Idade, Renda Anual e Pontuação de Gastos.
2. **Clustering Hierárquico:** Para validar os resultados do K-Means e visualizar a hierarquia de agrupamento.
3. **Algoritmo Apriori:** Para encontrar regras de associação entre perfis de clientes (Gênero, Faixa Etária, Faixa de Renda, Faixa de Score) e seus clusters atribuídos.

##  Dataset
O dataset `Mall_Customers.csv` contém informações sobre 200 clientes com os seguintes atributos:
* **CustomerID:** Identificador único.
* **Gender:** Masculino ou Feminino.
* **Age:** Idade do cliente.
* **Annual Income (k$):** Renda anual em milhares de dólares.
* **Spending Score (1-100):** Pontuação atribuída pelo shopping com base no comportamento e natureza de gastos do cliente.

##  Metodologia

### 1. Pré-processamento de Dados
* Renomeação de colunas para facilitar o acesso.
* **Normalização:** Escalonamento de features numéricas (Idade, Renda, Score) usando `MinMaxScaler` para clustering.
* **Discretização:** Conversão de features numéricas em grupos categóricos (ex: Idade → Jovem, Adulto, Senior) para Mineração de Regras de Associação.

### 2. Análise de Clustering
* **Método do Cotovelo & Coeficiente de Silhueta:** Utilizados para determinar o número ótimo de clusters. Ambos os métodos sugeriram **K=5** como o número ideal.
* **K-Means:** Executado com K=5 para segmentar os clientes.
* **Clustering Hierárquico:** Realizado usando o método de Ward. O Dendrograma confirmou que 5 clusters fornecem uma separação distinta.

### 3. Mineração de Regras de Associação
* **Algoritmo Apriori:** Aplicado ao dataset discretizado combinado com rótulos de cluster.
* **Parâmetros:** Suporte Mínimo = 0.10, Confiança Mínima = 0.60.
* **Objetivo:** Encontrar regras fortes ligando dados demográficos dos clientes ao seu comportamento de gastos e pertencimento ao cluster.

##  Principais Insights & Resultados

### Perfis dos Clusters (K-Means)
Com base na análise, 5 perfis distintos de clientes foram identificados:

| Cluster | Nome do Perfil | Idade (Média) | Renda (Média k$) | Score (Médio) | Descrição |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **0** | **Jovens Gastadores** | ~25 | ~26 | ~79 | Clientes jovens com baixa renda mas pontuação de gastos muito alta. |
| **1** | **Ricos Conservadores** | ~41 | ~88 | ~17 | Clientes de meia-idade com alta renda mas baixa pontuação de gastos. |
| **2** | **Ricos Gastadores** | ~33 | ~87 | ~82 | Clientes Jovens/Adultos com alta renda e alta pontuação de gastos. (Grupo Alvo) |
| **3** | **Clientes Médios** | ~43 | ~55 | ~50 | O maior grupo. Adultos com renda média e pontuação de gastos média. |
| **4** | **Clientes Frugais** | ~45 | ~26 | ~21 | Adultos com baixa renda e baixa pontuação de gastos. |

### Regras de Associação (Apriori)
Associações fortes foram encontradas, revelando padrões como:
* **Cluster 1 (Ricos Conservadores):** Fortemente associado com **Baixa Pontuação de Gastos** e **Renda Média/Alta**.
* **Cluster 2 (Ricos Gastadores):** Fortemente associado com **Alta Pontuação de Gastos** e faixa etária **Adulta**.
* **Cluster 0:** Associado com **Alto Score** e **Baixa Renda**.
* **Cluster 3:** Associado com **Score Médio** e **Renda Média**.

##  Visualizações
O Jupyter Notebook inclui as seguintes visualizações para apoiar a análise:
1. **Curva do Cotovelo:** Mostra a redução na inércia conforme K aumenta, indicando o "cotovelo" em K=5.
2. **Gráfico de Silhueta:** Visualiza a distância de separação entre os clusters resultantes.
3. **Gráficos de Dispersão dos Clusters:**
   * *Renda Anual vs. Pontuação de Gastos:* Mostra claramente os 5 clusters separados por renda e comportamento de gastos.
   * *Idade vs. Pontuação de Gastos:* Mostra distribuição entre faixas etárias.
4. **Dendrograma:** Um diagrama em árvore mostrando as relações taxonômicas dos clusters, validando a escolha de 5 clusters.

##  Como Executar
```
# 1. Criar ambiente virtual
python -m venv venv
source venv/bin/activate  # Linux/Mac
# ou
venv\Scripts\activate  # Windows
```
```
# 2. Instalar TUDO com um comando
pip install -r requirements.txt
```
```
# 3. Executar o notebook
jupyter notebook seg_clientes_shopping_centers.ipynb
```

##  Estrutura do Projeto
* `seg_clientes_shopping_centers.ipynb`: Notebook principal de análise.
* `Mall_Customers.csv`: Dataset bruto.
* `perfil_clusters.csv`: Resumo exportado dos perfis dos clusters.
* `regras_associacao_apriori.csv`: Regras de associação exportadas.
* `metricas_projeto.csv`: Métricas de desempenho dos modelos.

##  Métricas de Avaliação

| Métrica | Valor | Interpretação |
|---------|-------|---------------|
| **Inércia** | 3.5831 | Compactação dos clusters |
| **Coeficiente de Silhueta** | 0.5595 | Ótima separação entre clusters |
| **Índice Davies-Bouldin** | 0.5678 | Excelente diferenciação (quanto menor, melhor) |
| **Total de Regras Apriori** | 78 | Regras de associação descobertas |
| **Lift Máximo** | 5.466 | Força da associação mais forte |

##  Recomendações Estratégicas

### Por Segmento:

**Cluster 0 - Jovens Gastadores (11%)**
* Oferecer opções de parcelamento facilitado
* Implementar programa de pontos e cashback
* Promoções frequentes e limitadas

**Cluster 1 - Ricos Conservadores (17.5%)**
* Produtos premium com garantia estendida
* Comunicação sobre durabilidade e valor de longo prazo
* Serviços pós-venda diferenciados

**Cluster 2 - Ricos Gastadores (19.5%)**  *Grupo Prioritário*
* Lançamentos exclusivos e pré-vendas VIP
* Atendimento personalizado premium
* Programa de fidelidade com benefícios exclusivos

**Cluster 3 - Clientes Médios (40.5%)**
* Mix balanceado de produtos e preços
* Promoções sazonais regulares
* Cross-selling baseado em padrões

**Cluster 4 - Clientes Frugais (11.5%)**
* Campanhas agressivas de cupons e descontos
* Produtos de entrada e promoções "combos"
* Comunicação focada em economia

##  Principais Descobertas

###  Insights Críticos:
1. **Renda não determina gastos:** Correlação de apenas 0.01 entre renda e pontuação de gastos.
2. **Idade influencia comportamento:** Correlação negativa de -0.33 entre idade e gastos (jovens gastam mais).
3. **Segmento Premium:** Apenas 19.5% dos clientes, mas representa o maior potencial de receita.
4. **Base estável:** 40.5% são clientes médios que garantem receita recorrente.

###  Top 5 Regras de Associação:

1. **Cluster 1 ⟷ Score Baixo + Renda Média**
   - Confiança: 62.9% → 95.7% | Lift: 5.47

2. **Adulto + Cluster 2 → Score Alto + Renda Média**
   - Confiança: 67.7% → 80.8% | Lift: 5.21

3. **Adulto + Score Alto + Renda Média → Cluster 2**
   - Confiança: 100% | Lift: 5.13

4. **Score Baixo + Adulto → Cluster 1**
   - Confiança: 69% | Lift: 3.94

5. **Score Baixo ⟷ Cluster 1**
   - Confiança: 64% → 91.4% | Lift: 3.66

##  Conclusão
Este projeto demonstrou com sucesso a aplicação de técnicas de mineração de dados para segmentação de clientes e descoberta de padrões comportamentais. Os resultados fornecem uma base sólida para estratégias de marketing personalizadas e otimização de recursos, permitindo:

- Identificação precisa de 5 segmentos distintos de clientes  
- Descoberta de 78 regras de associação com alta confiança  
- Insights acionáveis para personalização de campanhas  
- Compreensão profunda que renda não determina comportamento de compra  
- Priorização estratégica do segmento Premium (maior ROI)
