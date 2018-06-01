# Planificación de pruebas

Planificar un proyecto...

* ¿CÓMO hay que hacerlo?
* ¿QUÉ hay que hacer?
* ¿CUÁNDO hay que hacerlo?
* ¿QUIÉN lo va a hacer?

### Formas de planificación (Modelos)

* **Predictiva**: Soporta muy mal los cambios
  * **Modelo sequencial** (waterfall)
* **Adaptativa**:
  * **Modelos iterativos**
  * **Modelos ágiles**

> Planificar una iteración como una minicascada es un error. En las iteraciones se van haciendo requisitos.

### Niveles de planificación (horizontes)

> Hay que mirar mas allá del horizonte, pero teniento en cuanta que la exactictitud de nuestro plan irá decreciendo.

1. **Día**:
2. **Iteración**:
3. **Release**: Considera las historias de usuario que serán desarrolladas en la SIGUIENTE entrega.
4. **Product**: Considera la evolución de posteriores "releases".
5. **Portfolio**: Considera la selección de desarrollos dentro de la estrategia de la empresa.
6. **Stratey**:

* **Predictiva**:
  * **Modelo sequencial**: Planifican a nivel de release.
* **Adaptativa**:
  * **Modelos iterativos**: Planifican a nivel de release, iteración.
  * **Modelos ágiles (XP)**: Planifican a nivel de release, iteración y día.

### Efectividad de las pruebas

* ¿Cuántas pruebas? triángulo de recursos
> #### Triángulo de recursos
> El desarrollo del software debe equilibrar los tres vértices del triángulo: **tiempo**, **dinero** y **funcionalidad**.

* Priorizar: decidir qué tests son más importantes.
* Fijar criterios


### Recordar: El proceso de las pruebas

> Objetivos:
> * Demostrar que el software satisface los requerimientos.
> * Descubrir situaciones en las que el comportamiento del soMware es incorrecto, indeseable, o no conforme a las especificaciones

1. **Planificación y control**
    * Planificación: Se determina ¿QUÉ? se va a probar, ¿CÓMO?, ¿QUIÉN? y ¿CUÁNDO?
    * Control: Se determina QUÉ se va a hacer si los planes no se ajustan a la realidad.
2. **Analisis y diseño**
    * Se determina qué CASOS DE PRUEBA se van a utilizar 
    * Un caso de prueba es un DATO CONCRETO + RESULTADO ESPERADO
3. **Implementación y ejecución**
    * Se implementan y ejecutan los tests. Es la parte más visible, pero no es posible que los test sean efectivos sin realizar los pasos anteriores
4. **Evaluar informe**
    * Se verifica que se alcanzan los comple'on criteria (p.ej 85% de cobertura) y se genera un informe
5. **Actividades finales**
    * En este punto las pruebas ya han finalizado. Se trata de asegurar que los informes están disponibles, ... 

### Recordar: Niveles de las pruebas

* **UNIDAD**
  * **Objetivo**: Encontrar defectos en las unidades. Deben de probarse de forma AISLADA.
  * **Diseño**: Camino básico (CB) y Particiones equivalentes (CN) 
  * **Implementación**: Java, Maven, JUnit, EasyMock.
* **INTEGRACIÓN**
  * **Objetivo**: Encontrar defectos en la interacción de las unidades. Debe establecerse un ORDEN de integración.
  * **Diseño**: Guías (consejos) en función de los tipos de interfaces (CN).
  * **Implementación**: Java, Maven, JUnit, DbUnit.
* **SISTEMA**
  * **Objetivo**: Encontrar defectos derivados del comportamiento del sistema como un todo.
  * **Diseño**: Transición de estados (CN).
  * **Implementación**: Java, Maven, Junit.
* **ACEPTACIÓN**
  * **Objetivo**: Valorar en qué GRADO se satisfacen las expectativas del cliente. Basadas en criterios de ACEPTACIÓN (prop. emergentes) cuantificables.
  * **Diseño**:
    * Basado en requerimientos (CN)
    * Basado en escenarios (CN)
    * Pruebas de rendimiento (CN)
    * Pruebas α y β (CN)
  * **Implementación**:
    * Katalon recorder.
    * Java, Maven, Junit, Webdriver.
    * JMeter
    * Usuario (α y β)

### Planificación temporal de las pruebas

Hay que tener claro en que momento se hace cada tipo de pruebas

#### En modelos sequenciales...

V-Model y W-Model

