# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

La programación orientada a objetos se basa en cuatro características fundamentales que permiten organizar el software de forma más estructurada y cercana a la realidad. Estas características facilitan la reutilización del código, la modularidad y el mantenimiento de los programas a medida que crecen en tamaño y complejidad.

La **encapsulación** consiste en agrupar los datos y las operaciones que los manipulan dentro de una misma entidad, llamada clase. Además, permite controlar el acceso a dichos datos, evitando que sean modificados directamente desde fuera y protegiendo así el estado interno del objeto.

La **abstracción** permite centrarse en los aspectos esenciales de una entidad, ocultando los detalles de implementación. Se define qué puede hacer un objeto sin necesidad de conocer cómo lo hace internamente, lo que simplifica el diseño y el uso del software.

La **herencia** permite crear nuevas clases a partir de otras ya existentes, heredando sus atributos y métodos. Por último, el **polimorfismo** permite que una misma operación se comporte de manera diferente según el objeto que la ejecute, aumentando la flexibilidad del código.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Existen numerosos lenguajes de programación que soportan la programación orientada a objetos y que se utilizan ampliamente en distintos ámbitos del desarrollo de software. Estos lenguajes incorporan mecanismos como clases, objetos, herencia y polimorfismo.

Uno de los lenguajes más conocidos es **Java**, que está fuertemente orientado a objetos y se basa casi exclusivamente en el uso de clases y objetos. Es muy utilizado en aplicaciones empresariales y en el desarrollo de aplicaciones multiplataforma.

Otro lenguaje ampliamente usado es **C++**, que extiende el lenguaje C incorporando características de la programación orientada a objetos, permitiendo combinar programación estructurada y orientada a objetos. **Python** también soporta la POO de forma flexible y sencilla, y es muy utilizado en educación y ciencia de datos. Por último, **C#** es un lenguaje orientado a objetos desarrollado por Microsoft, empleado en aplicaciones de escritorio, web y videojuegos.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la programación estructurada? y, todavía mejor, ¿Qué es la programación modular?

La programación estructurada es un paradigma que propone organizar los programas utilizando estructuras de control claras, como secuencias, condicionales y bucles. Su objetivo principal es mejorar la legibilidad y fiabilidad del código, evitando saltos incontrolados como el uso excesivo de `goto`.

En este paradigma, el programa se divide en funciones o procedimientos, cada uno encargado de realizar una tarea concreta. Lenguajes como C son un ejemplo claro de programación estructurada, donde el flujo del programa está bien definido y jerarquizado.

La programación modular va un paso más allá y consiste en dividir el programa en módulos independientes, cada uno con una responsabilidad bien definida. Un módulo puede contener funciones y datos relacionados entre sí, y se comunica con otros módulos mediante interfaces claras.

Este enfoque facilita el mantenimiento, la reutilización del código y el trabajo en equipo, ya que cada módulo puede desarrollarse y probarse de forma independiente antes de integrarse en el programa completo.


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Un objeto en programación orientada a objetos se define mediante tres elementos fundamentales que permiten diferenciarlo y describir su funcionamiento dentro de un programa. Estos elementos permiten modelar entidades del mundo real de forma precisa.

El primer elemento es la **identidad**, que permite distinguir un objeto de otro, incluso aunque tengan los mismos valores en sus atributos. Cada objeto ocupa una posición única en memoria y se trata como una entidad independiente.

El segundo elemento es el **estado**, que viene determinado por los valores actuales de sus atributos. Este estado puede cambiar a lo largo de la ejecución del programa. El tercer elemento es el **comportamiento**, que corresponde al conjunto de operaciones o métodos que el objeto puede realizar.


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una clase es una plantilla o definición que describe cómo serán los objetos de un determinado tipo. En ella se especifican los atributos que tendrán los objetos y los métodos que definen su comportamiento, pero no representa un elemento concreto del programa.

Una clase no es lo mismo que un objeto. Mientras que la clase es una definición abstracta, el objeto es una entidad concreta creada a partir de esa definición, con valores reales asignados a sus atributos y capaz de ejecutar métodos.

Una instancia es precisamente un objeto concreto creado a partir de una clase. Instanciar una clase significa reservar memoria y crear un objeto basado en su estructura. Aunque la mayoría de los lenguajes orientados a objetos utilizan clases, existen algunos lenguajes, como los basados en prototipos, que no usan clases de forma tradicional.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la recolección de basura?

En muchos lenguajes orientados a objetos, los objetos se almacenan en una zona de memoria llamada *heap* o montón. Las variables del programa suelen contener referencias a estos objetos, no el objeto en sí, lo que permite un acceso flexible y dinámico a la memoria.

Sin embargo, el almacenamiento y la gestión de memoria no es igual en todos los lenguajes. En algunos lenguajes, como C++, los objetos pueden almacenarse tanto en la pila como en el heap, y la liberación de memoria suele ser responsabilidad del programador. En otros lenguajes, la gestión es automática.

La recolección de basura es un mecanismo que se encarga de liberar automáticamente la memoria ocupada por objetos que ya no se utilizan. Este proceso reduce errores comunes como fugas de memoria y accesos inválidos, simplificando el desarrollo de programas orientados a objetos.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 


Un método es una función asociada a una clase que define una operación o comportamiento que pueden realizar los objetos de dicha clase. Desde el punto de vista de alguien que conoce C/C++, un método puede entenderse como una función que, además de recibir parámetros explícitos, opera sobre los datos internos (atributos) del objeto al que pertenece. Los métodos permiten manipular el estado del objeto y definir cómo interactúa con otros objetos o con el resto del programa.

En programación orientada a objetos, los métodos forman parte esencial de la definición de una clase, junto con los atributos. Mientras que los atributos representan el estado, los métodos representan el comportamiento. La invocación de un método se realiza sobre un objeto concreto, lo que implica que el mismo método puede producir efectos distintos según el estado del objeto que lo ejecuta.

La sobrecarga de métodos consiste en definir varios métodos con el mismo nombre dentro de una misma clase, pero con diferentes listas de parámetros (distinto número de parámetros o distintos tipos). El compilador decide qué método ejecutar en función de los argumentos utilizados en la llamada. Este mecanismo no existía en el C clásico, donde cada función debía tener un nombre único.

La sobrecarga de métodos mejora la legibilidad y la expresividad del código, ya que permite usar un mismo nombre para operaciones conceptualmente similares. De este modo, se facilita el uso de las clases y se reduce la necesidad de crear nombres artificialmente distintos para funciones que realizan la misma acción con diferentes datos.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

Ejemplo minimo y claro:

public class Punto {
    // Atributos con visibilidad por defecto (package-private)
    double x;
    double y;

    // Método que calcula la distancia al origen (0,0)
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

Ejemplo de uso:

public class Main {
    public static void main(String[] args) {
        Punto p = new Punto();   // Creación de una instancia
        p.x = 3;
        p.y = 4;

        double distancia = p.calculaDistanciaAOrigen();
        System.out.println(distancia);
    }
}



## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada de un programa en Java es el método `main`, cuya firma es `public static void main(String[] args)`. Este método es el primero que se ejecuta cuando se lanza un programa Java desde la línea de comandos. La máquina virtual de Java busca exactamente este método para comenzar la ejecución.

La palabra clave `static` indica que un método o atributo pertenece a la clase y no a un objeto concreto. Esto significa que puede utilizarse sin necesidad de crear una instancia de la clase. En el caso del método `main`, se utiliza `static` porque aún no existe ningún objeto cuando el programa comienza a ejecutarse.

El modificador `static` no se emplea únicamente en el método `main`. Puede utilizarse también en métodos auxiliares, constantes o variables compartidas por todas las instancias de una clase. Cuando se combina con `final`, suele indicar que el valor no puede modificarse, como ocurre habitualmente con constantes.

---

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde línea de comandos? ¿Java es compilado? ¿Qué es la máquina virtual? ¿Qué es el byte-code y los ficheros `.class`?

Un programa Java se compila desde la línea de comandos utilizando el compilador `javac`. Por ejemplo, al ejecutar `javac Main.java`, se traduce el código fuente a un fichero con extensión `.class`. Posteriormente, el programa se ejecuta con el comando `java Main`, sin indicar la extensión del archivo.

Java es un lenguaje compilado, pero su proceso de compilación es distinto al de C o C++. En lugar de generar código máquina específico para un sistema operativo, el compilador genera un código intermedio llamado *byte-code*.

Este byte-code se ejecuta sobre la máquina virtual de Java (JVM), que actúa como una capa intermedia entre el programa y el sistema operativo. Gracias a este diseño, un mismo programa Java puede ejecutarse en distintos sistemas sin recompilar, siempre que exista una JVM compatible.

Los ficheros `.class` contienen precisamente ese byte-code. Cada clase del programa se traduce a un archivo `.class`, que es interpretado o compilado dinámicamente por la máquina virtual en tiempo de ejecución.

---

## 11. En el código anterior de la clase `Punto`, ¿qué es `new`? ¿Qué es un constructor? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

La palabra clave `new` se utiliza en Java para crear un objeto, es decir, para reservar memoria y generar una instancia de una clase. Al usar `new`, se crea un objeto en memoria y se devuelve una referencia a dicho objeto.

Un constructor es un método especial de una clase que se ejecuta automáticamente cuando se crea un objeto con `new`. Su función principal es inicializar los atributos del objeto. Un constructor tiene el mismo nombre que la clase y no devuelve ningún valor.

Si no se define ningún constructor, Java proporciona uno por defecto sin parámetros. Sin embargo, es habitual definir constructores propios para inicializar correctamente los objetos desde el momento de su creación.

Ejemplo de constructor en una clase `Empleado`:
```java
public class Empleado {
    String dni;
    String nombre;
    String apellidos;

    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}
```

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

La referencia this se utiliza para hacer referencia al objeto actual, es decir, al objeto que está ejecutando un método en un momento determinado. Permite acceder de forma explícita a los atributos y métodos de dicho objeto.

El uso de this es especialmente útil cuando los parámetros de un método tienen el mismo nombre que los atributos de la clase, ya que permite distinguir claramente entre ambos.

El nombre de esta referencia no es igual en todos los lenguajes. En C++ también se utiliza this, mientras que en otros lenguajes orientados a objetos puede recibir nombres distintos o no ser accesible explícitamente.
```java
public class Punto {
    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
}
```

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

Un método puede recibir como parámetro otro objeto de la misma clase. En este caso, el método distanciaA recibe un objeto Punto y calcula la distancia entre el objeto actual y el punto proporcionado.

El cálculo se realiza accediendo a los atributos de ambos objetos, utilizando this para referirse al objeto que ejecuta el método y el parámetro para acceder al objeto recibido.

Este tipo de métodos permite definir relaciones entre objetos y realizar operaciones que dependen del estado de más de una instancia.
```java
double distanciaA(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
```

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En Java, cuando se pasa un objeto como parámetro a un método, se pasa una copia de la referencia al objeto. Esto implica que el método puede modificar el estado del objeto original, ya que ambas referencias apuntan al mismo objeto en memoria.

Sin embargo, si dentro del método se reasigna la referencia, ese cambio no afecta a la referencia original fuera del método. Solo las modificaciones sobre los atributos del objeto son visibles externamente.

En el caso de los tipos primitivos como int, double o char, el paso de parámetros es por copia del valor. Por tanto, cualquier modificación realizada dentro del método no afecta a la variable original.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

El método toString() es un método definido en la clase base Object, de la cual heredan todas las clases en Java. Su finalidad es devolver una representación en forma de texto del objeto.

Por defecto, este método devuelve una cadena poco informativa, por lo que es habitual redefinirlo para mostrar información relevante del estado del objeto.

Métodos con una finalidad similar existen en otros lenguajes orientados a objetos, aunque con nombres o mecanismos distintos.
```
@Override
public String toString() {
    return "Punto(" + x + ", " + y + ")";
}
```


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


Una clase y un struct en C comparten similitudes, ya que ambos permiten agrupar datos relacionados bajo un mismo tipo. Desde este punto de vista, una clase puede verse como una evolución del concepto de estructura.

Sin embargo, un struct en C solo contiene datos, mientras que una clase también contiene comportamiento en forma de métodos. Además, una clase permite controlar el acceso a sus miembros y asociar directamente las funciones a los datos.

A un struct en C le faltan mecanismos como encapsulación, constructores, herencia y polimorfismo. Tampoco existe una referencia implícita al objeto, como ocurre con this.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

En C, una forma de emular una clase consiste en definir un struct para los datos y funciones externas que operen sobre dicho struct. Estas funciones reciben explícitamente un puntero a la estructura.

La referencia implícita this desaparece y se sustituye por un parámetro que representa el objeto sobre el que se opera. El programador debe pasar manualmente dicho puntero a cada función.

Este enfoque permite aproximarse al modelo de objetos, pero carece de las garantías y mecanismos automáticos que ofrece la programación orientada a objetos.
```
#include <math.h>

struct Punto {
    double x;
    double y;
};

double calculaDistanciaAOrigen(struct Punto *p) {
    return sqrt(p->x * p->x + p->y * p->y);
}
```
