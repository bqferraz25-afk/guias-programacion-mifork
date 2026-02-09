# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

En Programación Orientada a Objetos, la encapsulación busca agrupar en una misma entidad (la clase) tanto los datos como las operaciones que trabajan sobre esos datos. De este modo, un objeto se presenta como una unidad coherente que combina estado y comportamiento, en contraste con el estilo procedural de C, donde los datos y las funciones suelen estar separados.

La ocultación de información es un principio complementario que persigue limitar el acceso directo al estado interno del objeto. Esto implica que los detalles de implementación no deben ser visibles ni modificables desde fuera de la clase, y que la interacción con el objeto debe realizarse únicamente a través de métodos bien definidos.

Entre las ventajas de la ocultación de información se encuentran una mayor robustez del programa, al evitar modificaciones inconsistentes del estado interno; una mejor mantenibilidad, ya que los cambios internos no afectan al código cliente; y una reducción de errores, al restringirse los usos incorrectos de los datos.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La interfaz pública de una clase está formada por el conjunto de métodos y atributos que son accesibles desde fuera de la propia clase. Constituye el “contrato” que la clase ofrece a otros componentes del programa para interactuar con sus objetos.

Esta interfaz define qué operaciones pueden realizarse sobre un objeto, sin revelar cómo están implementadas internamente. Desde el punto de vista del usuario de la clase, solo importa qué hace el objeto y cómo se le puede pedir que lo haga, no cómo se logra internamente.

La relación con la ocultación de información es directa: cuanto más clara y reducida sea la interfaz pública, mayor será el grado de ocultación. Los detalles internos permanecen protegidos, lo que permite cambiar la implementación sin alterar el código que utiliza la clase.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

La interfaz pública debe diseñarse con cuidado porque es la parte de la clase de la que dependerá el resto del programa. Cualquier otro módulo que use la clase se basará en esa interfaz, asumiendo que se mantendrá estable en el tiempo.

Una interfaz mal diseñada puede forzar usos incorrectos del objeto o exponer detalles que no deberían ser visibles. Esto puede derivar en errores difíciles de detectar y en una mayor dependencia entre componentes del sistema.

Cambiar la interfaz pública no suele ser fácil, ya que implica modificar todo el código que hace uso de ella. Por este motivo, es preferible dedicar tiempo al diseño inicial y minimizar cambios posteriores, manteniendo la implementación interna como el principal punto de evolución.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las invariantes de clase son condiciones que deben cumplirse siempre para que un objeto se considere válido. Estas condiciones afectan al estado interno del objeto y deben mantenerse tras la ejecución de cualquier método público.

Por ejemplo, una clase puede requerir que ciertos valores numéricos estén siempre dentro de un rango determinado. Si esta condición se viola, el objeto entra en un estado inconsistente y su comportamiento deja de ser fiable.

La ocultación de información ayuda a preservar las invariantes porque impide que el estado interno sea modificado directamente desde el exterior. Al centralizar las modificaciones en métodos controlados, se puede garantizar que las invariantes se comprueben y mantengan correctamente.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

La clase Punto puede representar un punto en el plano mediante dos coordenadas x e y. Para aplicar la ocultación de información, estas coordenadas deben declararse como privadas, de modo que no puedan modificarse directamente desde fuera de la clase.

La interfaz pública de la clase estaría formada por el constructor y por los métodos públicos, como el que calcula la distancia al origen. Estos métodos permiten interactuar con el objeto sin acceder directamente a sus atributos internos.

El modificador public indica que un miembro es accesible desde cualquier otra clase, mientras que private restringe el acceso únicamente al interior de la propia clase. Esta distinción es fundamental para implementar correctamente la encapsulación.
```java
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores de visibilidad como public y private pueden aplicarse a clases, atributos, métodos y constructores. Su función es controlar desde dónde se puede acceder a cada uno de estos elementos.

Las clases pueden ser públicas o tener visibilidad por defecto, pero no pueden ser privadas a nivel superior. En cambio, los atributos y métodos sí pueden declararse como privados para ocultar su uso externo.

Esta flexibilidad permite diseñar clases con una interfaz pública clara y una implementación interna protegida, reforzando el principio de ocultación de información.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Además de la visibilidad pública y privada, en muchos lenguajes orientados a objetos existen niveles intermedios de visibilidad. Estos niveles permiten un control más fino sobre quién puede acceder a los miembros de una clase.

En Java existen cuatro niveles: public, private, protected y la visibilidad por defecto (package-private). El modificador protected permite el acceso desde clases del mismo paquete o desde subclases.

En otros lenguajes, como C++, existen visibilidades similares, aunque con diferencias semánticas. La idea común es proporcionar distintos grados de acceso para equilibrar reutilización, seguridad y encapsulación.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

Los miembros de instancia privados están ocultos para otras clases, pero no para otras instancias de la misma clase. Esto significa que un método de una clase puede acceder a los atributos privados de otro objeto de la misma clase.

Esta característica es necesaria para implementar operaciones que relacionan varios objetos del mismo tipo, como el cálculo de distancias entre puntos. Aunque los atributos sean privados, la clase actúa como un “ámbito de confianza”.

En el ejemplo siguiente, el método puede acceder a x e y del objeto otro sin violar la encapsulación:
```java
public double calcularDistanciaAPunto(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
```

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Los métodos getter y setter son métodos públicos que permiten, respectivamente, acceder y modificar atributos privados de una clase. Actúan como intermediarios entre el exterior y el estado interno del objeto.

Un getter devuelve el valor de un atributo, mientras que un setter permite asignar un nuevo valor. A diferencia del acceso directo, estos métodos pueden incluir validaciones o transformaciones.

Su uso es habitual en POO porque permite mantener la ocultación de información y preservar las invariantes de clase, incluso cuando se permite modificar el estado del objeto.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

Cuando se habla de seguridad en el contexto de la ocultación de información, no se hace referencia a la seguridad frente a ataques externos o hackeo. El término se utiliza en un sentido lógico y estructural.

La seguridad aquí implica proteger el estado interno de los objetos frente a usos incorrectos dentro del propio programa. Se trata de evitar que otros módulos modifiquen datos de forma indebida o inconsistente.

Por tanto, la ocultación de información mejora la fiabilidad y la corrección del software, no la seguridad informática en un sentido criptográfico o de red.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

Un miembro de instancia pertenece a cada objeto creado a partir de una clase, por lo que cada instancia tiene su propia copia de ese miembro. En cambio, un miembro de clase es compartido por todas las instancias.

En Java, los miembros de clase se declaran usando la palabra clave static. Estos miembros existen independientemente de que se cree o no un objeto de la clase.

Los miembros de clase también pueden ocultarse utilizando modificadores de visibilidad como private. De este modo, se controla el acceso tanto a datos compartidos como a datos individuales.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, tiene sentido que los constructores sean privados en determinados diseños. Un constructor privado impide que se creen objetos de la clase desde fuera de ella.

Esta técnica se utiliza, por ejemplo, en patrones de diseño como el Singleton o en clases que solo ofrecen métodos estáticos o factorías para crear objetos.

Al controlar la creación de instancias, se puede asegurar que los objetos se construyen siempre en un estado válido y coherente con las invariantes de clase.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

En Java, los miembros de clase se indican mediante la palabra clave static. Estos miembros pertenecen a la clase en sí y no a una instancia concreta.

En el caso de la clase Punto, se pueden usar miembros estáticos para almacenar información global, como los valores máximos de x e y creados hasta el momento.
```java
public class Punto {
    private double x;
    private double y;
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }
}
```

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

Un método factoría es un método que se encarga de crear y devolver instancias de una clase. Suele declararse como static porque no depende de una instancia previa.

En este caso, el método recibe dos coordenadas, las redondea y devuelve un nuevo objeto Punto. El uso de static es necesario para poder invocar el método sin un objeto existente.
```java
public static Punto crearRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}
```

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

La implementación interna de una clase puede cambiar sin afectar al código cliente si la interfaz pública se mantiene. Este es uno de los principales beneficios de la encapsulación.

En lugar de almacenar x e y como atributos separados, se puede usar un array privado de dos posiciones. Los métodos públicos seguirán ofreciendo el mismo comportamiento.

Esto demuestra que la ocultación de información permite modificar la estructura interna sin romper la compatibilidad con el resto del programa.
```java
private double[] coords = new double[2];
```

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

Aunque un atributo tenga un getter y un setter públicos, no es recomendable declararlo directamente como público. El uso de métodos permite introducir validaciones o lógica adicional en el futuro.

La convención más habitual en POO es declarar los atributos como privados y acceder a ellos mediante métodos. Esto proporciona mayor flexibilidad y control sobre el estado del objeto.

Esta práctica está directamente relacionada con las invariantes de clase, ya que los métodos pueden asegurar que los cambios no violen dichas invariantes.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Una clase es inmutable cuando, una vez creado un objeto, su estado no puede cambiar. Esto implica que no existen métodos que modifiquen los atributos internos tras la construcción.

Un método modificador es aquel que cambia el estado del objeto. No todos los métodos modificadores son setters, aunque los setters son el ejemplo más común.

Las clases inmutables tienen ventajas como una mayor simplicidad, ausencia de efectos colaterales y facilidad para razonar sobre el programa, especialmente en contextos concurrentes.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No es recomendable incluir setters de forma automática o por convención. Cada setter expone una posibilidad de modificar el estado del objeto.

Incluir setters innecesarios puede debilitar la encapsulación y hacer más difícil mantener las invariantes de clase. Por ello, deben añadirse solo cuando realmente sean necesarios.

Un buen diseño en POO prioriza la coherencia del objeto frente a la comodidad de acceso.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

La clase String en Java es inmutable. Esto significa que una vez creada una cadena, su contenido no puede modificarse.

Cuando se concatenan dos cadenas, en realidad se crea un nuevo objeto String. El objeto original permanece sin cambios.

Si se necesita construir una cadena muy larga mediante múltiples concatenaciones, se recomienda usar StringBuilder, que es mutable y más eficiente en ese contexto.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

En POO, los objetos pueden compararse por identidad o por contenido. La identidad indica si dos referencias apuntan al mismo objeto, mientras que el contenido compara el estado interno.

En Java, el método equals se utiliza para comparar el contenido lógico de los objetos. Por defecto, equals compara la identidad, como el operador ==.

Las cadenas en Java deben compararse siempre con equals, ya que usar == solo comprueba si se trata del mismo objeto, no si tienen el mismo contenido.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases wrapper son clases que encapsulan tipos primitivos dentro de objetos. En Java, ejemplos de estas clases son Integer, Double o Boolean.

Este proceso puede realizarse automáticamente mediante autoboxing, donde el compilador convierte entre tipos primitivos y sus wrappers sin intervención explícita.

Las clases wrapper permiten tratar valores primitivos como objetos, lo que facilita su uso en colecciones y en contextos orientados a objetos. No todos los lenguajes necesitan wrappers, ya que algunos no distinguen entre tipos primitivos y objetos.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Un tipo de dato enumerado representa un conjunto finito y cerrado de valores posibles. Se utiliza cuando una variable solo puede tomar uno de varios valores predefinidos.

En Java, un enumerado es una clase especial. Esto permite que cada valor tenga atributos, métodos y comportamiento propio.

Desde el punto de vista de la encapsulación, los enumerados en Java permiten agrupar datos y lógica relacionada, evitando valores inválidos y mejorando la claridad del código.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

Un enumerado en Java puede definirse con atributos privados, constructores y métodos, lo que refuerza la encapsulación. Cada mes se define como una instancia única con sus propios datos.

Los métodos permiten consultar información derivada, como el número de días o la estación, sin exponer los detalles internos. El parámetro del hemisferio permite adaptar el comportamiento.
```java
public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4),
    MAYO(31, 5), JUNIO(30, 6), JULIO(31, 7), AGOSTO(31, 8),
    SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private int dias;
    private int ordinal;

    private Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinal() {
        return ordinal;
    }

    public boolean esDePrimavera(boolean hemisferioNorte) {
        return hemisferioNorte ? (this == MARZO || this == ABRIL || this == MAYO)
                               : (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE);
    }

    public boolean esDeVerano(boolean hemisferioNorte) {
        return hemisferioNorte ? (this == JUNIO || this == JULIO || this == AGOSTO)
                               : (this == DICIEMBRE || this == ENERO || this == FEBRERO);
    }

    public boolean esDeOtoño(boolean hemisferioNorte) {
        return hemisferioNorte ? (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE)
                               : (this == MARZO || this == ABRIL || this == MAYO);
    }

    public boolean esDeInvierno(boolean hemisferioNorte) {
        return hemisferioNorte ? (this == DICIEMBRE || this == ENERO || this == FEBRERO)
                               : (this == JUNIO || this == JULIO || this == AGOSTO);
    }
}
```
