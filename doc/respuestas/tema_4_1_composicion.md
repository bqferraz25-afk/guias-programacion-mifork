# Tema 4.1. Composición

## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

En C, la composición se consigue mediante la inclusión de estructuras dentro de otras estructuras. Esto permite modelar relaciones del tipo “tiene-un”, donde una estructura mayor contiene como campos otras más simples. En este caso, una línea puede definirse como un objeto que tiene dos puntos, y cada punto contiene dos coordenadas. Este enfoque es puramente estructural, ya que no existe encapsulación ni métodos asociados a las estructuras.

Para operar con estas estructuras, es necesario definir funciones externas que reciban dichas estructuras como parámetros. Así, se puede implementar una función para calcular la distancia entre dos puntos usando la fórmula euclídea, y otra función que calcule la longitud de una línea utilizando la distancia entre sus dos extremos. Se observa que los datos y el comportamiento están separados, a diferencia de la orientación a objetos.
```
#include <stdio.h>
#include <math.h>

typedef struct {
    float x;
    float y;
} Punto;

typedef struct {
    Punto p1;
    Punto p2;
} Linea;

float distancia(Punto a, Punto b) {
    return sqrt(pow(b.x - a.x, 2) + pow(b.y - a.y, 2));
}

float longitud(Linea l) {
    return distancia(l.p1, l.p2);
}
```
## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

En Java, la composición se expresa mediante atributos que son objetos de otras clases. En este caso, una clase Linea contiene dos objetos de tipo Punto. A diferencia de C, se integran datos y comportamiento dentro de las clases, permitiendo que cada objeto sea responsable de sus propias operaciones, como calcular distancias.

Gracias a la encapsulación, se puede garantizar la inmutabilidad de los objetos. Esto se logra declarando los atributos como private final y evitando proporcionar métodos modificadores (setters). De este modo, una vez creado un Punto o una Linea, su estado no puede cambiar, lo que mejora la seguridad y previsibilidad del programa.

````
class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        return Math.sqrt(Math.pow(otro.x - this.x, 2) +
                         Math.pow(otro.y - this.y, 2));
    }
}

class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
````

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La multiplicidad en composición indica cuántas instancias de una clase se relacionan con otra. Es un concepto importante en el modelado, ya que define las cardinalidades entre objetos. Se expresa normalmente en términos como “uno a uno”, “uno a muchos”, etc.

En el ejemplo, una Linea está compuesta exactamente por dos objetos Punto, por lo que la multiplicidad de Linea a Punto es 2. En sentido inverso, un Punto podría pertenecer a múltiples líneas o a ninguna, dependiendo del diseño, por lo que la multiplicidad de Punto a Linea sería 0..N. Esto indica que un mismo punto puede ser compartido entre varias líneas.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La composición fuerte implica que los objetos contenidos dependen completamente del ciclo de vida del objeto contenedor. Si el objeto principal desaparece, también lo hacen sus componentes. Este tipo de relación representa una “posesión” fuerte y suele modelar partes esenciales de un todo.

Por otro lado, la composición débil (también llamada agregación o asociación) implica que los objetos pueden existir independientemente. El contenedor simplemente mantiene referencias a ellos, pero no es responsable de su creación ni destrucción. En terminología habitual, la composición fuerte se denomina “composición” propiamente dicha, mientras que la débil se conoce como “agregación” o “asociación”.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

Cuando una clase simplemente utiliza otra en parámetros, variables locales o dentro de métodos, no se está estableciendo una relación de composición, sino una relación de dependencia. Esto significa que una clase necesita a otra para funcionar, pero no la contiene como parte de su estado interno.

La dependencia es una relación más débil que la composición, ya que no implica propiedad ni persistencia de la relación. Por ejemplo, crear un objeto dentro de un método con new o recibirlo como argumento no implica que el objeto forme parte de la estructura interna de la clase. Por tanto, se trata de una relación temporal y no estructural.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

En composición fuerte, los objetos se crean dentro de la clase contenedora, ligando su ciclo de vida a esta. En cambio, en composición débil, los objetos se reciben desde fuera, permitiendo que existan independientemente.

````
// Composición fuerte
class LineaFuerte {
    private final Punto p1;
    private final Punto p2;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }
}
````
````
// Composición débil
class LineaDebil {
    private final Punto p1;
    private final Punto p2;

    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }
}
````

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En Java, no se gestiona manualmente la memoria como en C o C++. La destrucción de objetos se realiza automáticamente mediante el recolector de basura (garbage collector). Este mecanismo elimina los objetos que ya no tienen referencias activas.

Por ello, aunque no se observe una destrucción explícita de los objetos Punto dentro de Linea, cuando una Linea deja de ser accesible, los puntos también quedan sin referencias (si no están compartidos) y serán eliminados automáticamente. Este comportamiento simplifica la gestión de memoria y evita errores como fugas o dobles liberaciones.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

En este caso, el departamento contiene profesores, pero estos pueden existir fuera de él, por lo que se trata de composición débil. Se mantiene además una invariante: siempre debe haber un director y este debe pertenecer al conjunto de profesores.
````
class Profesor {
    private String nombre;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }
}

class Departamento {
    private Profesor[] profesores = new Profesor[50];
    private int numProfesores = 0;
    private Profesor director;

    public Departamento(Profesor director) {
        this.director = director;
        profesores[0] = director;
        numProfesores = 1;
    }

    public void addProfesor(Profesor p) {
        if (numProfesores >= 50) throw new RuntimeException("Límite alcanzado");
        profesores[numProfesores++] = p;
    }

    public void removeProfesor(int pos) {
        if (profesores[pos] == director)
            throw new RuntimeException("No se puede eliminar el director");

        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        numProfesores--;
    }

    public int getNumProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int pos) {
        return profesores[pos];
    }

    public void setDirector(Profesor p) {
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == p) encontrado = true;
        }
        if (!encontrado) throw new RuntimeException("Debe pertenecer al departamento");
        director = p;
    }
}
````

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

El uso de List simplifica significativamente la implementación, ya que elimina la necesidad de gestionar manualmente el tamaño, desplazamientos y límites del array. Métodos como add, remove o size ya están implementados, reduciendo errores y mejorando la legibilidad.
````
import java.util.*;

class Departamento {
    private List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor director) {
        this.director = director;
        profesores.add(director);
    }
}
````
Si se devolviera directamente la lista interna, se rompería la encapsulación, ya que código externo podría modificarla sin control. Para evitarlo, se puede devolver una copia de la lista o una vista inmutable (Collections.unmodifiableList). Esto garantiza que la estructura interna no pueda ser alterada desde el exterior.

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

La composición recursiva ocurre cuando una clase contiene una referencia a otra instancia de sí misma. En este caso, una persona tiene una madre, que también es una persona. Esto permite modelar estructuras jerárquicas o encadenadas.
````
class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }
}
````
````
public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Ana", null);
        Persona madre = new Persona("Bea", abuela);
        Persona hijo = new Persona("Carlos", madre);
    }
}
````
Otros ejemplos clásicos incluyen estructuras como árboles, listas enlazadas o excepciones encadenadas. En todos estos casos, la clase contiene referencias a instancias de su mismo tipo, permitiendo definir estructuras potencialmente infinitas.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

Una relación bidireccional implica que ambas clases mantienen referencias entre sí. Es decir, no solo el departamento conoce a sus profesores, sino que cada profesor también conoce a qué departamento pertenece. Esto facilita la navegación en ambos sentidos.

Para implementarlo, se debe añadir un atributo Departamento en la clase Profesor y actualizarlo cada vez que se añade o elimina un profesor. Además, es necesario mantener la coherencia entre ambas direcciones, asegurando que si un profesor pertenece a un departamento, el departamento también lo contiene en su colección. Esto requiere especial cuidado para evitar inconsistencias.

