# Planificación de pruebas

Planificar un proyecto...

* ¿CÓMO hay que hacerlo?
* ¿QUÉ hay que hacer?
* ¿CUÁNDO hay que hacerlo?
* ¿QUIÉN lo va a hacer?

## Formas de planificación (Modelos)

* **Predictiva**: Soporta muy mal los cambios
  * **Modelo sequencial**: Planifican a nivel de **release**.
    * Waterfall
    * Modelo V
    * Modelo W
* **Adaptativa**:
  * **Modelos iterativos**: Planifican a nivel de **iteración**, **release**.
    * Modelo UP
  * **Modelos ágiles**: (Más iterativos) Planifican a nivel de **día**, **iteración** y **release**.
    * Modelo XP
    * Modelo Scrum

    
> Planificar una iteración como una minicascada es un error. En las iteraciones se van haciendo requisitos.

## Niveles de planificación (horizontes)

> Hay que mirar mas allá del horizonte, pero teniento en cuanta que la exactictitud de nuestro plan irá decreciendo.

1. **Día**
2. **Iteración**
3. **Release**: Planifica la siguiente entrega (las historias de usuario).
4. **Producto**
5. **Portfolio**
6. **Stratey**


## Efectividad de las pruebas

* ¿Cuántas pruebas? triángulo de recursos
> #### Triángulo de recursos. Equilibrar:
> ```
> funcionalidad
>   /      \
> dinero-tiempo
> ```

* Priorizar: decidir qué tests son más importantes.
* Fijar criterios


## Recordar: El proceso de las pruebas

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

## Recordar: Niveles de las pruebas

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

## Modelos sequenciales

### V-Model: `\/`
* La `\`, es ???.
* La `/`, es el diseño+implentación de cada nivel de pruebas.

### W-Model: `\\//`
* La primera \ es ???.
* La "v central" se corresponde a las pruebas:
  * La parte izquierda es el diseño (de cada nivel).
  * La parte derecha la implementación (de cada nivel).
* La última / es el debugging.


## Modelos iterativos

### Modelo	UP
...

### Scrum

![scrum](http://www.metodoagil.com/wp-content/uploads/2017/02/metodologia_scrum.gif?x78273)

* **Sprint**: 3-4 semanas.
* **Product backlog**: Lista de requerimientos priorizados por su valor de negocio (los valores más altos al principio).
* **Product owner**: Responsable de conseguir el máximo valor de negocio del producto.
* **Scrum master**: Actúa como guía del grupo durante el proceso, "protege" al grupo del exterior, y sirve como ayuda al mismo. NO es un gestor de proyectos

En un proceso Scrum NO hay una fase de testing "separada" del resto de actividades del desarrollo. Cuando un desarrollador termina una historia de usuario, los tests tienen que estar preparados para su ejecución. Si el test pasa, la historia es aceptada y se pasa a la siguiente. Una vez que se han probado todas las historias y han pasado los tests, se da por concluido el sprint y se pasa al siguiente.

### Modelo	XP

* **Story cards** (historias de usuario): Representan características requeridas por el usuario (luego se dividirá en tareas
concretas para ser desarrolladas). Criterios de aceptación al reverso.

> *As an [actor] I want [action] so that [achievement].*

* **Task list** o **Iteration plan board**: Story cards de una iteración. (Se priorizan según su valor).
* **Iteración**: ≃1 semana.
* **Release plan board**: Lista de historias de usuario priorizadas para cada iteración.


## Test Driven Development (TDD)

Es una práctica donde primero se escriben los tests y luego el software. (Utilizado en desarrollos ágiles).

1.	Escribir	una	prueba	para	el	nuevo	código	y	ver	cómo	falla.	
2.	Implementar	el	nuevo	código,	haciendo	"la	solución	más	simple	que	pueda	funcionar".
3.	Comprobar	que	la	prueba	es	exitosa	y	refactorizar	el	código.

* **Aproximación "clásica"**: Utilizar objetos reales siempre que sea posible, y stubs (u otro “doble”) si es complicado.
  * Estilo de diseño **INSIDE-OUT**: comenzamos por los componentes de bajo nivel. No necesitamos stubs.
* **Aproximación "mockist"**: Utilizar siempre un mock para cualquier objeto con un comportamiento interesante.
  * Estilo de diseño **OUTSIDE-IN**: comenzamos por los componentes de alto nivel. Mocks para sustituir los componentes de
bajo nivel.
  * Como consecuencia de esta aproximación surge **BDD** (Behaviour Driven Development).

## Behaviour Driven Development (BDD)

Es muy importante entender por qué estamos llevando a cabo un proyecto.

Cualquier sistema software proporciona un VALOR de NEGOCIO.
BDD nos permite centrarnos en el valor de negocio de nuestra aplicación.

> Si preguntamos a los usuarios "qué quieren", generalmente obtendremos por respuesta un conjunto de requerimientos detallados sobre cómo imaginan la solución. Es decir, los usuarios no nos dicen lo que necesitan, diseñan una solución por
nosotros.
>
> Debemos centrarnos en las "features" (funcionalidades) que proporcionan un valor de negocio para el usuario. Es decir, funcionalidades que nos ayudan a conseguir los OBJETIVOS del proyecto.

Ejemplos de objetivos:
* Incrementar en un 10% el número de clientes.
* Incrementar las ventas en un 5%.

## Integración continua

Consisten en integrar el código del proyecto de forma ininterrumpida (en ciclos cortos) en una máquina que debe estar funcionando 24/7.

Nace con el proceso de desarrollo XP

Las integraciones continuas realizan **construcciones planificadas** del sistema. Cada hora or ejemplo.

Funcionamiento: Falta...

# Ejercicios

##### 1. Indica si son ciertas o falsas las siguientes afiirmaciones:

#### a) La práctica de integraciones continuas es un requisito de los modelos iterativos.

No porque el modelo UP es iterativo y no te obliga a aplicar esa práctica.

#### b) No podemos determinar el alcance de las pruebas sin implementar los tests.

El alcance es lo primero que determinas. Defines el objetivo lo primero, y después diseñas acorde a ese objetivo. Otra cosa es que luego revises para ver hasta dónde has llegado, pero es falsa.

#### c) El modelo en V determina la planifiicación temporal de las pruebas en modelos de proceso que utilizan una planifiicación predictiva.

Un modelo secuencial es predictivo, por lo tanto, la afirmación es cierta. El modelo en V no se aplica a una planificación adaptativa. Los adaptativos son los iterativos y los ágiles. En un modelo secuencial tu predices lo que va a pasar desde el principio, en uno iterativo estás siendo adaptativo, no eres predictivo.

---

#### 2. Teniendo en cuenta TODAS y cada uno de los métodos y herramientas que hemos explicado en clase y/o usado en prácticas en esta segunda parte, construye un PLAN según el modelo W (puedes omitir las tareas relativas a las sesiones S1..S6).

#### Cada tarea debe tener asociado un número que indique el orden cronológico en el plan, el objetivo CONCRETO de dicha tarea, y el método/herramienta utilizada para llevar a cabo cada tarea. Además, para cada una de las tareas del plan, tenéis que indicar cómo la hemos llevado a cabo en prácticas. Proporcionamos la siguiente tabla, que deberás rellenar, añadiendo las fiilas que consideres oportunas.

| TAREAS  | Objetivo | Herramienta/método | En practicas... |
|---------|----------|--------------------|-----------------|
| Tarea 1 |          |                    |                 |
| Tarea 2 |          |                    |                 |

En una iteración tenemos certeza que no va a haber ningún cambio, por lo menos hasta el final de la iteración, pero durante la iteración no te molesta el cliente, te molesta en las pruebas de validación de fin de iteración. La iteración es una zona segura, y ahí puedo aplicar una V o W, pero sólo dentro de la iteración, no para todo el plan.
