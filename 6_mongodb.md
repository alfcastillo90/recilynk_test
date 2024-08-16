## Estructura de la Colección
- La colección "pedidos" tiene la siguiente estructura:

```json
{
  "_id": ObjectId("123"),
  "customerId": ObjectId("456"),
  "items": [
    {
      "productId": ObjectId("789"),
      "quantity": 2
    },
    {
      "productId": ObjectId("012"),
      "quantity": 1
    }
  ],
  "orderDate": ISODate("2022-01-01T00:00:00Z")
}

```

### Propuesta de Mejoras para Optimizar la Estructura de la Base de Datos:
#### Normalización vs. Denormalización:

- Actualmente, la colección almacena tanto el customerId como los items dentro del mismo documento. Esto puede ser eficiente si los pedidos y sus detalles no se actualizan frecuentemente.
Si los detalles de los productos cambian frecuentemente o si hay un gran volumen de datos, podría ser mejor almacenar los items en una colección separada y vincularlos mediante referencias (productId). Esto facilita la actualización de productos y evita la duplicación de datos.

#### Anidación de Estructuras:

- Si la estructura de los items siempre es la misma (simplemente productId y quantity), la anidación actual está bien. Sin embargo, si la estructura se vuelve más compleja, podrías evaluar si es mejor normalizar o si es necesario mantener el anidamiento.

#### Fechas:

- Si hay una necesidad frecuente de consultar la base de datos por rangos de fechas, es bueno asegurarse de mantener el campo orderDate en un formato optimizado como ISODate, y considerar aplicar un índice sobre este campo.

### Propuesta de Estrategias de Indexación:
#### Índice Compuesto:

- Una buena estrategia podría ser aplicar un índice compuesto en customerId y orderDate si las queries suelen buscar pedidos de un cliente específico en un rango de fechas.

```javascript
db.pedidos.createIndex({ customerId: 1, orderDate: -1 });
```

- Este índice optimiza las consultas que buscan pedidos por cliente y ordenados por fecha (por ejemplo, los últimos pedidos).

#### Índice para Subdocumentos:

- Si necesitamos realizar búsquedas específicas sobre los items (por ejemplo, encontrar todos los pedidos que incluyan un cierto productId), podriamos crear un índice para ese campo dentro de items:

```javascript
db.pedidos.createIndex({ "items.productId": 1 });
```

- Esto permite realizar búsquedas rápidas basadas en productos específicos dentro de los pedidos.

#### Índice para Fecha:

- Si las consultas más comunes son por fecha (por ejemplo, obtener todos los pedidos en un rango de fechas), un índice específico en orderDate también sería recomendable:

```javascript
db.pedidos.createIndex({ orderDate: 1 });
```