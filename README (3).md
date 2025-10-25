# Aplicação Prática de Python na Ciência dos Dados
**Autor:** Doriel Pires Santana  
**Cidade:** Redenção  
**Data:** 25/10/2025  
**Disciplina:** Ciência dos Dados para Tomadas de Decisões  
**Professor:** Sergio Ricardo Pereira de Mattos

> Entrega com **códigos** e **saídas** usando o dataset `default_of_credit_card_clients`.

---


## 1) Quantas linhas e colunas há no conjunto de dados?

**Código:**
```python
import pandas as pd
df = pd.read_csv("default_of_credit_card_clients__courseware_version_1_21_19.csv", sep=";")
df.shape
```

**Saída:**
```
(30000, 25)
```

---


## 2) Tipos de variáveis (numéricas e categóricas)

**Código:**
```python
dtypes = df.dtypes
obj_cols = df.select_dtypes(include="object").columns.tolist()

# inferência simples de categóricas por baixa cardinalidade (≤ 10 valores únicos)
inferred_cat = []
for c in df.columns:
    if df[c].nunique(dropna=False) <= 10 and c not in obj_cols:
        inferred_cat.append(c)

candidate_cat_cols = list(dict.fromkeys(obj_cols + inferred_cat))
numeric_for_describe = [c for c in df.columns if c not in candidate_cat_cols]

dtypes, obj_cols, inferred_cat, numeric_for_describe
```

**Saída:**
```
Tipos (dtypes):
ID                            object
LIMIT_BAL                      int64
SEX                            int64
EDUCATION                      int64
MARRIAGE                       int64
AGE                            int64
PAY_1                         object
PAY_2                          int64
PAY_3                          int64
PAY_4                          int64
PAY_5                          int64
PAY_6                          int64
BILL_AMT1                      int64
BILL_AMT2                      int64
BILL_AMT3                      int64
BILL_AMT4                      int64
BILL_AMT5                      int64
BILL_AMT6                      int64
PAY_AMT1                       int64
PAY_AMT2                       int64
PAY_AMT3                       int64
PAY_AMT4                       int64
PAY_AMT5                       int64
PAY_AMT6                       int64
default payment next month     int64

Categóricas (object):
["ID", "PAY_1"]

Categóricas inferidas (baixa cardinalidade):
["SEX", "EDUCATION", "MARRIAGE", "PAY_5", "PAY_6", "default payment next month"]

Colunas numéricas para resumo estatístico:
["LIMIT_BAL", "AGE", "PAY_2", "PAY_3", "PAY_4", "BILL_AMT1", "BILL_AMT2", "BILL_AMT3", "BILL_AMT4", "BILL_AMT5", "BILL_AMT6", "PAY_AMT1", "PAY_AMT2", "PAY_AMT3", "PAY_AMT4", "PAY_AMT5", "PAY_AMT6"]
```

---


## 3) Resumo estatístico das variáveis numéricas

**Código:**
```python
df[numeric_for_describe].apply(pd.to_numeric, errors="coerce").describe().T
```

**Saída:**
```
             count           mean            std       min       25%       50%        75%        max
LIMIT_BAL  30000.0  165760.989333  130158.590432       0.0  50000.00  140000.0  240000.00  1000000.0
AGE        30000.0      35.108800       9.851592       0.0     28.00      34.0      41.00       79.0
PAY_2      30000.0      -0.132867       1.191215      -2.0     -1.00       0.0       0.00        8.0
PAY_3      30000.0      -0.164333       1.191096      -2.0     -1.00       0.0       0.00        8.0
PAY_4      30000.0      -0.219300       1.162348      -2.0     -1.00       0.0       0.00        8.0
BILL_AMT1  30000.0   50646.744233   73376.695080 -165580.0   3234.00   21644.5   66148.50   964511.0
BILL_AMT2  30000.0   48624.349167   70893.963498  -69777.0   2682.00   20597.0   62999.75   983931.0
BILL_AMT3  30000.0   46497.357100   69102.510012 -157264.0   2403.00   19752.5   59526.75  1664089.0
BILL_AMT4  30000.0   42791.362167   64090.316188 -170000.0   2034.00   18759.5   53572.25   891586.0
BILL_AMT5  30000.0   39884.398167   60606.644833  -81334.0   1534.00   17835.5   49804.00   927171.0
BILL_AMT6  30000.0   38480.350933   59406.836932 -339603.0   1080.00   16643.0   48863.50   961664.0
PAY_AMT1   30000.0    5613.321500   16539.094312       0.0    836.00    2084.5    5000.00   873552.0
PAY_AMT2   30000.0    5855.410300   22992.563836       0.0    721.75    2000.0    5000.00  1684259.0
PAY_AMT3   30000.0    5174.387967   17565.538305       0.0    371.00    1776.0    4500.00   896040.0
PAY_AMT4   30000.0    4776.089733   15532.893047       0.0    223.00    1500.0    4000.00   621000.0
PAY_AMT5   30000.0    4754.749200   15239.070708       0.0    170.75    1500.0    4000.00   426529.0
PAY_AMT6   30000.0    5164.223267   17712.664703       0.0      9.00    1500.0    4000.00   528666.0
```

---


## 4) Frequência das variáveis categóricas

**Código:**
```python
for c in candidate_cat_cols:
    print(f"Frequência de {c}:")
    print(df[c].value_counts(dropna=False).head(15))
    print()
```

**Saída:**
```
Frequência de 'ID':
ad23fe5c-7b09    2
1fb3e3e6-a68d    2
89f8f447-fca8    2
7c9b7473-cc2f    2
90330d02-82d9    2
2a793ecf-05c6    2
75938fec-e5ec    2
7be61027-a493    2
a3a5c0fc-fdd6    2
b44b81b2-7789    2
998fa9b2-b341    2
69566a6b-6156    2
4e2380e6-a8cf    2
b87bf8f3-d704    2
4f95b36b-ab10    2

Frequência de 'PAY_1':
0                13402
-1                5047
1                 3261
Not available     3021
-2                2476
2                 2378
3                  292
4                   63
5                   23
8                   17
6                   11
7                    9

Frequência de 'SEX':
2    17910
1    11775
0      315

Frequência de 'EDUCATION':
2    13884
1    10474
3     4867
0      329
5      275
4      122
6       49

Frequência de 'MARRIAGE':
2    15810
1    13503
0      369
3      318

Frequência de 'PAY_5':
 0    17069
-1     5488
-2     4503
 2     2603
 3      177
 4       81
 7       57
 5       17
 6        4
 8        1

Frequência de 'PAY_6':
 0    16408
-1     5686
-2     4849
 2     2749
 3      181
 4       48
 7       45
 6       19
 5       13
 8        2

Frequência de 'default payment next month':
0    23438
1     6562
```

---


## 5) Valores faltantes por coluna

**Código:**
```python
df.isnull().sum()
```

**Saída:**
```
ID                            0
LIMIT_BAL                     0
SEX                           0
EDUCATION                     0
MARRIAGE                      0
AGE                           0
PAY_1                         0
PAY_2                         0
PAY_3                         0
PAY_4                         0
PAY_5                         0
PAY_6                         0
BILL_AMT1                     0
BILL_AMT2                     0
BILL_AMT3                     0
BILL_AMT4                     0
BILL_AMT5                     0
BILL_AMT6                     0
PAY_AMT1                      0
PAY_AMT2                      0
PAY_AMT3                      0
PAY_AMT4                      0
PAY_AMT5                      0
PAY_AMT6                      0
default payment next month    0
```
