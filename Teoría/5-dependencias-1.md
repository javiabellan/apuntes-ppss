# Dependencias externas

#### Conceptos:
* Entradas directas: Parámetros del SUT (proporcionadas en el test)
* Entradas indirectas: Componentes que invoca el SUT (valores de retorno, actualización de parámetros o excepciones)
* Seam: Un sitio en el código, donde se puede alterar el comportamineto, sin modificar el código.

#### Pasos:
1. Identificar las dependencias externas del SUT.
2. Refactorizar si no es testable. (puede ser probado de forma aislada) (Refactorizar no se considera modificar código)
3. Implementación ficticia (doble)
4. Implementar drivers:
  * Verificación basada en el estado: Pruebas de unidades. Solo interesa el resultado del SUT. Stub.
  * Verificación basada en el comportamiento: Pruebas de intergración. Mock


## 1. Identificar dependencias externas.

```java
puclic class ClassSUT
{
	public sut()
	{
		Objeto objeto = new Objeto();
		objeto.metodo(); // Dependencia externa. SUT no testable
	}
}
```

## 2. Refactorizar

Formas libro:
* Extract an interface to allow replacing underlying implementation.
* Receive an interface at the constructor level.
* Receive an interface as a property get or set.
* Get a stub just before a method call.
* Inject stub implementation into a class under test.

Formas diapositivas:
* Interfaz: un parámetro en el método
* Interfaz: una clase factoría
* Interfaz: un método de factoría local
* Interfaz: variaciones de las anteriores
* Herencia

### Método de factoría local


Paso 2: Refactorizar (en src/main)
```java
puclic class ClassSUT
{
	public Objeto getObjeto()
	{
		Objeto o = new Objeto();
		return o;
	}

	public sut()
	{
		Objeto objeto = getObjeto();
		objeto.metodo(); // Dependencia externa
	}
}
```

Paso 3: Doble (en src/test)
```java
public class TestableClassSUT extends ClassSUT
{
	private Objeto objeto;

	@Override
	public Objeto getObjeto() {return objeto;}

	public void setObjeto(Objeto o) {this.objeto = o;}
}
```

Paso 3: Stub
```java
public class ObjetoStub extends Objeto
{
	int result;

	public void setResult(int salida) { this.result = salida;}

	@Override
	public int metodo(params...) {return result;}
}