# Análise de Projetos de Investimento no Distrito Federal (ObrasGov)

## Sobre o Projeto

Este projeto realiza uma análise exploratória completa sobre os projetos de investimento público no Distrito Federal, utilizando dados abertos da plataforma ObrasGov.br. O objetivo é extrair, tratar, armazenar e visualizar os dados para identificar padrões, entender a distribuição de investimentos, avaliar o andamento das obras e descobrir insights relevantes sobre a aplicação de recursos públicos na região.

Este trabalho demonstra um fluxo completo de análise de dados, desde a coleta programática até a comunicação visual dos resultados, incluindo o tratamento de desafios comuns em dados do mundo real.

## Fonte dos Dados

Os dados foram extraídos da API pública do ObrasGov.br:

* **Endpoint Principal:** `https://api.obrasgov.gestao.gov.br/obrasgov/api/projeto-investimento`
* **Filtro Aplicado:** `uf=DF` (Distrito Federal)
* **Observação:** Durante a extração, foi necessário implementar lógicas para lidar com paginação e *Rate Limiting* (erro 429) impostos pela API.

## Tecnologias Utilizadas

* **Linguagem:** Python 3
* **Bibliotecas Principais:**
    * `requests`: Para extração de dados via API.
    * `pandas`: Para manipulação e tratamento dos dados.
    * `sqlalchemy`: Para interação com o banco de dados.
    * `sqlite3`: Banco de dados relacional leve utilizado.
    * `matplotlib` & `seaborn`: Para visualização de dados.
    * `squarify`: Para a visualização Treemap.
    * `ast`: Para conversão segura de strings estruturadas.
* **Ambiente:** Jupyter Notebook

## Estrutura do Projeto

O repositório está organizado da seguinte forma:

```text
ANALISE-DADOS-OBRASGOV/
│
├── Relatório de Análise.pdf
├── README.md               # Este arquivo
├── requirements.txt        # Dependências do Python
│
├── data/
│   ├── raw/                # Dados brutos extraídos (csv, json)
│   └── processed/          # Dados limpos para análise
│
├── notebooks/
│   └── analise_obrasgov_df.ipynb  # Notebook com o código
│
└── db/
    └── obras_df.db         # Banco de dados SQLite
```

##  Metodologia e Tratamento dos Dados

O processo seguiu as seguintes etapas:

1.  **Extração de Dados:** Coleta dos dados da API, tratando paginação e *rate limits*. Salvamento dos dados brutos.
2.  **Carga e Limpeza Fundamental:** Carregamento dos dados brutos e remoção imediata de registros duplicados (`id_unico`).
3.  **Diagnóstico Pós-Deduplicação:** Análise detalhada da estrutura, tipos de dados, valores ausentes e estatísticas descritivas dos dados únicos.
4.  **Engenharia de Features (Flattening):** Expansão das colunas com strings de listas/dicionários (`executores`, `eixos`, `fontes_de_recurso`, etc.) em novas colunas "achatadas" (`executor_nome`, `eixo_descricao`, `origem_recurso`).
5.  **Cálculo de Métricas:** Criação da coluna `valor_total_previsto` pela soma das `fontes_de_recurso`. Validação da necessidade desta soma.
6.  **Tipagem Adequada:** Conversão rigorosa das colunas para os tipos corretos (`datetime64[ns]`, `float64`, `boolean`, `string`, `category`). Validação da escolha entre `string` e `category` com base na cardinalidade.
7.  **Tratamento de Anomalias:** Identificação, documentação e (quando apropriado) filtragem de dados problemáticos (ex: nomes de projetos como códigos, valores simbólicos de R$ 0.01).
8.  **Otimização e Limpeza Final:** Remoção de colunas redundantes e otimização final dos tipos de dados no DataFrame master (`df_tratado`).
9.  **Armazenamento:** Salvamento do `df_tratado` no banco de dados SQLite (`projetos_investimento`).
10. **Preparação para Análise:** Carregamento dos dados do SQLite e criação do `df_analise` (versão focada para visualização), com filtragem final de valores simbólicos e salvamento em `data/processed`.

## Análises Realizadas

Utilizando o `df_analise` (557 projetos)  foram realizadas as seguintes análises, na ordem apresentada no notebook:

1.  **Distribuição do Valor dos Projetos:**

2.  **Evolução Temporal do Cadastro de Projetos e Investimentos:**

3.  **Distribuição por Setor (Eixo Temático):**

4.  **Principais Órgãos Executores por Número de Projetos:**

5.  **Distribuição por Situação:**
