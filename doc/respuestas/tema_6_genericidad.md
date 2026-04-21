# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

En lenguajes sin programación genérica nativa, una forma habitual de simular estructuras de datos “universales” ha sido usar punteros sin tipo concreto (void* en C) o una superclase común (Object en Java). La idea consiste en almacenar referencias genéricas, perdiendo información estática sobre el tipo real de los elementos.
En C, un array de void* puede almacenar direcciones de cualquier tipo de dato, ya que void* es compatible con cualquier puntero. El compilador no conoce el tipo real apuntado, por lo que el programador debe recordar qué hay almacenado en cada posición y hacer conversiones manuales al recuperarlo.
En Java, el papel de void* lo desempeña la clase Object, ya que todas las clases heredan de ella. Un array de Object permite almacenar instancias de cualquier clase, incluyendo tipos envoltorio como Integer o Double.
```
/* C */
void* array[10];
int x = 5;
double y = 3.14;

array[0] = &x;
array[1] = &y;
```
```
// Java
Object[] array = new Object[10];
array[0] = "Hola";
array[1] = 42; // autoboxing a Integer
```
## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

La programación genérica consiste en definir algoritmos y estructuras de datos independientes de los tipos concretos con los que trabajan, permitiendo reutilizar el mismo código para distintos tipos sin duplicación. El objetivo principal es aumentar la reutilización y mantener el chequeo de tipos en tiempo de compilación.
En este paradigma, el tipo se trata como un parámetro, de forma similar a cómo una función puede recibir valores como parámetros. Esto permite escribir una única implementación de una estructura de datos, como una lista o un vector, válida para múltiples tipos.
El ejemplo basado en void* o Object no es programación genérica en sentido estricto, sino una simulación manual. Aunque permite almacenar cualquier tipo de dato, el compilador no puede verificar que los usos posteriores sean correctos desde el punto de vista del tipo.
Por tanto, ese ejemplo puede considerarse un acercamiento rudimentario o previo a la programación genérica, pero no ofrece las garantías de seguridad y expresividad que caracterizan a este paradigma.
## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

El principal problema es la pérdida de información de tipo en tiempo de compilación. Al almacenar elementos como void* u Object, el compilador deja de saber qué tipo concreto hay en cada posición de la estructura de datos.
Esto obliga a realizar conversiones explícitas (casting) al recuperar los elementos. Dichas conversiones no se comprueban completamente en compilación, y si el programador se equivoca, el error se manifestará en tiempo de ejecución, normalmente como comportamiento indefinido en C o como ClassCastException en Java.
Además, este enfoque impide que el compilador detecte usos incorrectos, como intentar tratar un objeto como si fuera de otro tipo incompatible. El programador debe imponer disciplina manual, lo que aumenta el riesgo de errores y reduce la mantenibilidad del código.
En resumen, el uso de void* u Object sacrifica seguridad de tipos a cambio de flexibilidad, algo que la programación genérica moderna intenta evitar.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Los parámetros de tipo son identificadores que representan tipos desconocidos en tiempo de definición, pero que se concretan en tiempo de uso. Funcionan de manera análoga a los parámetros de una función, pero aplicados a clases, interfaces o métodos.
Mediante parámetros de tipo, es posible expresar que una estructura de datos trabaja con “un tipo cualquiera T”, manteniendo la coherencia interna: lo que se inserta como T se recupera como T. De este modo, el compilador puede verificar la corrección de los tipos sin conocerlos de antemano.
Estos parámetros permiten escribir código genérico sin perder información de tipo, a diferencia del uso de Object. El compilador impone restricciones y evita conversiones innecesarias o peligrosas, reforzando la seguridad.
En Java, los parámetros de tipo se introducen entre < >, por ejemplo List<T>, y constituyen la base del sistema de generics del lenguaje.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

En Java, los generics permiten definir colecciones parametrizadas por tipo, de forma que el compilador garantice que solo se almacenan elementos del tipo indicado. Al instanciar una List<String>, se impide añadir objetos que no sean String.
Durante el recorrido, cada elemento recuperado es directamente de tipo String, sin necesidad de conversiones manuales. Esto mejora la legibilidad y elimina posibles errores en tiempo de ejecución relacionados con el casting.
En C++, los templates permiten un comportamiento similar, pero con diferencias importantes en la implementación. Un std::vector<std::string> es un vector concretamente especializado para cadenas, generando código específico para ese tipo.
```
// Java
List<String> lista = new ArrayList<>();
lista.add("uno");
lista.add("dos");

for (String s : lista) {
    System.out.println(s.toUpperCase());
}
```
```
// C++
#include <vector>
#include <string>
#include <iostream>

std::vector<std::string> v;
v.push_back("uno");
v.push_back("dos");

for (const std::string& s : v) {
    std::cout << s << std::endl;
}
```
## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

El comportamiento del compilador frente a los parámetros de tipo depende del lenguaje. Aunque Java y C++ ofrecen programación genérica, internamente siguen estrategias muy diferentes.
En Java, los generics se implementan mediante type erasure (borrado de tipos). Esto significa que la información sobre los parámetros de tipo se utiliza para comprobar el código en compilación, pero se elimina después. En tiempo de ejecución, una List<String> y una List<Integer> son esencialmente la misma clase.
En C++, los templates se instancian en compilación. Para cada combinación concreta de tipos, el compilador genera una versión específica del código. Esto permite mantener el tipo exacto en todo momento, incluso en tiempo de ejecución.
En resumen, Java prioriza compatibilidad binaria y simplicidad en la máquina virtual, mientras que C++ prioriza rendimiento y expresividad mediante generación de código específico.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

Una clase genérica Par permite agrupar dos valores de tipos potencialmente distintos, sin perder información de tipo. Este patrón es habitual cuando se quiere devolver más de un resultado de una función sin crear una clase específica para cada caso.
Al usar parámetros de tipo, se consigue que cada componente del par tenga su tipo bien definido y verificado en compilación. El compilador garantiza que el primer valor se maneja siempre como T y el segundo como U.
Esto resulta especialmente útil en cálculos que devuelven resultados relacionados, como la media y la desviación típica de una colección de números.
```
public class Par<T, U> {
    private final T primero;
    private final U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() { return primero; }
    public U getSegundo() { return segundo; }
}

// Uso
public static Par<Double, Double> calcular(double[] datos) {
    // cálculo ficticio
    return new Par<>(10.0, 2.5);
}
```

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

Los métodos genéricos permiten definir parámetros de tipo localmente, sin necesidad de hacer genérica toda la clase. Esto es útil cuando la genericidad solo afecta a un comportamiento concreto.
Si se define el método usando Object, el compilador no puede garantizar que ambos argumentos sean del mismo tipo, ni evitar conversiones posteriores. El resultado debe convertirse manualmente al tipo esperado.
Con parámetros de tipo, el compilador fuerza que los dos argumentos sean del mismo tipo T y garantiza que el valor devuelto también lo es, evitando downcasting y errores en tiempo de ejecución.
```
// Versión sin genéricos
Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}

// Versión genérica
<T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}
```

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

En Java, los parámetros de tipo pueden restringirse mediante límites superiores (extends). Esto permite indicar que un tipo genérico debe pertenecer a una jerarquía concreta, por ejemplo Number, para poder utilizar ciertos métodos.
Una primera solución consiste en definir las coordenadas directamente como Number. Esto permite usar distintos tipos numéricos, pero se pierde información precisa sobre el tipo concreto, y todos los getters devuelven Number.
Una solución más robusta introduce un parámetro de tipo T extends Number, lo que permite que un Punto trabaje exactamente con Integer, Double, etc., manteniendo coherencia interna.
Tras la compilación, debido al type erasure, el tipo efectivo en Java será Number en ambos casos.
```
// Sin genéricos
class Punto {
    private final Number x, y;
}

// Con genéricos
class Punto<T extends Number> {
    private final T x, y;
}
```

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

Ambas soluciones permiten reutilizar la clase Punto para distintos tipos de números sin duplicación de código. Sin embargo, el nivel de chequeo de tipos que ofrecen es diferente.
En la solución sin genéricos, es posible crear un punto con una coordenada entera y otra real, ya que ambas son Number. Además, getX devuelve siempre un Number, obligando a convertir manualmente si se desea un tipo concreto.
En la solución con genéricos, no se permite mezclar tipos: un Punto<Integer> solo puede tener enteros, y un Punto<Double> solo reales. El método getX devuelve exactamente Integer o Double, reforzando la seguridad de tipos.
Por tanto, el uso de generics no solo evita errores, sino que documenta las intenciones del diseño de forma explícita.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

El problema del diseño original radica en que el método distanciaA acepta cualquier Punto, aunque solo tenga sentido calcular distancias entre puntos del mismo tipo dimensional. Esto obliga a usar instanceof y conversiones explícitas.
Mediante generics, se puede expresar esta restricción en la propia interfaz, indicando que un punto solo puede calcular la distancia a otro punto del mismo tipo. Esto se logra usando un parámetro de tipo recursivo.
De esta forma, el compilador garantiza la corrección y se elimina completamente la necesidad de comprobaciones dinámicas.
```
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}

public class Punto2D implements Punto<Punto2D> {
    // implementación segura
}
```

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

Aunque String es subtipo de Object, List<String> no es subtipo de List<Object>. Esto se debe a que los genéricos en Java son invariantes, para evitar inserciones incorrectas en tiempo de ejecución.
En cambio, los arrays sí son covariantes: String[] es subtipo de Object[]. Esto permite asignaciones convenientes, pero introduce riesgos, ya que se pueden insertar objetos incompatibles y provocar excepciones en tiempo de ejecución (ArrayStoreException).
Un tipo genérico es covariante si se conserva la relación de herencia, contravariante si se invierte, e invariante si no se permite ninguna. Java elige genéricos invariantes para maximizar la seguridad.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

Un wildcard (?) representa un tipo desconocido y permite relajar la invariancia de los genéricos de forma controlada. Java introduce wildcards acotados para expresar covarianza y contravarianza.
? extends T se usa cuando una estructura produce valores de tipo T o subtipos, pero no se van a insertar elementos. Es típico en algoritmos de lectura, como sumar números.
? super T se usa cuando se van a insertar elementos de tipo T, pero no importa el tipo concreto almacenado. Es habitual en algoritmos de escritura.
````
// (i) Lectura: covariante
double suma(List<? extends Number> lista) {
    double s = 0;
    for (Number n : lista) s += n.doubleValue();
    return s;
}

// (ii) Escritura: contravariante
void añadirEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
}
````
