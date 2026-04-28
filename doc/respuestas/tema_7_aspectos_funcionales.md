# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

Un puntero a una función en C es una variable que almacena la dirección de memoria de una función. A diferencia de una llamada directa, permite invocar dinámicamente distintas funciones que compartan la misma firma (tipo de parámetros y tipo de retorno). Este mecanismo introduce un primer nivel de abstracción funcional en un lenguaje principalmente imperativo.
Este concepto resulta especialmente útil para implementar comportamientos configurables en tiempo de ejecución, como callbacks o estrategias. Desde el punto de vista conceptual, permite tratar a las funciones como valores que pueden asignarse a variables y pasarse como argumentos, aunque con fuertes limitaciones frente a lenguajes funcionales modernos.
````
#include <stdio.h>
#include <ctype.h>

void aMayusculas(char *cadena) {
    for (int i = 0; cadena[i]; i++) {
        cadena[i] = toupper(cadena[i]);
    }
}

int main() {
    void (*aMayusculasPtr)(char *) = aMayusculas;

    char texto[] = "hola mundo";
    aMayusculasPtr(texto);

    printf("%s\n", texto);
    return 0;
}
````
## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una función lambda es una función anónima que puede definirse directamente en una expresión y asignarse a una variable o pasarse como argumento. Su objetivo principal es permitir comportamientos breves y locales sin necesidad de definir métodos o funciones con nombre, favoreciendo un estilo más declarativo.
En JavaScript las funciones lambda existen desde sus primeras versiones mediante funciones anónimas, mientras que en Java se incorporan oficialmente a partir de Java 8 como parte del soporte al paradigma funcional. En Java, las lambdas siempre están asociadas a una interfaz funcional, lo que mantiene la comprobación estática de tipos.
````
let aMayusculas = texto => texto.toUpperCase();
console.log(aMayusculas("hola mundo"));
````
````
Function<String, String> aMayusculas = s -> s.toUpperCase();
System.out.println(aMayusculas.apply("hola mundo"));
````
## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El paradigma funcional es un estilo de programación que se basa en evaluar funciones matemáticas, evitando el estado mutable y los efectos secundarios. En este paradigma se prioriza el qué se hace sobre el cómo se hace, utilizando funciones como principal mecanismo de composición de programas.
Lenguajes como Java se consideran multi‑paradigma porque, aunque nacieron orientados a objetos, han incorporado características de otros paradigmas, como el funcional. Java 8 añade lambdas, streams y referencias a métodos, permitiendo mezclar programación imperativa, OO y funcional según convenga.
Decir que las funciones son “ciudadanos de primera clase” implica que pueden almacenarse en variables, pasarse como parámetros, devolverse desde otras funciones y componerse, al mismo nivel que cualquier otro tipo de dato.

## 4. Explica la sintaxis básica de una función lambda en Java.

La sintaxis básica de una función lambda en Java consta de tres partes: la lista de parámetros, el operador -> y el cuerpo de la función. Los parámetros pueden tener tipo explícito o inferido, dependiendo del contexto proporcionado por la interfaz funcional.
Cuando el cuerpo de la lambda es una sola expresión, el return y las llaves pueden omitirse. Si el cuerpo contiene varias instrucciones, debe emplearse un bloque con {} y una sentencia return explícita.
Este diseño permite reducir significativamente la verbosidad frente a las clases anónimas, manteniendo al mismo tiempo el sistema de tipos estático y la seguridad en tiempo de compilación.
````
s -> s.toUpperCase()
````
## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

Recibir funciones como parámetros permite desacoplar el algoritmo del comportamiento concreto que se desea aplicar, siguiendo el principio de inversión de dependencias. Esta técnica resulta clave tanto en el paradigma funcional como en diseños flexibles orientados a objetos.
El método transformar recibe una cadena y una función transformadora, y delega en dicha función el trabajo específico. De este modo, el método es reutilizable con cualquier transformación compatible, sin necesidad de modificar su implementación.
````
function transformar(texto, transformador) {
    return transformador(texto);
}
````
````
static String transformar(String texto, Function<String, String> f) {
    return f.apply(texto);
}
````
## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

Una de las principales ventajas de las funciones lambda es la posibilidad de definir el comportamiento “en el punto de uso”. Esto permite escribir código más expresivo y localizado, evitando la proliferación de funciones auxiliares con nombre.
En este caso, la función de inversión de cadena se define directamente al pasarla como argumento a transformar. Esto refuerza la idea de que el comportamiento es un dato más del programa.
````
transformar("hola", t => t.split("").reverse().join(""));
````
````
transformar("hola", s -> new StringBuilder(s).reverse().toString());
````
## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

Un closure se produce cuando una función lambda captura y utiliza variables definidas en su contexto externo. En Java, dichas variables locales deben ser efectivamente finales, lo que garantiza coherencia y evita efectos secundarios inesperados.
Este mecanismo permite construir funciones que recuerdan información del entorno donde fueron creadas, incluso cuando se ejecutan posteriormente. Conceptualmente, es una extensión natural de las lambdas hacia comportamientos parametrizados por su contexto.
````
String sufijo = "!!!";

Function<String, String> agregarSufijo =
    s -> s + sufijo;
````

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

Los punteros a funciones en C son mecanismos de bajo nivel que solo almacenan direcciones de memoria. No capturan contexto ni variables externas, y no están ligados a un sistema de tipos expresivo más allá de la firma de la función.
Las funciones lambda, en cambio, son objetos de alto nivel que pueden encapsular comportamiento y contexto (closures), integrarse con el sistema de tipos del lenguaje y combinarse de forma más segura y expresiva.
Por tanto, aunque ambos permiten tratar funciones como valores, las lambdas representan una abstracción muy superior en términos de expresividad, seguridad y capacidad de composición.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

Devolver funciones permite fabricar comportamientos especializados a partir de parámetros de configuración. La función crearDescuento devuelve una función que aplica un porcentaje fijo previamente capturado.
Este es un ejemplo claro de closure: la lambda devuelta recuerda el valor del porcentaje incluso tras salir del método. Cada función descuento creada mantiene su propio estado inmutable.
````
static Function<Double, Double> crearDescuento(double porcentaje) {
    return precio -> precio * (1 - porcentaje);
}
````
## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

Una interfaz funcional es una interfaz que declara exactamente un método abstracto. Este método define la firma que deben cumplir las funciones lambda que se asignen a dicha interfaz.
Java utiliza interfaces funcionales para proporcionar tipado estático a las lambdas. Pueden incluir métodos default o static, siempre que solo exista un método abstracto.
La anotación @FunctionalInterface no es obligatoria, pero permite al compilador verificar que se cumple este requisito.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

Definir una interfaz funcional propia permite modelar comportamientos específicos del dominio sin depender exclusivamente de interfaces genéricas. En este caso, se define un transformador de cadenas.
Este enfoque es especialmente útil cuando se desea mayor expresividad semántica o se quiere mejorar la legibilidad del código.
````
@FunctionalInterface
interface Transformador {
    String transformar(String texto);
}
````
## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

Generalizar la interfaz mediante genéricos permite reutilizarla para múltiples tipos de transformación. Esto sigue los principios básicos de la genericidad vistos previamente en Java.
El uso de parámetros de tipo permite mantener seguridad estática sin duplicar interfaces para cada combinación de tipos.

````
@FunctionalInterface
interface Transformador<T, R> {
    R transformar(T valor);
}

Transformador<Double, Integer> redondear =
    d -> d.intValue();
````

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

Java proporciona varias interfaces funcionales en java.util.function para los casos más comunes. La más general es Function<T, R>, pero existen muchas especializadas.
Algunas de las más usadas son Consumer<T>, Supplier<T>, Predicate<T>, UnaryOperator<T> y BiFunction<T, U, R>. Estas interfaces evitan la necesidad de definir tipos propios en la mayoría de los casos.
Su diseño sigue patrones funcionales ampliamente utilizados, facilitando la interoperabilidad con streams y APIs modernas.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

El método forEach permite expresar recorridos de colecciones de forma declarativa, delegando el control de la iteración al framework. Esto reduce errores comunes asociados a índices y bucles manuales.
La acción a realizar sobre cada elemento se define como una función, lo que refuerza la separación entre estructura de datos y comportamiento.
````
lista.forEach(n -> {
    if (n > 0)
        System.out.println("Positivo
````

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

PECS significa Producer Extends, Consumer Super, y es una regla que indica cómo usar comodines en genéricos. Si una estructura produce valores, se usa extends; si los consume, se usa super.
forEach consume elementos de tipo T, por lo que permite cualquier Consumer de un supertipo de T. Esto aumenta la flexibilidad sin perder seguridad de tipos.
Aplicado a transformar, permitir Function<? super T, ? extends R> haría el método más reutilizable ante jerarquías de tipos.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

Las referencias a métodos permiten reutilizar métodos existentes como funciones, evitando lambdas triviales. Esto mejora la legibilidad y reduce código repetitivo.
Tanto JavaScript como Java permiten capturar métodos de instancia en variables y ejecutarlos posteriormente como funciones.
````
let p = new Persona("Ana");
let saludo = p.saludar.bind(p);
saludo();
````
````
Per
sona p = new Persona("Ana");
Runnable saludo = p::saludar;
saludo.run();
````

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

Java permite referencias a métodos estáticos (Clase::metodoStatic), a constructores (Clase::new), a métodos de una instancia concreta (obj::metodo) y a métodos de cualquier instancia (Clase::metodo).
Cada tipo se utiliza según el contexto y la interfaz funcional esperada. Esta flexibilidad evita código redundante y favorece la reutilización de lógica existente.
Las referencias a métodos son conceptualmente equivalentes a lambdas simples, pero más expresivas cuando el método ya existe.

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

La ordenación con lambdas permite definir criterios de comparación de forma local y concisa. En la versión manual, se implementa directamente la comparación dentro de la expresión lambda.
El uso de Comparator encadenado mejora la legibilidad y reduce errores, aprovechando métodos auxiliares ya probados.
````
Collections.sort(personas, (p1, p2) -> {
    int c = Integer.compare(p1.getEdad(), p2.getEdad());
    return c != 0 ? c : p1.getNombre().compareTo(p2.getNombre());
});
````
````
Collections.sort(personas,
    Comparator.comparing(Persona::getEdad)
              .thenComparing(Persona::getNombre));
````
