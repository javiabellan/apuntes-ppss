# Pruebas de aceptación

Objetivo: valorar en qué **grado** el software desarrollado satisface los **criterios de aceptación**/expectativas del cliente.

Los criterios de aceptación deben ser DEFINIDOS y ACORDADOS entre el desarrollador y el cliente.

Tras las pruebas de sistema, se ejecutan las pruebas de aceptación. Y si son aceptadas, se lleva producción.

Las pruebas de acep. deben basarse en la especificación y no en la implementación (caja negra, functional testing).


### Dirigias por

* **El cliente** (User Acceptance Testing, UAT)
  * Pruebas alpha: El desarrollador dice lo (poco) que hay, y el user prueba?
  * Pruebas beta: Usuarios reales en un entorno real. El desarrollador observa?
* **Los desarrolladores** (Business Acceptance Testing)



### Propiedades emergentes

Prueban las propiedades emergentes: Emergen al final cuando está ya todo integrado.

Cualquier atributo incluido en los criterios de aceptación es una propiedad emergente.

* **Funcionales**: Propósito del sistema
  * Basado en **requerimientos**: Validación de que el sistema cumple los requerimeintos
  * Basado en **escenarios**: Como se debería usar el sistema. (Katalon y Selenium WebDriver)
* **No funcionales**: Comportamineto (-idad)
  * **Pruebas de rendimiento**: (JMeter)

#### Requerimiento
Ejemplo:
> Si el paciente es alérgico, debe aparecer un warining en la preescripción.

Una prueba basada en requerimiento necesita varios tests para "cubrir" todo el requerimiento.

Pruebas que se harían:

> * Agraga un paciente con una alergia. Comprobar que salta el warning
> * Agraga un paciente con 3 alergias. Comprobar que salta el warning
> * Agraga un paciente sin alergias. Comprobar que no salta el warning

#### Escenario
Las pruebas basadas en escenarios normalmente prueban varios requerimientos en un mismo escenario.

Y además de probar los requerimientos individuales, prueban la combinación de ellos.

---

# Katalon (Selenium)

Katalon Automation Recorder (KAR) es una herramienta para automatizar **pruebas funcionales** (normalmente basadas en escenarios) sobre aplicaciones Web.

Permite grabar "scripts" de pruebas como si fuese un usuario en el navegador. Y luego se ejecutan

Esas acciones acciones son comandos "selenese":

| Command       | [Target]                 | [Value]    |
|---------------|--------------------------|------------|
| open          | https://www.ua.es        |            |
| verifyText    | //div[id='title']        | UA         |
| click         | link=Estudios            |            |
| type          | name=rama                | ciencias   |
| verifyNotText | /html/body/div[2]        | filologia  |
| storeText     | //div[@id='informatica'] | miGrado    |
| echo          | ${miGrado}               |            |
| storeEval     | ${miGrado} + " es guay"  | comentario |
| echo          | ${comentario}            |            |

### Commands
Hay 3 tipos:
* **Actions**: Hacen cosas: `click`, `clickAndWait,`, `type`
* **Accessors**: Usar variables: `storeText`, `storeEval`, `storeTitle`
* **Assertions**: Comprueban cosas:
  * Verify: no detiene la ejec. `verifyText`, `verifyNotText`, `verifyElementPresent`
  * Assert: detiene la ejec. `assertTitle`
  * WaitFor: Espera antes de detener. `waitForPageToLoad`, `waitForElementPresent)` (tras `click`)

### Locators (Target)
* `id=xxx`: Atributo id
* `name=xxx`: Atributo name
* `/html/body/form[1]`: Ruta absoluta
* `//form[1]`: Ruta relativa
* `link=Continue`: Se usa en los clicks

# Ejercicios

#### a) Dados los tests implementados en prácticas con Katalon Recorder, indica con cuál de los dos métodos de diseño de casos de prueba vistos en clase hemos obtenido dichos tests. Justifica claramente tu respuesta.

Caja negra: pruebas funcionales:

* ~Pruebas unitarias~
  * ~Método de particiones equivalentes~
* ~Pruebas de sistema~
  * ~Método de diagramas de transición de estados~
* Pruebas de aceptación
  * ~Método de pruebas basado en requerimientos~
  * Método de pruebas basado en escenarios. Porque probamos varios requerimientos (login, compra…)
---

#### b) Podemos integrar la ejecución de los tests citados en el apartado a) con la construcción del proyecto? Justifica claramente tu respuesta, indicando por qué no podemos hacerlo, si tu respuesta es negativa, y cómo podemos hacerlo si tu respuesta es afirmativa.

No, con Katalon es imposible, porque es una herramienta que no se puede invocar desde Maven.

---

#### c) Pon un ejemplo de caso de prueba diseñado con el método basado en escenarios para realizar una prueba de uso Bitbucket, de forma que comprobemos que la práctica P08, que está formada por 2 ejercicios se ha subido correctamente (el escenario debe contemplar las acciones previas a la consulta en Bitbucket). Implementa un driver utilizando comandos selenese para dicho caso de prueba

Test con varios verify, cada vez que cambias de página verificas titulo.
