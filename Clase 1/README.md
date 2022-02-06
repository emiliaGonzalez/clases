## Introducción

Dentro de la mayoría de los aplicativos de bases de datos (aka "DB" del inglés Database) existen distintos niveles:

1. **DBs**: Bases de datos, es decir dentro de una misma instalación pueden existir distintos scopes o namespaces que permiten separar y organizar los datos en distintos aplicativos, organizaciones o sistemas.

2. **Tables or collections**: Las tablas para sistemas SQL o colecciones, indices, etc son grupos de registros comunes que semanticamente son iguales y se relacionan con otras tablas de distintos tipos. ej.tablas de: **productos**, **categorias**, **carrito**, **clientes**, etc..

3. **Document, row, record**: Cada tabla incluy _n_ cantidad de registros o documentos que comparten un mismo sentido semántico y una misma estructura de datos.

Representandolo de una manera más gráfica:

```
db > table > record
```

## SQL

_SQL_ significa **Structured Query Language** es decir es un lenguaje de consulta estructurado.

Antes de comenzar con la creación de este lenguaje necesitamos diseñar los datos sobre los que vamos a trabajar. Hablar de diseño de datos es hablar del diseño de tablas.

### Convenciones

La importancia de las convenciones radica en entender que las estructuras de datos normalmente viven mucho más que el código o el aplicativo mismo del que nacen, es muy común que una aplicación evolucione, se acabe y nazcan nuevas consumiendo las mismas bases de datos. También es importante saber que el diseño de una base de datos es un **_contrato_**, es decir cualquier cambio a una base de datos por mu pequeño que sea puede romper o modificar totalmente el comportamiento de una aplicación.

El tener convenciones a lo largo de tu modelo de datos significa que cualquier desarrolador necesita menos tiempo para buscar a lo largo de tablas, vistas o columnas. Es más sencillo desarrollar cuando las nomenclaturas son predecibles, ej.

```
person_id = id on person
```

1. **_Evitar usar comillas en nombres de identificacion_** ej.: evita usar nombres como `"FirstName"` o `"All Employees"`. Escribir SQL usando nombres con comillas es frustrante y hace imposible hacer ciertas operaciones.

2. **_Minúsculas_** Usa siempre minpusculas en identificadores de cualquier cosa, nombres de tablas, columnas, etc.. El no hacerlo significa tener que usar en ocasiones comillas (ver punto 1), es propenso a errores, porque se presta a poder equivocarse en el uso mixto de mayúsculas y minúsculas, y requiere de mayor complejidad cognitiva el leer, escribir y referenciar palabras con un **mixed casing** ej: Usa `first_name`, no `First_Name`.

3. **_Tipos de datos no son nombres_** Cuando elijas el nombre de tus tablas o de tus columnas, elije nombres que sean descriptivos de lo que vas a guardar, no solo `text` o `fecha` ej. no uses `fecha` o `date`, es mejor usar `post_created_at`.

4. **_Usa guion bajo para separar palabras (sanke case)_** los identificadores deben ser una sola **palabra** por lo que cuando se quieren usar varias es mejor usar snake case ej. `first_name`

5. **_Usa palabras completas, no abreviaciones_** Entender para un equipo lo es todo, y es más fácil entender cuando un nombre es descriptivo y **legible**, además SQL es un lenguaje escrito en inglés por lo que escribir en distintos idiomas es cognitivamente agotante. ej. No uses `Num_Profesor` en su lugar usa `teacher_number`.

6. **_Cuando es común utiliza abreviaciones_** Hay palabras como **Internationalization** o **localization** que en la jerga técnica son mejor conocidos como `i18n` o `l10n` ej. No uses `latitude` usa `lat`, o en lugar de `Internationalization` usa `i18n`. Si tienes dudas de si hay una abreviación mejor usa la palabra complete en inglés en sanke case

7. **_No uses palabras reservadas_** No uses palabras reservadas como `user` o `table`ya que puede interferir con las sentencias.

### Keys

1. **_Primary key_** Todas las tablas requieren de un identificador único por registro que sirve para identificar inequivocamente a un solo elemento de la tabla.

2. **_Foreign Keys_** Para que una tabla pueda referenciar a otra es necesario que contenga una columna que contenga el identificador único de otra tabla.

### Convenciones en relaciones

1. **_Nombra tus tablas en singular_** En lugar de usar `teachers` usa `teacher` lo cual asegura la consistencia en el nombre de las tablas, reduce la ambiguedad y permite una implementación **_transparente_** con lenguajes de programación y gestores de bases de datos. ej. `team_member` **_Java_** lo puede interpretar automáticamente como la clase `TeamMember`

2. **_Primary key_** Cuando la llave primaria de una tabla se conforma de una sola columna, esta debe ser llamada simplemente `id`. Es corto, no es ambiguo y cuando escribes sentencias no tienes que recordar nombres de campos para poder relacionar. Usar prefijos como `teacher_id`es redundante, por eso se tiene que crear nombres de tablas y columnas explicitas, de tal manera que las consultas sean predecibles, hacer esa especia de **_namespace_** es siempre una mala idea, además de que choca con las `Foreign Keys`.

3. **_Foreign Keys_** Las llaves foraneas deben ser una combinación del nombre de la tabla referenciada y el campo referenciado por ejemplo `team_id` donde se hace referencia al campo `id` en la tabla `team`.

## Ejemplos

### Tabla simple

**_product_**

| id  | name      | brand   | price | sku         |
| --- | --------- | ------- | ----- | ----------- |
| 1   | iPhone 12 | Apple   | 23000 | 10203040303 |
| 2   | Galaxy S6 | Samsung | 20000 | 11231324123 |
| 3   | iPhone 11 | Apple   | 16000 | 11231324123 |

### Tabla Relaciones

**_product_**

| id  | name      | brand_id | price | sku         |
| --- | --------- | -------- | ----- | ----------- |
| 1   | iPhone 12 | 1        | 23000 | 10203040303 |
| 2   | Galaxy S6 | 2        | 20000 | 11231324123 |
| 3   | iPhone 11 | 1        | 16000 | 11231324123 |

**_inventory_**

| id  | available_quantity | hold_quantity | in_transit | product_id |
| --- | ------------------ | ------------- | ---------- | ---------- |
| 10  | 23                 | 200           | 4          | 1          |
| 34  | 10                 | 199           | 30         | 2          |
| 58  | 8                  | 8             | 50         | 3          |

**_brand_**

| id  | name    |
| --- | ------- |
| 1   | Apple   |
| 2   | Samsung |

### Enumerables

Es importante resaltar el término **_Enumerable_** ya que nos permite generar catálogos. Ya que un enumerable generá una lista de posibles valores disponibles, de tal modo que no se puedan usar valores que no esten definidos en el enumerable, generalmente asocian un identificador a un valor, por lo que solo constan de esos dos datos. En el ejemplo anterior la tabla `brand`es un enumerable ya que limita a la tabla `product`a solo poder contener la marca **\*Apple** o **_Samsung_**
