# Análisis de pruebas

Una vez hechas las pruebas, tenemos que evaluar si hemos alcanzado las metas definidas y emitir un informe

Tenemos que poder **medir** si hemos alcanzando nuestros objetivos.

Si parte de nuestro código no se ejecuta durante las pruebas, es que no está siendo probado.
Esto es la COBERTURA de código: analizar cual es la EXTENSIÓN de nuestras pruebas.

### Cobertura de código

* A	nivel	de	bloque
* A	nivel	de	función
* A	nivel	de	decisiones
* A	nivel	de	condiciones


* Statement coverage: un 100% significa que hemos ejecutado cada sentencia (línea)
* Branch (or decision) coverage: un 100% significa que hemos ejecutado cada rama o DECISIÓN en sus vertientes verdadera y falsa.
* Condition coverage: un 100% significa que hemos ejercitado cada CONDICIÓN
* Multicondition coverage: un 100% de cobertura significa que hemos ejercitado todas las posibles combinaciones de condiciones.
* Condition decision coverage
* Loop coverage: un 100% de cobertura implica probar el bucle con 0 iteraciones, una iteración y múltiples iteraciones.
* Path coverage: un 100% de cobertura implica que se han probado todos los possibles caminos de control

* NIVEL	0	,	el	cual	se	define	como:	“test	whatever	you	test;	let the	users	test	the	rest”
* NIVEL 1: cobertura de líneas (100%)
* NIVEL 2: cobertura de ramas (100%)
* NIVEL 3: cobertura de condiciones (100%)
* NIVEL 4: cobertura de condiciones (100%) y decisiones (100%)
* NIVEL 5: cobertura de condiciones múltiples (100%)
* NIVEL 6: cobertura de bucles (100%)
* NIVEL 7: cobertura de caminos (100%)

### Analizadores automáticos de cobertura

**Instrumentan el código**: añadir código adicional para poder obtener una métrica de cobertura.

En Java:
* Instrumentación del código fuente.
* Instrumentación del byte-code. (Esto es lo que hacemos con Cobertura)
* Máquina virtual de Java modificada

### Cobertura

* Instrumenta el **bytecode** de Java.
* Basado en JCoverage.
* Puede utilizarse desde terminal, con Ant o con Maven
* Cobertura genera un informe con 3 métricas concretas:
  * Line coverage
  * Branch coverage
  * Complejidad ciclomática
