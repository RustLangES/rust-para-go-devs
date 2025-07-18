# Tuplas y Colecciones

## Tuplas

Una tupla es un conjunto de variables que se agrupan, generalmente de diferentes 
tipos, en una sola entidad. Esto permite que se manejen como una unidad en lugar 
de individualmente.

En Go no existen las tuplas como un tipo de dato estrictamente, pero si podemos
llegar a reconocer algo similar a una tupla por ejemplo cuando usamos el 
retorno de múltiples valores en una función. Por ejemplo:

```go
# package main
# 
# import (
# 	"fmt"
# )
# 
func getCoordinates() (int, int) {
    return 10, 20
}

func main() {
    x, y := getCoordinates()
    fmt.Println(x, y) // Imprime: 10 20
}
```

Los múltiples valores devueltos suelen cumplir una función similar a las tuplas.

En Rust, las tuplas son un tipo de dato. Se definen con paréntesis y accedemos
mediante la posición. Por ejemplo:

```rust
    let coords = (10, 20, "Las coordenadas", "x e y");
    println!("{}, {}", coords.0, coords.1);
```

Además gracias al pattern matching podemos desestructurar las tuplas de
forma más sencilla:

```rust
    let coords = (10, 20, "Las coordenadas", "x e y");
    let (x, y, desc1, desc2) = coords;
    println!("{desc1} {desc2} son:");
    println!("{x} para X");
    println!("{y} para Y");
```

Si quisiéramos declarar el valor de retorno de una función como una tupla 
podría ser algo así:

```rust,no_run,ignore
fn getCoordinates() -> (i32, i32) {
    (10, 20)
}
```

## Arrays / Slices / Vectores

### Arrays

Go utiliza la misma sintaxis para los tres tipos de datos

```go
#package main
# 
#import (
#	"fmt"
#)
# 
#func main() {
    // Arreglo (tamaño fijo)
    var arr [3]int = [3]int{1, 2, 3}

    // Slice (tamaño dinámico, vista sobre array)
    var s []int = arr[1:3]

    // Slice dinámico creado desde cero
    s2 := []int{1, 2, 3, 4}
    #
    #fmt.Println("arr =", arr) 
    #fmt.Println("s =", s) 
    #fmt.Println("s2 =", s2) 
#}
```

Mientras que Rust hace una clara diferencia entre cada tipo, el array es el tipo
primitivo, es una lista de elementos. 

Por ejemplo:

```rust,no_run
    let arr: [i32; 3] = [1, 2, 3];
    let arr = [1, 2, 3];
    let arr = [1; 5]; // [1,1,1,1,1] 
```

Los arreglos no se pueden redimensionar, tiene un tamaño fijo, podemos modificar 
las posiciones si declaramos el arreglo como mutable, pero no podemos agregar 
nuevos elementos.

El que tengan un tamaño fijo ayuda a que Rust nos diga si estamos accediendo a 
una posición del arreglo inexistente, es decir si estamos saliendo del rango
de elementos que están dentro del arreglo:

```rust
    let arr = [7, 5, 4];

    arr[99];
```

Este error sera encontrado en tiempo de compilación.

### Slices

Consideramos a los slices como una referencia a un rango de elementos:

```rust
    let arr = [7, 5, 4];

    let slice: &[i32] = &arr[1..3]; // [5, 4]
    #println!("{slice:?}")
```

Al igual que los array convencionales, si intentamos pasar un rango invalido
obtendremos un error en tiempo de compilación.

### Vectores

Además Rust tiene el tipo de dato `Vec`, el cual es un tipo de dato dinamico,
esta colección puede ser modificada agregando nuevos elementos, podemos crear
`Vec` de varias formas, pero una de las más convencionales es con la macro 
`vec!`:

```rust
    let mut v = vec![1, 2, 3];
    v.push(4);
    #println!("{v:?}")
```

Al declarar como mutable podemos utilizar el método `push` para agregar 
elementos dentro. 

Sin embargo al ser dinamico Rust no puede calcular en tiempo de compilación que
valores serán cargados en memoría durante la ejecución, es por eso que este 
código:

```rust
    let mut v = vec![1, 2, 3];
    v[99];
```

Deberá fallar en tiempo de ejecución, Rust para solucionar nos da el método 
`get` que es lo que deberemos de usar para obtener valores dentro de un arreglo.

```rust
    let mut v = vec![1, 2, 3];
    v.get(99);
    #println!("{:?}", v.get(99));
```

El método `get` nos devolverá un `Option<T>` profundizaremos sobre este tipo
en los siguientes capítulos.

Por otro lado hay que aclarar que tanto los array, como los slices y los `Vec`,
son tipos de datos que solo almacenan un único tipo de dato dentro, como pueden
ver en los ejemplos, entonces solo podemos almacenar o i32, Strings, flotantes, 
o lo que necesitemos, pero unicamente elementos que sean de ese tipo de dato,
para utilizar más de un tipo de dato posiblemente deberíamos de utilizar una 
tupla o usar otro tipo de dato.

### Mapas / Diccionarios / HashMaps

Un mapa es una estructura de datos que guarda pares clave-valor.
- Cada clave es única
- A partir de una clave, puedes obtener el valor asociado

En Go hariamos algo como esto:

```go
#package main
# 
#import (
#	"fmt"
#)
# 
#func main() {
    m := make(map[string]int)
    m["manzana"] = 3
    m["naranja"] = 5

    fmt.Println("Hay", m["manzana"], "manzanas")
#}
```

Mientras que el mismo ejemplo en Rust seria así:

```rust
    #use std::collections::HashMap;
    #
    let mut mapa = HashMap::new();
    mapa.insert("manzana", 3);
    mapa.insert("naranja", 5);

    println!("Hay {} manzanas", mapa.get("manzana").unwrap());
```
