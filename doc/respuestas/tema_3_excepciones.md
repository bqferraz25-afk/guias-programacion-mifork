# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En C no existen excepciones, por lo que el control de errores debe diseñarse explícitamente mediante valores de retorno o parámetros adicionales. En una función raiz que calcula la raíz cuadrada de un número flotante positivo, si se recibe un número negativo, se debe comunicar el error al código llamador para que sea este quien informe al usuario. El control del error no se realiza automáticamente, sino que debe programarse manualmente.

Primera opción: devolver un valor especial de error.
Se puede acordar que la función devuelva un valor que indique fallo, por ejemplo -1.0 (si se sabe que no es válido como resultado) o NAN (Not A Number). El código llamador debe comprobar ese valor.
```java
#include <stdio.h>
#include <math.h>

float raiz(float x) {
    if (x < 0) {
        return -1.0;  // valor especial de error
    }
    return sqrt(x);
}

int main() {
    float resultado = raiz(-9);

    if (resultado == -1.0) {
        printf("Error: número negativo\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }

    return 0;
}
```
Segunda opción: devolver un código de error y usar un parámetro de salida.
La función puede devolver un entero indicando éxito o error, y usar un puntero para devolver el resultado.

```
#include <stdio.h>
#include <math.h>

int raiz(float x, float *resultado) {
    if (x < 0) {
        return 0;  // error
    }
    *resultado = sqrt(x);
    return 1;  // éxito
}

int main() {
    float resultado;

    if (!raiz(-9, &resultado)) {
        printf("Error: número negativo\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }

    return 0;
}
```
## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una excepción es un mecanismo que permite señalar que se ha producido una situación anómala durante la ejecución de un programa. Esa situación rompe el flujo normal de ejecución, porque la función que la detecta no puede continuar de forma correcta.

El objetivo de las excepciones es separar la lógica normal del programa del tratamiento de errores. En lugar de devolver códigos especiales que deben comprobarse manualmente, se permite “lanzar” una excepción y dejar que otro código, situado en un nivel superior, la capture y decida qué hacer. Esto mejora la claridad del código y evita múltiples comprobaciones repetitivas.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En Java, el control de errores puede realizarse mediante excepciones. En lugar de devolver un valor especial, el método puede lanzar una excepción si recibe un número negativo. La función no informa directamente al usuario, sino que delega esa responsabilidad en el código llamador.
```
class Calculadora {

    public double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("Número negativo no permitido");
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        Calculadora calc = new Calculadora();

        try {
            double resultado = calc.raiz(-9);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```
Aquí el método raiz lanza una excepción si el valor es negativo. El método main utiliza un bloque try-catch para controlar el error desde fuera, que es exactamente lo que se buscaba en el diseño.

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

Lanzar una excepción significa crear un objeto excepción y detener inmediatamente la ejecución normal del método mediante la palabra clave throw. A partir de ese momento, el flujo del programa cambia y se busca un bloque catch adecuado.

Capturar una excepción significa interceptarla en un bloque catch para tratarla. Propagarse significa que, si un método no la captura, la excepción sube automáticamente por la pila de llamadas hacia el método que lo invocó.

Mientras se propaga, los métodos intermedios se abandonan inmediatamente: no continúan ejecutándose después del punto donde se produjo el error. No se reanudan posteriormente. En el ejemplo de la raíz, si raiz lanza la excepción y no la captura, el método que la llamó es el siguiente en intentar manejarla. Si nadie la captura, el programa finaliza mostrando el error.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

La propagación natural evita tener que comprobar manualmente códigos de error tras cada llamada, como ocurre en C. El flujo de error es automático y estructurado, lo que simplifica el diseño de funciones intermedias que no necesitan conocer los detalles del fallo.

Además, se separa claramente la lógica normal del tratamiento de errores. En C, cada función debe comprobar y reenviar códigos de error explícitamente. En Java, si no se captura la excepción, esta continúa automáticamente hacia arriba en la pila, reduciendo código repetitivo y posibles olvidos.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

En orientación a objetos, las excepciones son objetos. En Java, todas heredan de la clase Throwable. Esto significa que pueden contener atributos y métodos, como cualquier otro objeto.

Desde el punto de vista de la encapsulación, una excepción puede almacenar información relevante sobre el error (mensaje, causa, datos adicionales) sin exponer directamente los detalles internos. El código que la captura puede acceder a esa información mediante métodos públicos.

Sí es posible crear excepciones personalizadas extendiendo Exception o RuntimeException. Esto permite definir errores específicos del dominio del problema.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

Un objeto excepción contiene información esencial como un mensaje descriptivo del error y el rastro de la pila de llamadas (stack trace). Esta información es muy útil cuando el error llega a un manejador situado en un nivel superior.

El mensaje permite saber qué ha ocurrido, y el stack trace indica en qué métodos y líneas se produjo el problema. En comparación con C, donde solo se devuelve un código numérico, en Java se obtiene información mucho más detallada sin necesidad de diseñarla manualmente.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

Sí, se pueden tener varios bloques catch asociados a un mismo try. Cada bloque puede capturar un tipo distinto de excepción.

Sin embargo, solo se ejecuta un único bloque catch, el primero que coincida con el tipo de la excepción lanzada. Una vez ejecutado ese bloque, no se evalúan los siguientes

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

Para garantizar la ejecución de código necesario (por ejemplo, cerrar un fichero), se utiliza el bloque finally. Este bloque se ejecuta tanto si ocurre una excepción como si no.

Ejemplo con catch:

```try {
    // código que puede lanzar excepción
} catch (IOException e) {
    System.out.println("Error de E/S");
} finally {
    System.out.println("Cierre de recursos");
}
```
Ejemplo sin catch:
```
try {
    // código que puede lanzar excepción
} finally {
    System.out.println("Cierre de recursos");
}
```
En ambos casos, el bloque finally se ejecuta siempre antes de que la excepción continúe propagándose.

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí, el bloque finally puede existir sin catch. En ese caso, se utiliza para ejecutar código obligatorio independientemente de que ocurra una excepción.

El bloque finally se ejecuta siempre, tanto si se produce una excepción como si no. Incluso si hay un return dentro del try, el código del finally se ejecuta antes de que el método termine definitivamente.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

Las excepciones controladas (checked) son aquellas que el compilador obliga a capturar o declarar con throws. Las no controladas (unchecked) heredan de RuntimeException y no es obligatorio capturarlas.

RuntimeException representa errores de programación, como argumentos inválidos o errores lógicos.

### Situaciones donde se prefiere excepción controlada:

-Fallo al abrir un fichero (IOException)

-Error de red

-Acceso a base de datos

-Lectura de entrada del usuario

### Situaciones donde se prefiere excepción no controlada:

-Argumento inválido (IllegalArgumentException)

-Acceso fuera de rango (IndexOutOfBoundsException)

-Referencia nula (NullPointerException)

-Estado ilegal del objeto

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

La palabra clave throws se utiliza en la firma de un método para declarar que puede lanzar una excepción controlada. No maneja la excepción, sino que informa de que puede producirse.

Es alternativa a capturarla porque permite delegar la responsabilidad en el método llamador. De esta forma, la excepción se propaga hacia arriba sin necesidad de escribir un bloque try-catch en ese método.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.
```
import java.io.*;

public void leerFichero(String nombre) throws IOException {
    FileReader fr = null;
    try {
        fr = new FileReader(nombre);
        // lectura
    } finally {
        if (fr != null) {
            fr.close();
        }
    }
}
```
Aquí se declara throws IOException porque no interesa manejar el error localmente. Sin embargo, se garantiza el cierre del recurso mediante finally.

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Sí, se pueden declarar excepciones no controladas en throws, pero no es obligatorio. El compilador no exige capturarlas.

El método llamador no está obligado a usar try-catch. Declararlas puede tener sentido como documentación, indicando que el método puede lanzar ese tipo de error, pero no impone ninguna obligación técnica.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Se recomienda usar excepciones controladas cuando el error es externo y razonablemente recuperable, como problemas de entrada/salida. Se usan no controladas cuando el error es consecuencia de un fallo lógico del programador.

No todos los lenguajes distinguen entre ambas. En lenguajes como C# o Python solo existen excepciones no controladas (no hay obligación del compilador). La opción más habitual en lenguajes modernos es el modelo no controlado.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Sí tiene sentido lanzar excepciones dentro de un catch. Puede capturarse una excepción y lanzar otra distinta más apropiada al nivel de abstracción.

También se puede relanzar la misma excepción usando throw;. Esto tiene sentido cuando se quiere realizar alguna acción (por ejemplo, registrar el error) y dejar que continúe propagándose.
```
catch (IOException e) {
    System.out.println("Registro del error");
    throw e;  // relanzar
}
```
## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

Que una excepción sea la causa de otra significa que una excepción de nivel superior encapsula otra de nivel inferior. Se utiliza el constructor que recibe una causa.
```
try {
    // código que lanza IOException
} catch (IOException e) {
    throw new RuntimeException("Error al procesar datos", e);
}
```
Aquí la IOException es la causa. Cuando se muestra por pantalla, Java indica el mensaje principal y después “Caused by”, mostrando también la excepción original y su stack trace. Esto facilita el diagnóstico del problema.

