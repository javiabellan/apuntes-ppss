# Análisis de pruebas

Una vez hechas las pruebas, tenemos que evaluar si hemos alcanzado las metas definidas y emitir un informe

Tenemos que poder **medir** si hemos alcanzando nuestros objetivos.

Si parte de nuestro código no se ejecuta durante las pruebas, es que no está siendo probado.
Esto es la COBERTURA de código: analizar cual es la EXTENSIÓN de nuestras pruebas.

### Cobertura de código

Hay 7 niveles. Cobertura de...
1. **Líneas**: Probar cada sentencia (línea).
2. **Ramas** o **Decisiones**: Probar cada rama (decisión) `if (decisión)`, en sus vertientes verdadera y falsa.
3. **Condiciones**: Probar cada condición `if (cond1 && cond2 || cond3)`, en sus vertientes verdadera y falsa.
4. **Condiciones y decisiones**: Juntar las 2 anteriores.
    * Debido a que en lenguajes como C++ y Java, sólo ejecutan las condiciones "siguintes" (tras `&&`) si las anteriores furon falsas 
5. **Condiciones múltiples**: Probar todas las posibles combinaciones de condiciones.
    * Creo que si los ifs son sequeciales puedes ir prpbando en paralelo ((T,T),(T,T)) - ((F,T),(F,T)) - ...
    * Pero ifs anidados ahora se juntan para formar mas cominaciones (T,T,T,T) - (F,T,T,T) - ...
6. **Bucles**: Probar el bucle con 0 iteraciones, una iteración, n iteraciones y max iteraciones.
7. **Caminos**: Probar todos los posibles caminos de control. Es muy difícil de conseguir cuando hay bucles.

> Existe un nivel 0 de prueba lo que te de la gana y el usuario provará el resto.

Para pensar...
* Un 100% de cobertura de **ramas** implica un 100% de cobertura de líneas
* Un 100% de cobertura de **condiciones** NO garantiza un 100% de cobertura de líneas:

```java
if(cond1 && cond2) {
    sentencia;
}
// Podemos conseguir el nivel 3 (cobertura de condiciones), con dos casos:
// (TRUE && FALSE) y (FALSE && TRUE)
// Pero nunca ejecutamos la sentencia
```
* Un 100% de cobertura de **condiciones múltiples** implica un 100% de cobertura de **condiciones**, **decisiones**, y **condiciones/decisiones**
* Un 100% de cobertura de **caminos** implica un 100% de cobertura de bucles, ramas y sentencias.
* Un 100% de cobertura de **caminos** NO garantiza una cobertura de condiciones múltiples (ni al contrario).

### Analizadores automáticos de cobertura

**Instrumentan el código**: añadir código adicional para poder obtener una métrica de cobertura. (El código adicional añade algún contador de código que devuelve un resultado después de la ejecución del mismo).

En Java:
* Instrumentación del código fuente.
* Instrumentación del byte-code. (Esto es lo que hacemos con Cobertura)
* Máquina virtual de Java modificada

### Herramienta Cobertura

* Instrumenta el **bytecode** de Java.
* Basado en JCoverage.
* Puede utilizarse desde terminal, con Ant o con Maven
* Cobertura genera un informe con 3 métricas concretas:
  * Cobertura de **Líneas** (Line coverage)
  * Cobertura de **Ramas** (Branch coverage)
    * Aunque para nosotros es cobertura de DECISIONES, la herramienta Cobertura, considera el porcentaje de condiciones cubiertas frente al total de condiciones en el programa.
  * **Complejidad ciclomática** (Complexity): ¿Cuántos casos de prueba son necesarios para cubrir todos los caminos?

> ### Complejidad Ciclomática (CC)
>
> Nos da una **cota superior del número máximo de tests a realizar** de forma que se se garantice la ejecución de todas las **líneas** de código al menos una vez, y también nos garantiza que todas las **condiciones** se ejecutarán en sus vertientes verdadera y falsa (Recuerda: cada nodo del grafo debe representar como máximo una única condición).
>
> 1. CC = arcos - nodos + 2
> 2. CC = número de condiciones + 1
> 3. CC = número de regiones, (incluyendo la externa)
> 4. Forma de la herramienta Cobertura
>    * CC = número de condiciones + número de salidas (siendo como mínimo 1)
>
> Las alternativas 2 y 3 sólo se pueden utilizar si el programa no tiene saltos incondicionales

### Plugin de Covertura para Maven

Falta...

# Ejercicios

#### a) Justifiica con un ejemplo de código la siguiente afiirmación: “una cobertura de condiciones (nivel 3) del 100% no implica una cobertura de líneas (nivel 1) del 100%

```java
if(cond1 && cond2) {
    sentencia;
}
// Podemos conseguir el nivel 3 (cobertura de condiciones), con dos casos:
// (TRUE && FALSE) y (FALSE && TRUE)
// Pero nunca ejecutamos la sentencia
```

---

#### b) Cita qué 3 métricas aparecen en un informe de Cobertura e indica qué es lo que mide cada una de ellas

* Cobertura de **Líneas** (Line coverage): Número de líneas de nuestro código fuente que son recorridas cuando ejecutamos nuestrso tests.
* Cobertura de **Ramas** (Branch coverage): Aunque para nosotros es cobertura de DECISIONES, la herramienta Cobertura, considera el porcentaje de condiciones cubiertas frente al total de condiciones en el programa.
  * Juan Pablo: calcula la cobertura de condiciones, el número de condiciones de código fuente que recorren nuestros tests.
* **Complejidad ciclomática** (Complexity): cota superior del número máximo de casos de prueba a realizar.
