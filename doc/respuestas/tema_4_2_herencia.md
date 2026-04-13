# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

En orientación a objetos, la herencia es un mecanismo que permite definir una clase nueva (subclase) a partir de otra ya existente (superclase). La subclase hereda los atributos y métodos de la superclase, estableciendo una relación conceptual del tipo “A es-un B”. Esto significa que cualquier instancia de la subclase puede considerarse también una instancia de la superclase, manteniendo coherencia semántica.
La primera implicación importante es la compatibilidad de tipos. Dado que una subclase es también un tipo de su superclase, se puede utilizar una referencia del tipo base para apuntar a objetos de cualquiera de sus subtipos. Esto permite escribir código genérico que trabaje con la superclase sin conocer los tipos concretos, facilitando la reutilización y extensibilidad del código.
La segunda implicación es la herencia de estado y comportamiento. La subclase incorpora los atributos (estado) y métodos (comportamiento) definidos en la superclase, sin necesidad de redefinirlos. Esto evita duplicación de código y asegura que todos los subtipos compartan una base común coherente.
```
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

Soldado[] ejercito = {
    new Artillero("Carlos", 5),
    new Zapador("Luis", 3),
    new Artillero("Ana", 10)
};

for (Soldado s : ejercito) {
    s.saludar();
}
```

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

Al crear una instancia de una subclase en Java, se ejecutan varios constructores, uno por cada nivel de la jerarquía de herencia. Siempre se ejecuta primero el constructor de la superclase y, a continuación, el de la subclase. Este orden garantiza que el estado heredado esté correctamente inicializado antes de añadir el estado específico de la subclase.
La palabra clave super dentro de un constructor se utiliza para invocar explícitamente a un constructor de la clase base. Esta llamada debe ser la primera instrucción del constructor de la subclase. Su finalidad es inicializar la parte del objeto que corresponde a la superclase, especialmente cuando esta requiere parámetros.
Si la clase base no tiene un constructor sin parámetros visible, es obligatorio llamar a super de forma explícita pasando los argumentos adecuados. En caso contrario, el código no compilará. Esto obliga a que la subclase sea consciente del contrato de construcción definido por la superclase.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

Los atributos privados definidos en la superclase sí forman parte de una instancia concreta de la subclase en memoria. Un objeto de una subclase contiene físicamente todos los campos definidos en la superclase, además de los suyos propios, aunque pertenezcan a distintos niveles de la jerarquía.
Sin embargo, que esos atributos existan en memoria no implica que puedan usarse directamente desde el código de la subclase. El modificador private impide el acceso directo desde cualquier clase distinta de aquella en la que se definió, incluidas las subclases. Esto mantiene la encapsulación del estado interno.
En el ejemplo de Soldado, el atributo nombre existe dentro de cada Artillero o Zapador, pero no puede accederse directamente. El uso correcto sería a través de métodos públicos o protegidos definidos en Soldado, como saludar(), garantizando así un acceso controlado.
## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

La compatibilidad de tipos permite que el código que opera sobre la superclase no necesite modificarse cuando se añaden nuevos subtipos. Esto mejora notablemente la extensibilidad, ya que se pueden introducir nuevas funcionalidades ampliando la jerarquía en lugar de modificar código existente.
Al añadir un nuevo tipo de Soldado, basta con que herede de la clase base y cumpla su contrato público. El código que recorre un array de Soldado y llama a saludar() seguirá funcionando sin cambios, ya que confía únicamente en el comportamiento común.
Esto reduce el riesgo de errores y facilita el mantenimiento, dado que el código ya probado no necesita ser alterado. La herencia, usada correctamente, permite extender sistemas sin romper funcionalidades previas.
```
class Medico extends Soldado {
    public Medico(String nombre) {
        super(nombre);
    }
}

// El código de recorrido no cambia
Soldado s = new Medico("Elena");
s.saludar();
```

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

En Java es posible que una referencia del supertipo apunte a un objeto real de un subtipo. Esto es un comportamiento normal y seguro, ya que el objeto cumple el contrato definido por la superclase. A este proceso se le denomina upcasting y ocurre de forma implícita.
Sin embargo, con una referencia del supertipo solo se pueden invocar métodos declarados en ese supertipo. Para acceder a métodos específicos del subtipo, es necesario realizar un downcasting, que consiste en convertir explícitamente la referencia al tipo concreto. Este proceso no es seguro por defecto y puede provocar errores en tiempo de ejecución.
El operador instanceof permite comprobar el tipo real del objeto antes de realizar un downcasting. De esta forma se evita una excepción y se programa de forma defensiva.
```
for (Soldado s : ejercito) {
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s;
        System.out.println("Cohetes: " + a.getCohetes());
    }
}
```

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El acceso protegido (protected) permite que un atributo o método sea accesible desde la propia clase, desde sus subclases y desde otras clases del mismo paquete. Se sitúa, por tanto, entre private y public en cuanto a nivel de visibilidad.
En Java, se implementa mediante el modificador protected. Este tipo de acceso es útil cuando se desea que las subclases puedan reutilizar o extender cierto comportamiento sin hacer público el estado interno para todo el mundo.
En el ejemplo, hacer que el nombre del Soldado sea protegido permite que el Zapador lo utilice en su lógica interna sin romper completamente la encapsulación.
```
class Soldado {
    protected String nombre;
}

class Zapador extends Soldado {
    public void ponerMina() {
        System.out.println(nombre + " ha puesto una mina");
    }
}
```

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

En muchos lenguajes orientados a objetos existe una clase base común de la que heredan todas las demás clases, aunque esto no ocurre en todos los lenguajes. La existencia de una raíz común permite definir comportamientos universales para todos los objetos.
En Java, todas las clases heredan directa o indirectamente de la clase Object. Esto ocurre incluso si no se indica explícitamente en la declaración de la clase. Object define métodos comunes como toString(), equals() o hashCode().
Gracias a esta clase base común, Java puede tratar cualquier instancia como un Object, lo que facilita mecanismos como colecciones genéricas, reflexión y manejo uniforme de objetos.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

La herencia múltiple se produce cuando una clase hereda directamente de más de una clase base. Este mecanismo permite reutilizar comportamiento de varias fuentes, pero introduce problemas como conflictos de nombres o ambigüedades en la resolución de métodos.
Java no permite herencia múltiple de clases. Una clase solo puede extender (extends) a una única clase. Esta decisión de diseño evita muchos problemas de complejidad y ambigüedad que se dan en otros lenguajes.
No obstante, Java ofrece una alternativa mediante interfaces, que permiten heredar múltiples contratos de comportamiento sin estado, reduciendo así los riesgos asociados a la herencia múltiple tradicional.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

En Java, las excepciones son objetos que heredan de Throwable. Crear una excepción personalizada implica definir una nueva clase que extienda de RuntimeException si se desea que sea no controlada, es decir, que no obligue a usar try-catch.
Es posible componer la excepción con otros objetos para aportar contexto adicional al error. En este caso, la excepción incluye una referencia a un Usuario que causó el problema, facilitando el diagnóstico.
También es posible sobrecargar el constructor para aceptar una causa (Throwable) y encadenar excepciones, lo cual es una práctica común en Java.
```
class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super(causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```
## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

La herencia no debe utilizarse únicamente como un mecanismo de reutilización de código, ya que establece una relación fuerte y permanente entre clases. Esta relación implica que la subclase depende directamente de la implementación de la superclase.
Si el único objetivo es compartir código, la herencia puede introducir rigidez innecesaria y hacer que cambios en la clase base afecten a todas las subclases de forma no deseada. Esto complica el mantenimiento y la evolución del sistema.
Por tanto, la herencia debe reservarse para relaciones conceptuales claras del tipo “es-un”, y no como una simple herramienta de ahorro de líneas de código.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

La composición consiste en construir clases a partir de otras, manteniéndolas como atributos internos en lugar de heredarlas. Este enfoque permite reutilizar funcionalidad sin establecer una relación jerárquica rígida.
Se favorece la composición porque ofrece mayor flexibilidad: los componentes pueden cambiarse, combinarse o evolucionar de forma independiente. Además, reduce el acoplamiento entre clases, lo que simplifica las modificaciones futuras.
En general, la composición permite diseñar sistemas más modulares y reutilizables, especialmente en escenarios donde no existe una relación clara de especialización.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

Se dice que la herencia rompe la encapsulación porque la subclase depende de detalles internos de la superclase. Aunque esos detalles no sean públicos, cualquier cambio en su comportamiento puede afectar indirectamente a las subclases.
Esto significa que la implementación de la superclase deja de ser completamente oculta, ya que las subclases asumen cierto funcionamiento interno. Como resultado, la superclase se vuelve más difícil de modificar sin riesgo.
La composición, en cambio, interactúa con los objetos a través de interfaces bien definidas, preservando mejor la encapsulación y reduciendo dependencias implícitas.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

Una primera opción consiste en usar herencia, definiendo una superclase Persona que contenga los datos comunes, como DNI y nombre. Las clases Estudiante y Trabajador heredan de esta clase base, reutilizando directamente esos atributos y su inicialización.
La alternativa mediante composición consiste en crear una clase DatosPersonales que encapsule DNI y nombre. Tanto Estudiante como Trabajador reciben una instancia de dicha clase en su constructor y la almacenan internamente.
La segunda opción evita una relación jerárquica rígida y permite reutilizar los datos personales en otros contextos distintos sin necesidad de heredar, ofreciendo mayor flexibilidad de diseño.
```
// Herencia
class Persona {
    String dni;
    String nombre;
}

class Estudiante extends Persona {}
class Trabajador extends Persona {}

// Composición
class DatosPersonales {
    String dni;
    String nombre;
}

class EstudianteC {
    DatosPersonales datos;
    public EstudianteC(DatosPersonales datos) {
        this.datos = datos;
    }
}

class TrabajadorC {
    DatosPersonales datos;
    public TrabajadorC(DatosPersonales datos) {
        this.datos = datos;
    }
}
```
