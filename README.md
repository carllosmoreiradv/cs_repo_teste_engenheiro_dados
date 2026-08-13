# Teste Técnico – Engenheiro de Dados

Resolução do desafio técnico de Engenharia de Dados (SQL) proposto para a vaga, aplicado a um marketplace B2B fictício que conecta distribuidoras de alimentos a supermercados/mercearias.

**Autor:** Carlos Silvestre

## Estrutura do repositório

```
.
├── carlos_silvestre_teste.ipynb   # Notebook com as 4 queries e comentários de revisão
├── dados/                         # CSVs fornecidos no desafio
│   ├── buyers.csv
│   ├── order_items.csv
│   ├── orders.csv
│   ├── payments.csv
│   ├── products.csv
│   └── sellers.csv
└── README.md
```

## O que o notebook resolve

O time de negócio estava tomando decisões em cima de relatórios de vendas inconsistentes. O notebook resolve 4 problemas concretos com SQL:

1. **Desafio 1** – Faturamento bruto mensal (últimos 12 meses), considerando apenas pedidos `completed`/`delivered`, com quantidade de pedidos e ticket médio por mês.
2. **Desafio 2** – Ranking dos 10 sellers com maior crescimento de GMV, comparando duas janelas móveis de 90 dias (janela atual vs. janela imediatamente anterior), restrito a sellers com pelo menos 50 pedidos em ambas as janelas. A versão inicial comparava trimestres de calendário, mas o trimestre mais recente da base está incompleto (faltam dados de dezembro/2024) — isso inflava artificialmente uma queda de GMV. O ajuste para janelas de tamanho fixo (90 dias) corrige esse viés; o raciocínio completo está comentado no notebook.
3. **Desafio 3** – Pedidos (não cancelados) onde o desconto total dos itens ultrapassa 40% do valor bruto, indício de desconto abusivo para inflar volume.
4. **Desafio 4** – Produtos com mais de 1.000 unidades vendidas que nunca foram o item de maior valor unitário dentro de um pedido, usando `RANK()` (window function).

Cada query está comentada no próprio notebook, e cada desafio tem, logo abaixo do código, uma célula de markdown com a **revisão da solução** (o que está correto, o que vale ajustar e por quê), validada rodando as queries contra os dados reais.

## Como rodar

O notebook foi escrito originalmente no **Databricks** (por isso as células `%pip install` e `%restart_python`). Duas formas de rodar:

### Opção A – Google Colab (sugerido no enunciado)

1. Abra o notebook no Colab.
2. **Pule/ignore** a célula `%restart_python` — esse magic não existe no Colab (basta não executá-la; o `%pip install` sozinho já é suficiente).
3. Faça upload dos 6 arquivos de `dados/` para `/content/` no ambiente do Colab (ícone de pasta → upload), **ou** ajuste os `pd.read_csv('buyers.csv', ...)` no notebook para `pd.read_csv('/content/buyers.csv', ...)` (assim como no notebook-modelo fornecido no desafio).
4. Rode as células em ordem.

### Opção B – Databricks / Jupyter local

1. Coloque os 6 `.csv` da pasta `dados/` no mesmo diretório de trabalho do notebook (ou ajuste os caminhos em `pd.read_csv(...)`).
2. Rode `%pip install pandasql "sqlalchemy<2.0"` (e `%restart_python`, se estiver no Databricks).
3. Execute as células em ordem.

### Dependências

- `pandas`
- `pandasql` (usa SQLite por baixo dos panos para rodar SQL sobre os DataFrames)
- `sqlalchemy<2.0` (compatibilidade com o `pandasql`)

## Estrutura do banco de dados

| Tabela | Descrição |
|---|---|
| `orders` | Pedidos realizados |
| `order_items` | Itens de cada pedido |
| `products` | Catálogo de produtos |
| `sellers` | Distribuidoras vendedoras |
| `buyers` | Supermercados compradores |
| `payments` | Registro de pagamentos |