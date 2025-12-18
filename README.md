# Power BI – ETL de Vendas (Sample Superstore)

Projeto em Power BI focado em **ETL e preparação de dados de vendas** usando o dataset Sample Superstore.  
Aqui foram criadas colunas calculadas e classificações de pedidos para futuras análises de desempenho e lucratividade.

## 🎯 Objetivo do Projeto

Simular o trabalho de um analista de dados de Vendas, preparando uma base única para responder perguntas como:

- Como estão as margens por pedido?
- Quantos dias os pedidos levam para serem entregues?
- Quais pedidos têm ticket mais alto/baixo?

## 🗂️ Dataset

- Arquivo: `Sample-Superstore.csv` (Kaggle).
- Principais campos utilizados:
  - Datas: `Order Date`, `Ship Date`
  - Métricas: `Sales`, `Quantity`, `Discount`, `Profit`
  - Dimensões: Cliente, Cidade, Estado, Região, Categoria, Sub-Categoria, Product Name.

## 🔄 Processo de ETL no Power Query

Todas as transformações foram feitas no **Power Query Editor**:

1. **Importação e tratamento de datas**
   - Importação do CSV via Power BI Desktop.
   - Conversão de `Order Date` e `Ship Date` para **Data** usando **localidade en-US** (datas em formato MM/DD/YYYY).

2. **Tratamento de tipos numéricos**
   - Conversão de `Sales` e `Profit` para **Número Decimal** também com localidade en-US (ponto como separador decimal).

3. **Colunas calculadas criadas**

   - `AnoPedido`  
     - Fórmula (M): `Date.Year([Order Date])`  
     - Finalidade: facilitar análises por ano e séries temporais.

   - `DiasEntrega`  
     - Fórmula (M): `Duration.Days([Ship Date] - [Order Date])`  
     - Finalidade: medir o tempo entre pedido e envio, útil para KPIs logísticos.

   - `MargemPercent`  
     - Fórmula (M):  
       - `if [Sales] = 0 then null else [Profit] / [Sales]`  
     - Finalidade: calcular a **margem do pedido** em relação às vendas.

   - `TicketClass` (coluna condicional de classificação)  
     - Lógica em faixas (exemplo):  
       - Se `MargemPercent < 0,3` → `"Baixo"`  
       - Senão se `MargemPercent < 0,7` → `"Médio"`  
       - Senão → `"Alto"`  
     - Implementada via **Coluna Condicional** no Power Query.

4. **Ajustes finais**
   - Verificação e padronização de tipos (datas, números, textos).
   - Preparação para carregar no modelo e utilizar como base fato em relatórios.

## 🧾 Resultado

Após o ETL, a tabela principal de vendas passa a ter:

- Colunas adicionais:
  - `AnoPedido`
  - `DiasEntrega`
  - `MargemPercent`
  - `TicketClass` (Baixo/Médio/Alto)
- Datas e números corretamente interpretados pela localidade correta.
- Base pronta para dashboards de:
  - Vendas por ano / região / categoria.
  - Margem média por segmento.
  - Análise de tempo de entrega.

## 🛠️ Ferramentas Utilizadas

- **Power BI Desktop (Pro)**
- **Power Query Editor**
- **GitHub** para versionamento e publicação do `.pbix`.

## 🚀 Próximos Passos

- Criar um **modelo de dados** com tabela calendário (DimData) para análises por período.
- Montar um **dashboard de Vendas & Margem**:
  - Vendas x Lucro por Ano, Região e Categoria.
  - Distribuição de `TicketClass` (Baixo/Médio/Alto).
  - Análise de `DiasEntrega` por região/segmento.
- Adicionar prints do Power Query e do relatório final neste repositório.

## 📁 Como visualizar o arquivo

1. Baixe o arquivo `Superstore_ETL.pbix` deste repositório.
2. Abra com **Power BI Desktop**.
3. No menu **Transformar Dados**, acesse o Power Query para ver os passos de ETL.
