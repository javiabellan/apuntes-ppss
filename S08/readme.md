# Pruebas de aceptación

Objetivo: valorar en qué **grado** el software desarrollado satisface los **criterios de aceptación**/expectativas del cliente.

Los criterios de aceptación deben ser DEFINIDOS y ACORDADOS entre el desarrollador y el cliente.

Tras las pruebas de sistema, se ejecutan los tests de aceptación. Y si estos son aceptados, se lleva producción.

Las pruebas deben basarse en la especificación y no en la implementación (caja negra, functional testing).


### Dirigias por

*  **El cliente** (User Acceptance Testing, UAT)
  * Pruebas alpha: El desarrollador dice lo (poco) que hay, y el user prueba?
  * Pruebas beta: Usuarios reales en un entorno real. El desarrollador observa?
* **Los desarrolladores** (Business Acceptance Testing)



### Propiedades emergentes

Prueban las propiedades emergentes: Emergen al final cuando está ya todo integrado

* **Funcionales**: Propósito del sistema
  * Basado en **requerimientos**: Validación de que el sistema cumple los requerimeintos
  * Basado en **escenarios**: Como se debería usar el sistema
* **No funcionales**: Comportamineto (-idad)

##### Requerimineto
Ejemplo:
> Si el paciente es alérgico, debe aparecer un warining en la preescripción.

Una prueba basada en requerimiento necesita varios tests para "cubrir" todo el requerimiento.

Pruebas que se harían:

> * Agraga un paciente con una alergia. Comprobar que salta el warning
> * Agraga un paciente con 3 alergias. Comprobar que salta el warning
> * Agraga un paciente sin alergias. Comprobar que no salta el warning

##### Escenario
Las pruebas basadas en escenarios normalmente prueban varios requerimientos en un mismo escenario.
