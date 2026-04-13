# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El polimorfismo es un principio de la programación orientada a objetos que permite tratar objetos de distintos tipos concretos de forma uniforme a través de una referencia común, normalmente el tipo de una superclase o una interfaz. Su objetivo principal es permitir que un mismo mensaje (llamada a un método) produzca comportamientos distintos según el tipo real del objeto que recibe dicho mensaje.
Gracias al polimorfismo, el código se vuelve más flexible y extensible, ya que se puede escribir lógica general que no dependa de clases concretas. Esto resulta especialmente útil cuando se trabaja con jerarquías de herencia, donde diferentes subclases representan variaciones de un mismo concepto.
La sobreescritura de métodos es el mecanismo que permite al polimorfismo funcionar. Consiste en que una subclase redefine un método heredado de la superclase para proporcionar una implementación específica. Cuando el método se invoca a través de una referencia del tipo base, se ejecuta la versión correspondiente al tipo real del objeto.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La ligadura dinámica (o enlace tardío) es el proceso mediante el cual se decide en tiempo de ejecución qué implementación concreta de un método debe ejecutarse, en función del tipo real del objeto y no del tipo de la referencia. Este mecanismo es fundamental para que el polimorfismo funcione de forma correcta.
En lenguajes como Java, la ligadura dinámica es el comportamiento por defecto para los métodos de instancia no marcados como final, static o private. El programador no necesita indicar explícitamente que desea este comportamiento, ya que el lenguaje lo gestiona automáticamente.
En C++, sin embargo, la ligadura dinámica solo se produce si el método se declara como virtual. Si no se hace, el enlace es estático y se resuelve en tiempo de compilación. En Python, al ser un lenguaje dinámico, todas las llamadas a métodos se resuelven en tiempo de ejecución, por lo que el polimorfismo y el enlace tardío son implícitos desde el principio.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

El siguiente ejemplo muestra una jerarquía simple donde Soldado define un método saludar, y una de sus subclases redefine dicho comportamiento. Se ilustra cómo, al usar referencias del tipo base, el método ejecutado depende del tipo real del objeto.
La clave del polimorfismo se observa al recorrer una colección de Soldado sin conocer el tipo concreto de cada elemento. El código cliente invoca siempre el mismo método, pero el resultado varía según la subclase.
Esto demuestra cómo el polimorfismo permite desacoplar el código que usa los objetos del código que define su comportamiento concreto.
```
class Soldado {
    public void saludar() {
        System.out.println("Soldado saludando");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Zapador saludando");
    }
}

class Artillero extends Soldado {
}

Soldado[] soldados = {
    new Zapador(),
    new Artillero()
};

for (Soldado s : soldados) {
    s.saludar();
}
```

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Al sobreescribir un método, es posible invocar la versión definida en la superclase para reutilizar parte de su comportamiento y ampliarlo o modificarlo ligeramente. Esta técnica es útil cuando se desea mantener la lógica base y añadir funcionalidad específica.
Para ello, Java proporciona la palabra clave super, que permite acceder explícitamente a los métodos y constructores de la clase base. De este modo, se evita duplicar código y se mantiene la coherencia del comportamiento común.
En el ejemplo, el zapador realiza el saludo estándar del soldado y luego añade un mensaje adicional, combinando herencia y especialización de comportamiento.
```
class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar();
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
```

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Al sobreescribir un método en Java, la firma del método debe ser compatible con la del método base. Esto implica que los parámetros deben ser exactamente los mismos, y el tipo de retorno debe ser el mismo o un subtipo (retorno covariante). Además, no se puede reducir la visibilidad del método.
La sobreescritura (overriding) redefine un método heredado, mientras que la sobrecarga (overloading) consiste en definir varios métodos con el mismo nombre pero con distintos parámetros, dentro de una misma clase. La sobrecarga no participa en el polimorfismo en tiempo de ejecución.
La anotación @Override indica explícitamente que un método pretende sobrescribir otro de la superclase. Su uso es altamente recomendable, ya que permite al compilador detectar errores, como cambios accidentales en la firma del método.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

En Java, el polimorfismo se emplea desde las etapas iniciales del aprendizaje, aunque no siempre se sea consciente de ello. Un ejemplo claro es la sobreescritura de métodos heredados de la clase Object, como toString, equals o hashCode.
Cuando se redefine toString en una clase propia, y luego se imprime un objeto usando System.out.println, el método que se ejecuta depende del tipo real del objeto. Esto es un caso directo de polimorfismo basado en enlace dinámico.
Por tanto, incluso antes de estudiar formalmente el concepto, ya se está utilizando polimorfismo en Java, aprovechando la jerarquía común de todas las clases y la resolución dinámica de métodos.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una clase abstracta es una clase que no puede instanciarse directamente y que sirve como base para otras clases. Puede contener métodos con implementación y métodos sin ella. Estos últimos se denominan métodos abstractos.
Un método abstracto se declara sin cuerpo y obliga a que las subclases proporcionen una implementación concreta. Esto garantiza que ciertos comportamientos estén definidos en todos los tipos derivados, aunque varíen en su ejecución.
En el ejemplo, Soldado se define como abstracto y declara un método abstracto atacar. Cada tipo de soldado implementa su propia forma de atacar. La palabra clave abstract debe colocarse tanto en la clase como en el método.
````
abstract class Soldado {
    public void saludar() {
        System.out.println("Saludo general");
    }
    public abstract void atacar();
}

class Artillero extends Soldado {
    public void atacar() {
        System.out.println("Dispara cohetes");
    }
}
````
## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

La palabra clave final en Java tiene distintos efectos según dónde se aplique. Cuando se aplica a un método, impide que dicho método sea sobreescrito en las subclases. Cuando se aplica a una clase, evita que sea heredada.
Dado que el polimorfismo se basa en la sobreescritura de métodos, marcar un método o una clase como final limita explícitamente el polimorfismo. Esto puede ser útil por razones de diseño, seguridad o rendimiento.
Un ejemplo conocido en la API estándar de Java es la clase String, que es final. Esto garantiza que su comportamiento no pueda ser alterado mediante herencia, evitando errores y problemas de seguridad.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

Las interfaces en Java definen un conjunto de métodos que una clase se compromete a implementar. No contienen estado (salvo constantes) y describen qué hace un objeto, no cómo lo hace. Son una herramienta clave para el polimorfismo.
Aunque se parecen a las clases abstractas, las interfaces se centran exclusivamente en el contrato de comportamiento. A diferencia de las clases, una clase puede implementar múltiples interfaces, resolviendo así la limitación de la herencia simple.
Gracias a esto, las interfaces permiten diseñar sistemas más flexibles y desacoplados, donde los objetos pueden ser intercambiables siempre que cumplan el mismo contrato.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

El diseño propuesto utiliza una clase abstracta Punto con un método abstracto calcularDistanciaA. Las subclases Punto2D y Punto3D proporcionan implementaciones específicas del cálculo de distancia según sus dimensiones.
Mediante instanceof y downcasting, se verifica que el punto recibido sea del mismo subtipo, garantizando que el cálculo sea coherente. Aunque esta técnica es válida, también muestra las limitaciones cuando el diseño no evita comprobaciones de tipo explícitas.
La clase Linea trabaja únicamente con referencias de tipo Punto. Gracias al polimorfismo, puede calcular su longitud sin conocer la dimensión concreta de los puntos, delegando el cálculo en los propios objetos.
````
abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto { /* implementación */ }
class Punto3D extends Punto { /* implementación */ }

class Linea {
    private Punto a, b;
    public double longitud() {
        return a.calcularDistanciaA(b);
    }
}
````

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La herencia de interfaces permite que una interfaz extienda a otra, heredando sus métodos y ampliando su contrato. Esto facilita la evolución de diseños sin romper compatibilidad con implementaciones existentes.
En Java, existe herencia múltiple de interfaces, ya que una interfaz puede extender varias interfaces, y una clase puede implementar varias a la vez. Esto proporciona una gran flexibilidad en el diseño.
El ejemplo muestra una interfaz Fichero que define la lectura, y otra FicheroEscribible que la extiende añadiendo capacidades de escritura y eliminación, ampliando el comportamiento sin duplicación.
````
interface Fichero {
    String leer();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void borrar();
}
````
