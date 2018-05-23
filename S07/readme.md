# Pruebas de integración

Objetivo:
Encontrar defectos de la INTERACCIÓN entre las unidades.
* Comprobar que el sistema funciona cuando las uds son cominadas
* Las unidades deben tener una buena interfaz para usarlas.
* Construir version operativa del sistema poco a poco
  * Agregar uds de forma incremental
  * Comprobar que no perturban el sistema (pruebas de regresión)
* Descubrir errores asociado a las interfaces de las uds

### Tipos de interfaz
* **Parámetros**: Los métodos de un obj usan parametros.
* **Memoria compartida**: Un componente escribe en memoria y otro lee.
Ej sensores y sist. embebidos.
* **Interfaces procedurales**: Un componente (clase) encapsula un cjto. de procedimientos (métodos) que
pueden llamarse desde otras clases.**
* **Paso de mensajes**: Solicitar información y devolver contenido. Ej: cliente-servidor.

### Consejos.
* Probar los extremos de los rangos de los params.
* Si se pasan punteros, probar punetros nulos.
* Probar que falle.
* Pruebas de estrés en paso por mensajes. Medir tiempos..
* Tests de memoria compartida que sigan el orden que se haría en el programa.

### Estrategias
Recuerda que estamos probando interacción entre componentes, no componetes en sí.

* **Big bang**: Todo a la vez.
* **Top-down**: De mayor nivel de abstracción a menos.
* **Bottom-up**: De menor nivel de abstracción a mayor.
* **Sandwich**: Mezcla de las 2 antariores.
* **Dirigida por los riesgos**: Componentes con más probabilidad de error primero.
* **Dirigida por las funcionalidades**: Ir probando compontentes de una funalida en concreto y luego de otra y así.

**Pruebas de REGRESIÓN**: consisten en repetir las pruebas realizadas cuando integramos un nuevo componente.
