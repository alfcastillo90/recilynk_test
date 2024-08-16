## Análisis de la función `exampleFunction`

### Descripción
La función `exampleFunction` toma tres números como entrada (`a`, `b` y `c`) y devuelve el mayor de ellos. 

### Funcionamiento
La función utiliza una serie de comparaciones anidadas para determinar el valor máximo. A continuación, se presenta un desglose paso a paso:

1. **Comparaciones:**
   * Se compara `a` con `b`.
   * Si `a` es mayor, se compara `c` con `a`.
   * Si `a` es menor o igual, se compara `c` con `b`.

2. **Retorno:**
   * Dependiendo de las comparaciones anteriores, se devuelve el valor máximo encontrado.

### Complejidad
La complejidad temporal de esta función es **constante**, O(1). Esto significa que el tiempo de ejecución no aumenta significativamente con el incremento de los valores de entrada.

### Mejora propuesta

Para mejorar la legibilidad y simplificar la función, podemos utilizar el operador ternario de JavaScript y la función `Math.max()`:

```javascript
function exampleFunction(a, b, c) {
  return Math.max(a, Math.max(b, c));
}