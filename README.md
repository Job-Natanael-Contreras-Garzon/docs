# docs
# Ejercicio Práctico: Estimación de \(\displaystyle \int_{0}^{1} x^2\,dx\) con Monte Carlo

**Valor real**:  
\[
I = \int_{0}^{1} x^2\,dx = \frac{1}{3} \approx 0{,}3333
\]

---

## 🌟 Pasos del Método (cada paso en un “cuadro”)

```
+------------------------------+   +------------------------------+   +------------------------------+   +------------------------------+
|  1. Definir el problema     |   |  2. Modelo probabilístico    |   |  3. Generar números          |   |  4. Simular                  |
|                              |   |  • dominio: [0,1]×[0,1]      |   |     aleatorios (x,y) uniform |   |     experimentos: cada punto |
|  ∫₀¹ x² dx                   |   |  • condición: y ≤ x²         |   |     en el cuadrado           |   |     comprueba y ≤ x²         |
+------------------------------+   +------------------------------+   +------------------------------+   +------------------------------+

+------------------------------+   +------------------------------+   +------------------------------+
|  5. Evaluar                  |   |  6. Calcular estimación      |   |  7. Analizar error           |
|  • Contar M = puntos dentro  |   |  Ĩ = M / N                   |   |  • Comparar Ĩ vs. 1/3        |
|    de la curva (y ≤ x²)      |   |  • Área del cuadrado = 1     |   |  • Ajustar N según precisión |
+------------------------------+   +------------------------------+   +------------------------------+
```

---

## 📋 Desarrollo Detallado

### 1. Definir el problema  
Queremos estimar  
\[
I = \int_{0}^{1} x^2 \,dx
\]  
sin hacer la integral analítica, usando un método probabilístico.

---

### 2. Modelo probabilístico  
- **Dominio geométrico**: cuadrado unitario \([0,1]\times[0,1]\) (área = 1).  
- **Condición de “éxito”**: un punto \((x,y)\) cae bajo la curva si  
  \[
    y \le x^2.
  \]

---

### 3. Generar números aleatorios  
- Elegimos \(N\) puntos \((x_i, y_i)\) con  
  \[
    x_i \sim U(0,1),\quad y_i \sim U(0,1).
  \]  
- **Ejemplo**: \(N = 500\).

---

### 4. Simular experimentos  
- Para cada \(i = 1,2,\dots,N\):  
  1. Generar \((x_i,y_i)\).  
  2. Comprobar si \(y_i \le x_i^2\).  
  3. Si cumple, incrementa contador \(M\).

---

### 5. Evaluar  
- Al final de las \(N\) simulaciones, \(M\) es el nº de puntos “dentro” de la región bajo la curva.

---

### 6. Calcular estimación  
\[
\hat I = \frac{M}{N}\times\text{área del cuadrado}
       = \frac{M}{N}\times 1
       = \frac{M}{N}.
\]  
- **Ejemplo numérico**:  
  - Supongamos \(N=500\) y obtuvimos \(M=168\).  
  - Entonces \(\hat I = 168/500 = 0{,}3360\).

---

### 7. Analizar error  
- **Error absoluto** = \(\bigl|\hat I - \tfrac13\bigr|\).  
- Con \(N=500\), suele rondar ±0,02–0,03.  
- Para mejorar la precisión, aumenta \(N\).

---

## 🖥️ Código de ejemplo en Python

```python
import random

def monte_carlo_integral_x2(N=500):
    M = 0
    for _ in range(N):
        x = random.random()
        y = random.random()
        if y <= x**2:
            M += 1
    return M / N

for N in [100, 500, 1000, 5000]:
    estimate = monte_carlo_integral_x2(N)
    print(f"N={N:>4} → Ĩ={estimate:.4f}  (error={abs(estimate - 1/3):.4f})")
```

**Salida esperada** (aproximada):

|    N | Ĩ (estimación) | Error  |
|-----:|---------------:|-------:|
|  100 |      0.3500    | 0.0167 |
|  500 |      0.3360    | 0.0027 |
| 1000 |      0.3345    | 0.0012 |
| 5000 |      0.3338    | 0.0005 |
