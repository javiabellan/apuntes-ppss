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




## Recordar

> ### El proceso de las pruebas
> Demostrar que el software satisface los requerimientos (de la especificació).
>
> 1. **Planificación y control**: Qué se va a probar, y qué se va a hacer si no se cumple las espectativas.
> 2. **Analisis y diseño**: Establecer casos de prueba (datos concretos + resul esper).
> 3. **Implementación y ejecución**: Implementar y ejecutar los tests.
> 4. **Evaluar informe**: Verificar que se alcanzan los completion criteria (p.ej 85% de cobertura) y se genera un informe.

> ### Niveles de las pruebas
> 
> 1. **Unidad**: Encontrar defectos en las unidades de forma **aislada**.
>    * Caja blanca: Camino básico
>    * Caja negra: Particiones equivalentes
> 2. **Integración**: Encontrar defectos en la interacción de las unidades (debe establecerse un **orden**).
>    * Caja negra: Sugún tipo de interfaz a usar, hay consejos.
> 3. **Sistema**: Encontrar defectos derivados del comportamiento del sistema como un todo.
>    * Caja negra: Transición de estados.
> 4. **Aceptación**: Valorar en qué **grado** se satisfacen las expectativas del cliente. Basadas en criterios de aceptación (prop. emergentes) cuantificables.
>    * Caja negra:
>      * Funcionales: Basado en requerimientos/escenarios (Katalon, Selenium WebDriver).
>      * No funcionales: Pruebas de rendimiento (JMeter).





## Pruebas en modelos sequenciales

En modelos iterativos, las pruebas se dejan al **final** (Véase según el modelo).

### V-Model: `\/`
* La `\`, es ???.
* La `/`, es el diseño+implentación de cada nivel de pruebas.

```
ACEPTACIÓN     Especificación                  Tests aceptación
----------------------\-----------------------------/---------------
SISTEMA           Requerimientos             Tests sistema
------------------------\-------------------------/-----------------
INTEGRACIÓN          Diseñ. Arqu.          Tests integración
--------------------------\---------------------/-------------------
UNIDADES                Diseñ detall     Tests unitarios
----------------------------\-----------------/---------------------
                           Escribir código fuente
```

### W-Model: `\\//`
* La primera \ es ???.
* La "v central" se corresponde a las pruebas:
  * La parte izquierda es el diseño (de cada nivel).
  * La parte derecha la implementación (de cada nivel).
* La última / es el debugging.

```
Especificación    DisTestAcep               ImplTestAcep   Debug
--------\---------------\-----------------------/-----------/------
   Requerimientos    DisTestSist          ImplTestSist   Debug
----------\---------------\-------------------/-----------/--------
      Diseñ Arquit     DisTestInte      ImplTestInte   Debug
------------\---------------\---------------/-----------/----------
         Diseñ detall    DisTestUnit  ImplTestUnit   Debug
--------------\---------------\-----------/-----------/------------
            Escribir código fuente    Cambiar código fuente
```

## Pruebas en modelos iterativos

En modelos iterativos, las pruebas se planifican a nivel de **iteración** y **release**.

### Modelo	UP

Se hacen las pruebas al final de cada iteración. En las primeras iters. las pruebas serán pequeñitas
y en las últimas iters. las pruebas serán más relevantes.

![up](https://www.inflectra.com/GraphicsViewer.aspx?url=Methodologies/agile-development.xml&name=wordml://03000006.png)

### Scrum

![scrum](http://www.metodoagil.com/wp-content/uploads/2017/02/metodologia_scrum.gif?x78273)

**No** hay una fase de testing "separada" del resto de actividades. Cuando un desarrollador termina una historia de usuario, hace los tests. Si el test pasa, la historia es aceptada y pasa a la siguiente.

### Modelo	XP

Primero se hacen las **historias de usuario** (**Story cards**) que son las características que el usuario pide. Estas putas tarjetitas, para comprobar que se cumplen llevan por detrás los **criterios de aceptación**.

> Ejemplo de historia de usuario:
> *As an [actor] I want [action] so that [achievement].*

Una vez tenemos todas las historias de usuario, éstas, a su vez se dividen en **tareas** concretas (**tasks**) y son desarrolladas por los programadores. **Por detrás, cada tasks tiene asociado un conjunto de tests**.

> Extra:
> * Las historias de usuario se van poniendo en el **Release plan board** para ver que historias se harán en cada release (priorizando).
> * Luego en el **Iteration plan board** vemos como se dividen las historias de usuario en tareas concretas.
> * Y en el **Task board** se van poniendo las tasks por hacer, que no es muy distinto de lo que hace Scrum.

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
