# Análise de Inadimplência em Cartão de Crédito

## Sobre o Projeto

Análise exploratória de dados de 29.965 clientes de um banco taiwanês para identificar 
padrões de inadimplência e criar um sistema de pontuação de risco. O projeto investiga 
quais características do cliente (idade, escolaridade, histórico de pagamento, limite) 
estão mais associadas ao calote.

## Dataset

- **Fonte:** [Default of Credit Card Clients - Kaggle (UCI)](https://www.kaggle.com/datasets/uciml/default-of-credit-card-clients-dataset)
- **Tamanho:** 29.965 clientes após remoção de duplicatas
- **Período:** Abril a Setembro de 2005
- **Taxa de inadimplência:** 22% (6.630 clientes)

> O arquivo CSV não está incluído neste repositório. 
> Para reproduzir, baixe o dataset do Kaggle.

## Ferramentas Utilizadas

- Python 3
- Pandas
- Matplotlib e Seaborn
- Scikit-learn (StandardScaler)
- Jupyter Notebook

## Etapas do Projeto

### 1. Exploração Inicial
- 30.000 clientes, 25 variáveis originais
- 22% de inadimplência — dataset mais balanceado que problemas de fraude
- Variáveis incluem dados demográficos, limite de crédito e histórico de pagamento

### 2. Limpeza e Tratamento
- Remoção de 35 duplicatas e da coluna ID
- Renomeação de colunas para maior clareza (PAY_0 → PAY_1, coluna alvo → default)

### 3. Análise Exploratória (EDA)
- **Idade:** maior volume de inadimplência entre 25-35 anos
- **Escolaridade:** ensino médio tem proporcionalmente mais calote que pós-graduação
- **Limite:** inadimplentes tendem a ter limites mais baixos
- **Histórico de pagamento:** indicador mais forte — clientes com 2+ meses de atraso têm probabilidade de calote acima de 50%

### 4. Sistema de Pontuação de Risco

Criação de 5 indicadores baseados nos padrões encontrados:

| Flag | Regra | Lógica |
|------|-------|--------|
| flag_atraso_grave | PAY_1 >= 2 | Atraso de 2+ meses no pagamento recente |
| flag_uso_alto | BILL_AMT1/LIMIT_BAL >= 0.8 | Usando 80%+ do limite do cartão |
| flag_pagou_pouco | PAY_AMT1 < BILL_AMT1 × 0.1 | Pagou menos de 10% da fatura |
| flag_multiplos_atrasos | 2+ atrasos em 3 meses | Padrão recorrente de atraso |
| flag_limite_baixo | LIMIT_BAL < 140.000 | Limite abaixo da mediana |

O **risk_score** é a soma das 5 flags (0 a 5).

## Principais Resultados

| Métrica | Valor |
|---------|-------|
| Risk score médio — bons pagadores | 1,36 |
| Risk score médio — inadimplentes | 2,26 |
| Inadimplentes detectados (score >= 3) | 47,2% |
| Taxa de alarme falso | 20% |

## Conclusões e Limitações

- O histórico de pagamento recente é de longe o melhor preditor de inadimplência
- O sistema baseado em regras captura 47% dos inadimplentes, mas com 20% de alarmes falsos
- **Limitação reconhecida:** a taxa de alarme falso é alta para uso em produção. Modelos de machine learning (regressão logística, random forest) seriam o próximo passo para melhorar a separação entre bom e mau pagador
- A diferença entre este projeto e o de fraude mostra que **cada problema de negócio tem sua complexidade** — inadimplência é mais difícil de prever com regras simples

## Comparação com Projeto de Fraude

| Aspecto | Fraude | Inadimplência |
|---------|--------|---------------|
| Quem causa o prejuízo | Agente externo (fraudador) | O próprio cliente |
| Balanceamento | 0,17% fraude | 22% inadimplência |
| Separação do risk score | 11x (forte) | 1,7x (moderada) |
| Taxa de detecção | 70,6% | 47,2% |
| Alarmes falsos | 0,48% | 20% |

## Como Reproduzir

1. Baixe o dataset do [Kaggle](https://www.kaggle.com/datasets/uciml/default-of-credit-card-clients-dataset)
2. Coloque o arquivo `UCI_Credit_Card.csv` na mesma pasta do notebook
3. Abra o notebook no Jupyter
4. Execute todas as células em ordem (Kernel → Restart & Run All)

## Autor

Armando Munhoz — Estudante de Análise de Dados

[![GitHub](https://img.shields.io/badge/GitHub-MunhozAR-181717?logo=github)](https://github.com/MunhozAR)
