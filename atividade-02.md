# Parte 2 — Exploração de características financeiras (Python)

**Autor:** Doriel Pires Santana  
  
**Professor:** Sergio Ricardo Pereira de Mattos

---

## Exercício 1 — Listas de características financeiras
**Código:**
```python
bill_cols = ['BILL_AMT1', 'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6']
pay_cols = ['PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6']
bill_cols, pay_cols
```
**Saída:**
```
['BILL_AMT1', 'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6'], ['PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6']
```

---

## Exercício 2 — `.describe()` das características de valor da fatura (BILL_AMT1–6)
**Código:**
```python
df[bill_cols].apply(pd.to_numeric, errors='coerce').describe().T
```
**Saída:**
```
             count          mean           std       min     25%      50%       75%        max
BILL_AMT1  30000.0  50646.744233  73376.695080 -165580.0  3234.0  21644.5  66148.50   964511.0
BILL_AMT2  30000.0  48624.349167  70893.963498  -69777.0  2682.0  20597.0  62999.75   983931.0
BILL_AMT3  30000.0  46497.357100  69102.510012 -157264.0  2403.0  19752.5  59526.75  1664089.0
BILL_AMT4  30000.0  42791.362167  64090.316188 -170000.0  2034.0  18759.5  53572.25   891586.0
BILL_AMT5  30000.0  39884.398167  60606.644833  -81334.0  1534.0  17835.5  49804.00   927171.0
BILL_AMT6  30000.0  38480.350933  59406.836932 -339603.0  1080.0  16643.0  48863.50   961664.0
```

---

## Exercício 3 — Histogramas (20 bins) das características de valor da fatura
**Código:**
```python
for col in bill_cols:
    df[col] = pd.to_numeric(df[col], errors='coerce')
    df[col].dropna().hist(bins=20)
```
**Imagens:**
![](hist_BILL_AMT1.png)
![](hist_BILL_AMT2.png)
![](hist_BILL_AMT3.png)
![](hist_BILL_AMT4.png)
![](hist_BILL_AMT5.png)
![](hist_BILL_AMT6.png)

---

## Exercício 4 — `.describe()` das características de valor do pagamento (PAY_AMT1–6)
**Código:**
```python
df[pay_cols].apply(pd.to_numeric, errors='coerce').describe().T
```
**Saída:**
```
{desc_pay.to_string()}
```

---

## Exercício 5 — Histogramas (20 bins) das características de pagamento com rótulos rotacionados
**Código:**
```python
for col in pay_cols:
    df[col] = pd.to_numeric(df[col], errors='coerce')
    ax = df[col].dropna().hist(bins=20)
    ax.set_xlabel(col, rotation=45)
```
**Imagens:**
![](hist_PAY_AMT1.png)
![](hist_PAY_AMT2.png)
![](hist_PAY_AMT3.png)
![](hist_PAY_AMT4.png)
![](hist_PAY_AMT5.png)
![](hist_PAY_AMT6.png)

---

## Exercício 6 — Contagem de pagamentos iguais a zero
**Código:**
```python
zeros_count = {col: int((pd.to_numeric(df[col], errors='coerce') == 0).sum()) for col in ['PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6']}
zeros_count
```
**Saída:**
```
{
  "PAY_AMT1": 5504,
  "PAY_AMT2": 5663,
  "PAY_AMT3": 6223,
  "PAY_AMT4": 6660,
  "PAY_AMT5": 6955,
  "PAY_AMT6": 7416
}
```

---

## Exercício 7 — Histogramas de log10 dos pagamentos > 0 (usando `.apply(np.log10)`)
**Código:**
```python
df_pay = df[['PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6']].apply(pd.to_numeric, errors='coerce')
mask_nonzero = (df_pay > 0)
df_log = df_pay.where(mask_nonzero).apply(np.log10)
for col in ['PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6']:
    df_log[col].dropna().hist(bins=20)
```
**Imagens:**
![](hist_log10_PAY_AMT1.png)
![](hist_log10_PAY_AMT2.png)
![](hist_log10_PAY_AMT3.png)
![](hist_log10_PAY_AMT4.png)
![](hist_log10_PAY_AMT5.png)
![](hist_log10_PAY_AMT6.png)
