# Calentando motores: estadístcas de resumen

En esta parte vamos a trabajar con un set de datos pequeño correspondiente a estimaciones de rendimiento de soja (en tn/ha) de 25 lotes soja de una región. Los datos son:


```
4.12, 4.61, 4, 5.46, 4.7, 4.01, 4.79, 4.94, 4.85, 4.32, 5.41, 4.73, 4.13, 3.17, 5.17, 4.47, 4.49, 5.07, 4.99, 4.86, 5.05, 4.97, 4.54, 3.31, 4.87
```

## Leer los datos 

El siguiente código muestra como cargar los datos de la muestra en un objeto llamado `yields`.

### **R**

La manera más simple de crearlo es usando la función `c()`.


```r
yields <- c(
  4.12, 4.61, 4, 5.46, 4.7, 4.01, 4.79, 4.94, 4.85, 4.32, 5.41,
  4.73, 4.13, 3.17, 5.17, 4.47, 4.49, 5.07, 4.99, 4.86, 5.05,
  4.97, 4.54, 3.31, 4.87
)
yields
```

```
 [1] 4.12 4.61 4.00 5.46 4.70 4.01 4.79 4.94 4.85 4.32 5.41
[12] 4.73 4.13 3.17 5.17 4.47 4.49 5.07 4.99 4.86 5.05 4.97
[23] 4.54 3.31 4.87
```

### **Python**

Crearemos un objeto tipo `list` usando `[ ]`. Tambien podemos crear un `tuple` que es simiar pero no mutable.


```python
yields = [
  4.12, 4.61, 4, 5.46, 4.7, 4.01, 4.79, 4.94, 4.85, 4.32, 5.41,
  4.73, 4.13, 3.17, 5.17, 4.47, 4.49, 5.07, 4.99, 4.86, 5.05,
  4.97, 4.54, 3.31, 4.87
]
yields
```

```
[4.12, 4.61, 4, 5.46, 4.7, 4.01, 4.79, 4.94, 4.85, 4.32, 5.41, 4.73, 4.13, 3.17, 5.17, 4.47, 4.49, 5.07, 4.99, 4.86, 5.05, 4.97, 4.54, 3.31, 4.87]
```


## The sample mean 

Para averiguar el rendimiento promedio de la muestra tenemos que aplicar la siguiente función:

$$
\bar{y} = \dfrac{\sum y_i}{n}
$$

A continuación vamos a calcularlo _a mano_, paso por paso, y creando una función. Finalmente lo haremos usando las funciones incluidas en ambos lenguajes. 

### **R**

Calculando _a mano_


```r
# Total de observaciones
tot <- sum(yields)

# Numero de elementos en la muestra
n <- length(yields)

# Promedio
ybar <- tot/n
ybar
```

```
[1] 4.6012
```

Encapsulando todo en una función:


```r
# Definir la función `media`
media <- function(y) {
  tot <- sum(y)
  n <- length(y)
  tot/n
}

# Aplicar función
media(yields)
```

```
[1] 4.6012
```

Lo mismo obtenemos usando la función `mean()`


```r
mean(yields)
```

```
[1] 4.6012
```

### **Python**

Calculando _a mano_


```python
# Total de observaciones
tot = sum(yields)

# Numero de elementos en la muestra
n = len(yields)

# Promedio
ybar = tot/n
ybar
```

```
4.6012
```

Encapsulando todo en una función:


```python
# Definir la función `media`
def media(y):
  tot = sum(y)
  n = len(y)
  return(tot/n)
  

# Aplicar función
media(yields)
```

```
4.6012
```

Al contrario de **R** que ya incluye funciones estadísticas al inicio (paquete `stats`), en **Python** necesitamos importar modulos con `import`. Hay varias opciones: `statistics`, `numpy` y `scipy`. La más completa es el duo `numpy/scipy` que tiene funciones agrupadas en varios submodulos


```python
# Importar libreria numpy
import numpy as np

# Calcular media
np.mean(yields)
```

```
4.6012
```

## Standard deviation 

El desvió estándar de una muestra se define como:

$$
s = \sqrt{\dfrac{\sum (y_i - \bar{y})^2}{n-1}}
$$

A continuación se muestra como calcularlo _a mano_ y usando funciones incluidas en ambos lenguajes. Aunque el calculo _a mano_ se puede hacer en un solo paso, voy a mostrarlo por partes y creando una función. 

### **R**

Calculando _a mano_. Algunos datos ya los tenemos de antes `ybar` y `n`


```r
# Total de desvios al cuadrado
tot_sq <- sum((yields - ybar)^2)

# Desvio
s <- sqrt(tot_sq/(n-1))
s
```

```
[1] 0.5698707
```

Poniendo todo en una funcion


```r
# Definir funcion `desvio`
desvio <- function(y) {
  n <- length(y)
  ybar <- sum(y)/n
  sum((y - ybar)^2)/(n-1)
}

# Aplicar función 
desvio(x)
```

```
[1] 0.3247527
```

Usando la función `sd()`


```r
sd(yields)
```

```
[1] 0.5698707
```

### **Python**

Para calcular los desvios cuadráticos _a mano_ se requiere aplicar la función resta y potencia de elemento a elemento. Por defecto las funciones no están verctorizadas por lo que tenemos que iterar sobre los elementos de `yield`. Una opción es usando `for/loop` o `list-comprehension`.


```python
sq = [(y - ybar)**2 for y in yields]
tot_sq = sum(sq)
```

El código anterior aplica la función `(y - ybar)**2` a cada elemento `y` contenido en `yield` y devuelve una nueva lista. Luego esa lista es usada para calcular el total.

Como el cálculo del desvio requiere sacar la raiz cuadrada de la varianza, necesitamos importar la función `sqrt()` del módulo `math`


```python
# Importar sqrt
from math import sqrt

s = sqrt(tot_sq/(n-1))
s
```

```
0.5698707455789134
```

Creando una función:


```python
# Definiendo al funcion `desvio`
def desvio(y):
  from math import sqrt
  
  n = len(y)
  ybar = sum(y)/n
  sq = [(y - ybar)**2 for y in yields]
  tot_sq = sum(sq)
  
  return(sqrt(tot_sq/(n-1)))
  

# Aplicando la funcion
desvio(yields)
```

```
0.5698707455789134
```

Usando la función `std` del paquete `numpy` que ya importamos antes.


```python
np.std(yields)
```

```
0.5583570184031001
```

## Calculando varias estadísticas a la vez

Supongamos que queremos averiguar las siguientes estadísitcas descriptivas a partir de los datos de la muestra y presentar los resultados en una tabla: _media_, _desvio_, _varianza_, _minimo_, _máximo_, _cuartiles_, _asimetría_ y _kurtosis_.

### **R**

La manera más simple es creando un `vector` que contenga los resultados. Algunas funciones necesarias se encuentran en el paquete `moments`.


```r
results <- c(
  mean = mean(yields),
  sd = sd(yields),
  var = var(yields),
  min = min(yields),
  Q1 = quantile(yields, p = 0.25),
  median =median(yields),
  Q3 = quantile(yields, p = 0.75),
  max = max(yields),
  skw = moments::skewness(yields),
  kurt = moments::kurtosis(yields)
)
results
```

```
      mean         sd        var        min     Q1.25% 
 4.6012000  0.5698707  0.3247527  3.1700000  4.3200000 
    median     Q3.75%        max        skw       kurt 
 4.7300000  4.9700000  5.4600000 -0.9192805  3.5280435 
```

Otra alternativa es poner los resultados en `data.frame`


```r
data.frame(t(results))
```

```
    mean        sd       var  min Q1.25. median Q3.75.  max
1 4.6012 0.5698707 0.3247527 3.17   4.32   4.73   4.97 5.46
         skw     kurt
1 -0.9192805 3.528043
```

Finalmente, usando `dplyr` podemos aplicar facilmente una lista de funciones a un conjunto de datos. Esto es extensible a un conjunto de datos con grupos. 


```r
library(dplyr)

funs <- list(
  mean = ~ mean(.x),
  sd = ~ sd(.x),
  var = ~ var(.x),
  min = ~ min(.x),
  Q1 = ~ quantile(.x, p = 0.25),
  median = ~median(.x),
  Q3 = ~ quantile(.x, p = 0.75),
  max = ~ max(.x),
  skw = ~ moments::skewness(.x),
  kurt = ~ moments::kurtosis(.x)
) 

results <- data.frame(yields) %>% 
  summarise(
    across(.cols = everything(), .fns = funs, .names = "{.fn}")
  )
results
```

```
    mean        sd       var  min   Q1 median   Q3  max
1 4.6012 0.5698707 0.3247527 3.17 4.32   4.73 4.97 5.46
         skw     kurt
1 -0.9192805 3.528043
```

### **Python**

La mayoria de las funciones de la lista se incluyen en el módulo `numpy` pero otras deben ser cargadas desde `scipy`.

Primero calculamos las estadísticas y las guardamos en un objetio `dict`.


```python
from scipy import stats as sp

results = {
  "mean": np.mean(yields),
  "sd": np.std(yields),
  "var": np.var(yields),
  "min": min(yields),
  "Q1": np.quantile(yields, 0.25),
  "median": np.median(yields),
  "Q3": np.quantile(yields, 0.75),
  "max": max(yields),
  "skw": sp.stats.skew(yields),
  "kurt": sp.stats.kurtosis(yields)
}
results
```

```
{'mean': 4.6012, 'sd': 0.5583570184031001, 'var': 0.31176255999999997, 'min': 3.17, 'Q1': 4.32, 'median': 4.73, 'Q3': 4.97, 'max': 5.46, 'skw': -0.9192805310042922, 'kurt': 0.5280434853525069}
```

Luego, el diccionaro se convierte en una estructura tipo `DataFrame` usando el módulo `pandas`.


```python
import pandas as pd

results = pd.DataFrame(results, index = [0])
results
```

