# Dependencias externas

#### Conceptos:
* Entradas directas: Parámetros del SUT (proporcionadas en el test)
* Entradas indirectas: Componentes que invoca el SUT (valores de retorno, actualización de parámetros o excepciones)
* Seam: Un sitio en el código, donde se puede alterar el comportamineto, sin modificar el código.

#### Pasos:
1. Identificar las dependencias externas del SUT.
2. Refactorizar si no es testable. (puede ser probado de forma aislada) (Refactorizar no se considera modificar código)
3. Implementación ficticia (doble)
4. Implementar drivers:
  * Verificación basada en el estado: Pruebas de unidades. Solo interesa el resultado del SUT. Stub.
  * Verificación basada en el comportamiento: Pruebas de intergración. Mock


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

Hay una dependendia externa al llamar a metodo() que está definida en otro sitio
```java
puclic class Objeto
{
	public void metodo(){...}
}
```

## 2. Refactorizar

Existen distintas formas:

* Interfaz
  * Recibir la interfaz a nivel de constructor de la clase del SUT.
  * Recibir la interfaz a través de un método setter/getter de la clase del SUT.
  * Recibir la interfaz justo antes de su invocación desde el propio SUT usando:
    * un parámetro en el método
    * una clase factoría
    * un método de factoría local
    * variaciones de las anteriores
* Herencia

## Interfaz

Poner una capa de abastracción (interfaz) al objeto de la dependencia externa
```java
public interface InterfazObjeto
{
	void metodo();
}

public class Objeto: InterfazObjeto // El objeto ahora implementa la interfaz
{
	public void metodo(){...}
}
```

La dependencia externa de la sut, ahora se declara con la interfaz:
```java
puclic class ClassSUT
{
	public sut()
	{
		InterfazObjeto objeto = new Objeto();
		objeto.metodo(); // Dependencia externa. SUT no testable
	}
}
```

Implementación del stub (paso 3)
```java
// El stub también implementa la interfaz
public class ObjetoStub: InterfazObjeto
{
	public void metodo(){//codigo controlado}
}
```

Hasta aquí, tenemos la implementación ficicia del stub, pero nuestro código sigue llamando al método original. Tenemos que cambiar eso, tenemos que introducir el seam. Hay muchas formas de hacerlo:

* Recibir la interfaz a nivel de constructor de la clase del SUT.
* Recibir la interfaz a través de un método setter/getter de la clase del SUT.
* Recibir la interfaz justo antes de su invocación desde el propio SUT usando:
  * un parámetro en el método
  * una clase factoría
  * un método de factoría local
  * variaciones de las anteriores

---

## A nivel de constructor

En la clase del SUT, poner un nuevo constructor con un parámetro (o un parámetro en el existente) **que acepte un obejeto que implente la interfaz**

```java
puclic class ClassSUT
{
	private InterfazObjeto objeto;
	
	ClassSUT()
	{
		objeto = new Objeto(); // Código para producción
	}
	
	ClassSUT(InterfazObjeto obj)
	{
		objeto = obj; // Código para test
	}
	public sut()
	{
		objeto.metodo();
	}
}
```

Test:
```java
public class Tests
{
	@test
	public void test()
	{
		ObjetoStub myFakeObj = new ObjetoStub();        // Stub
		ClassSUT   classSUT  = new ClassSUT(myFakeObj); // Pasar stub al constructor
		
		bool resultado = classSUT.sut();      // SUT (que llamará al método del del stub)
		assert(resultado, resultadoEsperado);
	}
}
```

## A través de un método setter/getter

## Un parámetro en el método

The parameter injection method is trivial: you send in an instance of a (fake) dependency to the method in question by adding a parameter to the method signature.

## Método de factoría local

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
