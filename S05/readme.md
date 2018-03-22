# Dependencias externas

#### Conceptos:
* SUT: Unidad a probar
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
public class ClassSUT
{
	public sut()
	{
		Objeto objeto = new Objeto();
		objeto.metodo(); // Dependencia externa. SUT no testable
	}
}
```

Hay una dependendia externa al llamar a metodo() que está definida en otro sitio:
```java
public class Objeto
{
	public void metodo(){...}
}
```

## 2. Refactorizar (src/main)

Creo que si se declara la dependendia externa (`new`) dentro del SUT, es cuando hay que refactorizar.

Existen 2 formas principales:

* Interfaz
* Herencia

## Interfaz

Poner una capa de abastracción (interfaz) a la dep. externa.
```java
public interface InterfazObjeto
{
	public void metodo();
}
```

La dependencia externa de la sut, ahora se declara con la interfaz:
```java
public class ClassSUT
{
	public sut()
	{
		InterfazObjeto objeto = new Objeto();
		objeto.metodo();
	}
}
```

El objeto (dependencia externa) ahora implementa la interfaz. Poner `implements` y `@Override`.

```java
public class Objeto implements InterfazObjeto
{
	@Override
	public void metodo(){...}
}
```


> #### Extra (paso 3)
> El doble del objeto (stub) también implementa la interfaz
> ```java
> public class ObjetoStub implements InterfazObjeto
> {
>	private int resultado;
>	
>	public ObjetoStub(int r) { this.resultado = r; }
>	
>	@Override
>	public void metodo(){//codigo controlado}
> }
> ```

Hasta aquí, tenemos la implementación ficticia del stub, pero nuestro código sigue llamando al método original. Tenemos que cambiar eso, tenemos que introducir el seam. Hay muchas formas de refactorizar con **interzaz**:

* A nivel de **constructor** de la clase del SUT.
* A través de un método **setter/getter** de la clase del SUT.
* Con una **clase factoría**
* Con un **método de factoría local**
* Con un parámetro en el método (no se ha visto)
* Variaciones de las anteriores (no se ha visto)

---

## A nivel de constructor

En la clase del SUT, poner un nuevo constructor con un parámetro (o un parámetro en el existente) **que acepte un objeto que implente la interfaz**

```java
public class ClassSUT
{
	private InterfazObjeto objeto;
	
	ClassSUT() // Código para producción
	{
		objeto = new Objeto();
	}
	
	ClassSUT(InterfazObjeto obj) // Código para test
	{
		objeto = obj;
		// Si hay mas dep. ext., seguir rellenando el constructor.
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
		
		bool resultado = classSUT.sut(); // SUT (que llamará al método del del stub)
		assert(resultado, resultadoEsperado);
	}
}
```

## A través de un método setter/getter
Añadir un get-set por cada dependencia externa
```java
public class ClassSUT
{
	private InterfazObjeto objeto;
	
	ClassSUT()
	{
		objeto = new Objeto();
	}
	
	// Código para test
	public void setObjeto(InterfazObjeto o) {objeto=o;}
        public InterfazObjeto getObjeto() {return apellido;} // El getter creo que no es necesario
	
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
		ObjetoStub myFakeObj = new ObjetoStub(); // Stub
		ClassSUT   classSUT  = new ClassSUT();
		classSUT.setObjeto(myFakeObj); // Pasar stub por setter
		
		bool resultado = classSUT.sut(); // SUT (que llamará al método del del stub)
		assert(resultado, resultadoEsperado);
	}
}
```

## Clase factoría
Se inicializa la dependencia en el construtor, pero a través de una clase factoría responsable de crear objetos.

```java
public class FactoriaObjeto
{
	private static InterfazObjeto objeto = null;
	
	public static InterfazObjeto create()
	{
		if (objeto == null)
			return new Objeto(); // Código producción
		else
			return objeto; // Código test
	}
	
	static void setObjeto(InterfazObjeto obj) // Llamar en código test
	{
		objeto = obj;
	}
}
```

Y entonces el construtor de la clase SUT llama a factoria create
```java
public class ClassSUT
{
	private InterfazObjeto objeto;
	
	ClassSUT() // Código para producción y test
	{
		objeto = FactoriaObjeto.create()
	}
	
	public sut()
	{
		objeto.metodo();
	}
}
```

Otra forma de llamar a la clase factoría si solo necesitas la dependencia externa en un metodo
```java
pucbic class ClassSUT
{
	public sut()
	{
		InterfazObjeto objeto = FactoriaObjeto.create()
		objeto.metodo();
	}
}
```

Y en los tests
```java
ObjetoStub myFakeObj = new ObjetoStub(); // Stub
myFakeObj.setOpcion(x);

FactoriaObjeto.setObjeto(myFakeObj); // Al ser un metodo static, simplenete lo llamas para cambiar el comportamineto del constructor de ClassSUT

ClassSUT classSUT  = new ClassSUT(); // Ya tiene el stub inyectado
```

## Método de factoría local

```java
public class ClassSUT
{
	public Objeto getObjeto() // Método de factoría local
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

> #### Extra (paso 3) (en src/test)
> Sobrescribrir el la clase sut,
> ```java
> public class TestableClassSUT extends ClassSUT
> {
> 	private Objeto objeto;
> 
> 	@Override
> 	public Objeto getObjeto() {return objeto;}
> 
> 	public void setObjeto(Objeto o) {this.objeto = o;}
> }
> ```
> 
> Stub
> ```java
> public class ObjetoStub extends Objeto
> {
> 	int result;
> 
> 	public void setResult(int salida) { this.result = salida;}
> 
> 	@Override
> 	public int metodo(params...) {return result;}
> }
> ```
>
> Test
> ```
>TestableClassSUT sut = new TestableClassSUT();
>
> ObjetoStub oStub = new ObjetoStub();
> oStub.setResult(x);
>
> sut.setObjeto(oStub); // Inyectar stub
>```

## Un parámetro en el método

Es simplemente poner un parametro en el SUT y pasar por ahí el objeto dependencia.
