---
title: Javascript notes
author: Jill
---
> Eloquent Javascript (2019, press), Marijin Haverbeke

Convenciones Javascript
-----------------------

### Indentación

    Indentación:    2 espacios.
    Indentación:    4 espacios.

### Comentarios

    // Comentario de una línea.
    /* Comentario de varias líneas. */

### Nombres

    Variable:       carSpeed
    Constante:      CAR_SPEED
    Función:        carSpeed()
    Constructor:    Number()

> Tanto el símbolo `$` como `_`, están permitidos en la declaración de bindings
> (variables).

### Paso por valor y referencia

    Variables:      valor.
    Objetos:        referencia.

-------------------------------------------------------------------------------

> - Las comas son opcionales; pero en algunos casos son requeridos para indicar
>   el cuerpo de una sentencia, de modo que es recomendable usarlos.
> - Javascript es asíncrono y no bloqueante (aunque esto con promises y cosas
>   raras) por defecto es secuencial y bloqueante, al menos eso es lo que se
>   aprecia.

-------------------------------------------------------------------------------

### Variables

Las variables se puede declarar tanto con `let` como con `var`:

```js
if (...) {
    let letNumber = 10;
    var varNumber = 11;
}

console.log(letNumber);     # Undefined.
console.log(varNumber);     # 11;
```

> - `let` y `const` son locales al bloque en el que fueron declarados.
> - `var` es visible en toda la función que la contiene o todo el archivo, si
>   es el caso; además permite invocarse antes de declararse, redeclararse
>   nuevamente y *crear propiedad de objeto global*.
> - `let` y `const` fueron introducidos en la es6 o es2015.

#### El entorno

Cuando un programa inicia, por ejemplo el browser, se carga consigo una serie
de variables y funciones parte del lenguaje, que permiten interactuar con el
sistema subyacente y dispositivos como el mouse o el teclado.

> A la colección de bindings y sus valores que existen en un tiempo dado, se
> le conoce como *environment* (entorno).

## Part I: language

### 1. Values, Types, and Operators

#### 1.1 Números

```js
let number = 10;    # A partir del 2015.
var number = 10;    # Antes del 2015.
```

> Tipos especiales de números: `-Infinity`, `Infinity` y `NaN` (not a number).

#### 1.2 Strings

```js
let text = "Lie on the ocean"
let text = 'Float on the ocean'
let text = `Down on the sea`
```

> Este último, también llamado *template literals*, no necesita declarar los
> saltos de línea para crearlos y permite anidar variables a diferencia de los
> otros dos:
> `Hello, ${name}` (string entre backticks).

Obteniendo substrings de una cadena:

```js
let name = "John Doe";

console.log(name.slice(1,3));           # oh
console.log(name.substr(2,3));          # 'ohn '
console.log(name.substring(2,3));       # h
```

> - El método `slice` puede aceptar valores negativos que se cuentan desde el
>   final, su notación de intervaloes es `[)`.
> - El método `substr` recibe los  siguientes parámetros: `substr(start,
>   length)`.
> - El método `substring` recibe los argumentos `substring(start, end)`;
>   notación de intervalos tipo `[]`;

Retornar el valor de un índice dado:

```js
console.log(name.charAt(0));            # valor en el índice 0.
```

> En lugar de `charAt` también se puede utilizar `name[0]` que es más moderno.

Buscar coincidencias con `startsWith` y `endsWith`:

```js
console.log(name.startsWith("Jo"));     # True
console.log(name.endsWith("xyz"));      # False
```

Buscar coincidencias con `indexOf` o `lastIndexOf`:

```js
console.log(name.indexOf('h'));         # Índice del substring 'h'.
console.log(name.lastIndexOf('h', 4));  # Búsqueda de 'h' desde el índice 4.
```

> El método `indexOf` o `lastIndexOf` de una cadena, puede buscar un substring
> que contenga más de un caracter, a diferencia de un arreglo, donde solo se
> puede buscar un elemento.

Buscar coincidencias con `indexOf` o `lastIndexOf` (operador NOT a nivel de
bits):

```js
console.log(~2)   \\ -(2+1)             # -3
console.log(~1)   \\ -(1+1)             # -2
console.log(~0)   \\ -(0+1)             # -1
console.log(~-1)  \\ -(-1+1)            # -0

if(~name.indexOf("Jo"))                 # Posición de la subcadena.
    console.log("Found it!");
```

> Truco a nivel de bits que se usa para resolver el inconveniente con `indexOf`
> que devuelve `0` (false) si el substring se ubica al principio.

Buscar coincidencias con `includes`:

```js
console.log(name.includes("Jo", 2)      # Busca a partir del índice 2.

if(name.includes("Jo")                  # True/False
    console.log(Found it!");
```

> Devuelve `true/false` dependiendo de si existe el substring o no.

Separar/Unir cadenas:

```js
name = "John Doe Doe";

name.split(' ');                        # ["John", "Doe", "Doe"]
name.join('.');                         # "John.Doe.Doe"
```

Otros métodos útiles:

```js
name.trim()                             # Elimina espaciones en blanco.
name.repeat(3);                         # Repite n veces el string.
console.log(String(7).padStart(3, '0')  # Imprime 7 con 3 ceros a la izq.
```

Retornar el código de un caracter en una posición dada:

```js
console.log("z".codePointAt(0));        # 122
console.log("Z".codePointAt(0));        # 90

Obtener el valor de un caracter dado:

```js
console.log(String.fromCodePoint(90));  # Z
```

-------------------------------------------------------------------------------

> - Los datos textuales son almacenados como cadenas, no existen tipos
>   separados para un solo carácter.
> - El formato interno de una cadena es de tipo UTF-16 y no esta vinculado a la
>   codificación de la página.

-------------------------------------------------------------------------------

#### 1.3 Booleanos

Signos de comparación: `<, >, <=, >=, != e ==`.

```js
console.log("Aardvark" <= "Zoroaster")
console.log(NaN == NaN)
```

> `NaN` es el resultado de una computación absurda, por lo que su comparación
> siempre es `false`.

Operadores lógicos disponibles: `and`, `or` y `not` como `&&`, `||` y `!`.

```js
console.log(false or true)
console.log(1 + 1 == 2 && 10 * 10 > 50)
```

> En el último caso, la precendencia es el siguiente (menor a mayor): `||`,
> `&&`, `operadores de comparación` y el resto.

#### 1.4 Operador ternario

```js
console.log(true ? 1 : 2)
```

#### 1.5 Valores vacíos

Valores pero que no contienen información: `null` y `undefined`.

> La diferencia de significado entre ambos valores es un error de diseño de
> Javascript; tratarlos, en la mayoría de los casos, como intercambiables.

#### 1.6 Conversión automática de tipos

Conversiones automáticas que permiten operaciones raras:

```js
console.log(8 * null)       # 0
console.log("5" - 1)        # 4
console.log("5" + 1)        # 51
console.log("five" * 2)     # NaN
console.log(false == 0)     # true
```

> - A la conversión implícita de tipos de datos que realiza Javascript, se le
>   denomina como *type coercion* o coerción de tipos.
> - Para evitar la conversión automática de tipos en la comparación de valores,
>   se puede usar el operador `===` o `!==`.

Para valores de diferentes tipos, los operadores `OR` y `AND` convierten el
valor izquierdo en booleano para decidir que hacer posteriormente.

- Cortocircuito de operadores lógicos (OR):

```js
console.log(null || "user")         # user
console.log("Agnes" || "user")      # Agnes
console.log(0 || 3)                 # 3
```

> - El operador `OR` intenta devolver el valor `true`, basandose únicamente en
>   la comprobación del valor izquierdo.
> - En la conversión de números y strings a booleano, los valores `0, NaN` y
>   `""` cuentan como `false`.

- Cortocircuito de operadores lógicos (AND):

```js
console.log(null || "user")         # null
console.log("Agnes" || "user")      # user
console.log(0 || 3)                 # 0
```

> El operador `AND` intenta devolver el valor `false`, basandose únicamente en
> la comprobación del valor izquierdo (contrario al `OR`).

### 2. Program structure

#### 2.1 Variables y constantes

- Crear variables o enlaces con `let`:

```js
let one = 1, two = 2;
console.log(one + two);             # 3
```

> Es preferible imaginar los enlaces o variables como tentáculos que sujetan
> valores, en lugar de cajas que los contienen.

- Crear variables o enlaces con `var`:

```js
var $name = "Ayda";
var greeting = "Hello ";

console.log(greeting + $name);
```

- Crear constantes:

```js
const PI = 3.14;
```

#### 2.2 Condicionales

Condicional `if-else`:

```js
if (num < 10) {
    console.log("Small");
} else if (num < 100) {
    console.log("Medium");
} else {
    console.log("Large");
}
```

Condicional `switch`:

```js
switch (prompt("What is the weather like?")) {
    case "rainy":
        console.log("Remember to bring an umbrella");
        break;
    case "sunny":
        console.log("Dress lighty");
        break;
    case "cloudy":
        console.log("Go outside");
        break;
    default:
        console.log("Unknown weather type!");
        break;
}
```

#### 2.3 Bucles

Bucle `while`:

```js
while (i <= 10) {
    console.log(i);
    i++;
}
```

Bucle `do...while`:

```js
do {
    console.log(i);
    i++;
} while (i <= 10)
```

Bucle `for`:

```js
for (let i=0; i < 10; i++) {
    if(x == 5)
        break;          # Sale del bucle.
    if(x == 6)
        continue;       # Continua con la siguiente iteración.

    console.log(x);
}
```

> - También es posible salir de un bucle con la sentencia `break`.
> - La sentencia `continue` interrumpe la ejecución para continuar con la
>   siguiente iteración.

Bucle for y arreglos u objetos:

```js
for (let i=0; i < array.length; i++) {
    console.log(i);
}
```

Bucle `for...of`:

```js
for (let value of array) {
    console.log(value);
}
```

> Permite acceder al valor, mas no a los índices.
> También es posible utilizarlo con strings y otras estructuras de datos.

Bucle `for...in`:

```js
for (let value in array) {
    console.log(array[value]);
}
```

> - Como los arreglos son una tipo especial de objetos, es posible que el
>   iterador `for...in` itere a través de todas las propiedades del arreglo y
>   no tan solo de los númericos.
> - El iterador `for...in` está optimizado para objetos génericos pero no para
>   los arreglos, por lo que es 10-100x más lento.

### 2. Funciones

Declaración de función:

```js
console.log(square(3));

function square (x) {
    return x * x;
}
```

> La declaración de función puede no formar parte del flujo de control regular.

Arrow functions (funciones de flecha):

```js
const power = (x) => {return x * x; };  # Forma regular.
const power = x => x * x;               # Forma corta.
const print = () => {...};              # Sin parámetros.
```

> La palabra `function` es reemplazado por la flecha `=>`.

Función como constante:

```js
const square = function(x) {
    return x * x;
};
```

Función  como valor:

```js
let launchMissiles = function() {
    missileSystem.launch("now");
};

if(safeMode) {
    launchMissiles = function () {...};
}
```

Anidamiento de funciones: 

```js
const hummus = function(factor) {
    const ingredient = function(amount, unit, name) {
        ...
    };
};
```

Llamando a una función con argumentos extra:

```js
function square(x) { return x * x; }
console.log(square(8, true, "hello"));      # 64
```

> Los argumentos restantes son ignorados y si hay parámetros faltantes, a este
> se le asigna el valor `undefined`.

Función con argumentos por defecto:

```js
function power(base, exponent = 2) {
    ...
}
```

Verificar parámetro omitido, nulo, undefined o vacío de una función:

```js
function showMessage(text) {
    text = text || "empty";
}
```

Verificar parámetro `undefined` o `null` mediante el *operador de fusión null*
(nullish coalescing operator):

```js
let user;
console.log(user ?? "empty");                   # empty 
```

> - Esta funcionalidad es reciente (~2020), por lo que no esta soportado por
>   todos los navegadores.
> - El *operador de fusión null* se puede usar tantas veces como sea necesario:
>   `(value1 ?? value2 ?? value3)`.
> - También se puede usar `||` del mismo modo que con `??`, la diferencia está
>   en que `||` no distingue una variable vacía o con valor 0 de `undefined` o
>   `null`.

Función con un indeterminado número de argumentos (rest arguments):

```js
function max(...numbers) {
    let result = -Infinity;
    for (let number of numbers) {
        if (number > result) result = number;
    }
    return result;
}

console.log(max(4, 1, 9, -2, 11, 3));   # 11
```

> Las funciones de este tipo, vinculan los argumentos en un arreglo, por lo que
> es posible crear un arreglo de números y utilizarlos de la siguiente manera:
> `max(4, ...numbers, 8)`.

#### 2.1 Closure

Oh!, malditos closures!

```js
function multiplier(factor) {
    return number => number * factor;
}

let twice = multiplier(2);
console.log(twice(5));                  # 10
```

> - Un closure es: una función anidada dentro de otra y que, después de
>   finalizado la función contenedora, la función contenida aún pueda operar
>   con el entorno de la función padre (o algo asi?).
> - Como no es posible acceder a la función anidada, es necesario almacenar la
>   funcion contenedora como valor de una variable o constante. Además, la
>   función contenida debe ser el `return` de la función padre.
> - La función padre puede ser una fábrica de funciones.
> - Muchas funciones anidadas pueden compartir el mismo entorno de la función
>   contenedora.
> - Los `closures` permiten proteger el acceso a las variables, similar a los
>   métodos privados de Java que solo pueden ser invocados por los métodos de
>   la misma clase. La carencia de encapsulamiento de Javascript, permite a los
>   `closures`, ser la mejor opción para este tipo de propósitos.
> - Es importante identificar las situaciones de uso de los `closures` en favor
>   del rendimiento (los closures suelen consumir muchos recursos).
> - Casi todo en Javascript gira en torno a las flexibilidad de las funciones.
> - No se que tanto tenga que ver, pero Javascript utiliza funciones como
>   clases.

#### 2.2 Recursividad

```js
// simple example:
function power(base, exponent) {
    if (exponent == 0)
        return 1;
    else
        return base * power(base, exponent - 1);
}

/* Given a number, tries to find a sequence of additions and multiplications
 * (5,3) 
*/
function findSolution(target) {
    function find( current, history) {
        if (current == target) {
            return history;
        } else if (current > target) {
            return null;
        } else {
            return find(current + 5, `(${history} + 5`)
            || find(current * 3, `${history} * 3)`);
        }
    }
    return find(1, "1");
}

console.log(findSolution(28));
```
```
(((1 + 5 * 3) + 5 + 5))
```

> Es necesario distinguir la importancia de la elegancia vs la velocidad.

### 4. Data Structures: Objects and Arrays

#### 4.1 Arreglos

Crear arreglo:

```js
let array   = new Array("one", 2, "three", 4, "five");
let array   = ["one", 2, "three", 4, "five"];

console.log(array[2]);                                  # 'three'
```

> Si `new Array()` recibe un solo argumento, este es la longitud del arreglo.

Agregar/Quitar elementos del arreglo (stack):

```js
array.push("six")       # Agregar nuevo elemento al final.
array.pop()             # Quita y retorna el último elemento.
```

Agregar/Quitar elementos del arreglo (queue):

```js
array.unshift("six")    # Agrega nuevo elemento al principio.
array.shift()           # Quita y retorna el primer elemento.
```

> - Es posible agregar múltiples elementos tanto con `push()` como con
>   `unshift()`.
> - `push/pop` se ejecutan más rápido que `unshift/shift`.

Buscar un valor especificado:

```js
let names = ["John", "Edward", "Peter", "Sue"];

console.log(names.indexOf("Peter"));            # 2
console.log(names.lastIndexOf("Peter"));        # 2
```

> - El método `lastIndexOf()` busca desde el final hasta el principio,
>   contrario a `indexOf()`.
> - También se puede ingresar un segundo argumento que indique desde que índice
>   comenzar a buscar.
> - Si el valor no es encontrado, devuelve `-1` o bien el índice del valor.

Mostrar elementos dentro de un rango determinado:

```js
array.slice(1, 4);
```

> En notación de intervalos sería: `[)` (cerrado, abierto).

Concaternar arreglos:

```js
array2.concat(array.slice(2, 3));
```

> Si no se le pasa a `concat` un argumento de tipo arreglo, este agrega el
> elemento al nuevo arreglo. 

"Esparcir" el contenido de un arreglo dentro de otro:

```js
let numbers = [1, 2, 3];
let values = ['a', ...numbers, 'b'];    # ['a', 1, 2, 3, 'b']
```

> A esto también se le conoce como notación de triple punto y su uso está
> relacionado con las funciones que reciben argumentos indeterminados.

Arreglo multidimensional:

```js
// Matriz:
let matrix  = [
    [1, 2, 3],
    [4, 5, 6]
];

console.log(matrix[0][1]);      # 2
```

Cosas raras que se pueden hacer con un arreglo:

```js
array.name = "Esther";      # Crea una propiedad.
array[99] = "value";        # Indice mayor que la longitud.
array.length = 7;           # length es editable.
```

> - Los elementos de un arreglo son almacenados como propiedades que usan
>   números como nombres de propiedad. En el fondo siguen siendo objetos que
>   pueden comportarse como tal y echar a perder la optimización de datos
>   contiguos.

-------------------------------------------------------------------------------

> - Casi todos los valores tienen propiedades en Javascript, a excepción de
>   `null`.
> - Se puede acceder a las propiedades de los valores mediante la notación de
>   punto o corchete: `array.length` o `array["length"]`.
> - La propiedad `length` es el valor del índice más uno, mas no los elementos
>   dentro del objeto o arreglo.
> - A las propiedades que contiene funciones se les conoce como métodos del
>   valor al que pertenecen: `name.toUpperCase()`.

-------------------------------------------------------------------------------

#### Objetos

Crear objetos:

```js
let obj = new Objetc();                             # Constructor
let obj = {name: "John", lastName: "Doe", age: 38,};
const obj = {name: "John", lastName: "Doe", age: 38,};
```

> Los objetos definidos con `const` solo pueden modificarse mediante la
> notación de punto o corchete.
> Es posible dejar una coma al final de la última propiedad para facilitar
> agregar/eliminar/mover propiedades; a esto se le llama *trailing/hanging
> comma*.

Crear objetos con propiedades calculadas (computed properties):

```js
let fruit = prompt("bla bla bla");

// Simple:
let obj = {
    [fruit]: 10
}
// Complejo:
let obj = {
    [fruit + "computers"]: 10
}
```

> El valor de `fruit` será el que el usuario ingrese o el valor que se haya
> definido.

Objeto con nombre de propiedad de varias palabras:

```js
let obj = {
        name: "John",
        age:  32,
        "card number": "111-222-3333"
    }
```

> Para acceder a este tipo de propiedad se usa la *notación de corchetes*:
> `obj["card number"]`.

Acceder a las propiedades de un objeto:

```js
console.log(obj.name)                               # John
console.log(obj["name"])                            # John
```

> La notación de corchete puede ser usado para permitir al usuario ingresar el
> nombre de la clave de un objeto.

Devolver un objeto en una función:

```js
function makeUser(name, age) {
    return {
        name: name,
        age: age
    };
}
```

> Es posible omitir `name: name` y en su lugar usar simplemente `name` o usar
> ambas formas.

Borrar elementos de un objeto:

```js
delete obj.age
console.log(obj.age)                                # Undefined
```

> Si la propiedad no existe, Javascript no muestra ningun error, en su lugar
> retorna `undefined`.

Verificar si existe la propiedad especificada del objeto:

```js
console.log("property" in obj);                     # True/False
```

> También se podría utilizar comparaciones con `undefined` para saber si una
> propiedad existe; pero a diferencia de `in`, este no funciona cuando el valor
> almacena `undefined`.

Mostrar las *keys* de un objeto:

```js
console.log(Object.keys(obj));                      # Devuelve un arreglo.
```

Copiar las propiedades de un objeto dentro de otro:

```js
Object.assign(obj1, obj2);                          # obj1 ← obj2
```

-------------------------------------------------------------------------------

> - Es posible utilizar las palabras reservadas del lenguaje para el nombre de
>   las propiedades de un objeto, tales como: `for: 3`, `return: 4`, `let:
>   0`...
> - Los comparadores aplicados a objetos comparan el objeto en sí, mas no el
>   contenido, sea el operador `==` o `===`.

-------------------------------------------------------------------------------

#### Mutability




### 5. Higher-Order Functions

### 6. The Secret Life of Objects 

### 7. Project: A Robot

## Part II: Browser
## Part III: Node


