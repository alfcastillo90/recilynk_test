# Operadores JavaScript: `&&`, `||`, y `??`

Este documento explica la diferencia entre los operadores `&&`, `||`, y `??` en JavaScript.

## 1. `&&` (AND Lógico)

- El operador `&&` evalúa dos expresiones y devuelve el primer valor "falsy" que encuentre. Si ambos son "truthy", devuelve el último valor.
- Se usa para verificar si ambas condiciones son verdaderas.
-  En este caso, si la primera expresión es "falsy", se detiene ahí y devuelve ese valor.

**Ejemplo:**
```javascript
const result = true && 'Hola'; // Resultado: 'Hola'
const anotherResult = false && 'Hola'; // Resultado: false
```

## 2. Operador `||` (OR lógico):

- Evalúa dos expresiones y devuelve el primer valor "truthy" que encuentre. Si ambos son "falsy", devuelve el último valor.
- Se usa para verificar si alguna de las condiciones es verdadera.

**Ejemplo:**
```javascript
const result = false || 'Hello'; // Resultado: 'Hello'
const anotherResult = '' || 'Default'; // Resultado: 'Default'
```

- Este operador se usa mucho para establecer valores por defecto.

## 3. Operador `??` (Nullish Coalescing):

- Devuelve el operando de la derecha si el operando de la izquierda es `null` o `undefined`. En otro caso, devuelve el operando de la izquierda.

**Ejemplo:**
```javascript
const foo = null ?? 'default string'; // Resultado: 'default string'
const bar = 'some string' ?? 'default string'; //
```

