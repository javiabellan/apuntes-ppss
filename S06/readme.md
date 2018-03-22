# Dependencias 2

Diferencias entre Stub y Mock:

| Stub                                          | Mock                                                     |
|-----------------------------------------------|----------------------------------------------------------|
| Si sólo quiero controlar lo que devuelve      | Si además quiro controlar los parámetros, y las llamadas |
| Proporcionar sólo entradas indirectas al sut  | Observar también las salidas indirectas del sut          |
| Verificación basada en el **estado**          | Verificación basada en el **comportamiento**             |

Pasos con Easymock:

|                    | Stub                                    | Mock                   |
|--------------------|-----------------------------------------|------------------------|
| 1. Crear           | `S s = EasyMock.createNiceMock(S.class);`  |`Mock` ó `StrictMock`      |
| 2. Esperar valor   | `EasyMock.expect(s.m()).andStubReturn(x);` | EasyMock.expect().andReturn()  |
| 2. Esperar execep  | `EasyMock.expect().andStubThrow()`       | EasyMock.expect().andThrow()   |
| 2. Esperar void    | `EasyMock.asStub()`                      |  |
| 3. Listo           | `EasyMock.replay(s)`                      | `EasyMock.replay(m)`  |
| 4. registrar llamadas |                                        | `EasyMock.verify(m)`  |

---

## Si la dependendia externa es un método de la clase SUT

Hacer como un mock de la clase sut, y añadirle ese método
```java
ClaseSUT cSUT = EasyMock.partialMockBuilder(ClaseSUT.class).addMockedMethod("metodoDepExt").createMock();
ClaseSUT cSUT = EasyMock.createMockBuilder(ClaseSUT.class).addMockedMethods("metodoDepExt").createMock(); // otra forma no vista
```

Y en la expectativa ponemos los que nos de la gana:
```java
EasyMock.expect(cSUT.metodoDepExt()).andReturn(cal);
```

La forma tradicional de inyectar ese método, es con método de factoría local:

Heredar la ClaseSUT, en una claseSutTestable, y sobrescribir el método para que devuelva lo que queramos, (y acurdate de inicilaizar el resultado esperado con un contructor o un setter).

## Si la dependendia externa está definida como atributo en la clase SUT

Además, esto de crear un mock de la clase SUT también es útil para sobreescribir atributos privados
(que son dependencias externas). Esto es superútil si no tenemos ni ideo de como inyectar el stub.
```java
// Mira, te creas tu stub de mierda
ClaseDeMiAtributo stub = EasyMock.createMock(ClaseDeMiAtributo.class);
EasyMock.expect(stub.suMetodo()).andReturn(xxx);

// Y se lo metes gracias a tu cSUT
cSUT.atributo = stub;
```



---
## Easymock

Imagina que tienes una nueva clase a probar `ClassTested.java`

```java
public class ClassTested
{
	private Collaborator listener;

	public void setListener(Collaborator listener) {this.listener = listener;}

	public void addDocument(String title, String document) {//Llama a listener}
}
```

Quieres probar la clase, así que añades la dependencia de `easymock` a tu `pom.xml`
```xml
<dependency>
  <groupId>org.easymock</groupId>
  <artifactId>easymock</artifactId>
  <version>3.5</version>
  <scope>test</scope>
</dependency>
```

`addDocument()` llama a la dependencia `listener`, así que le ponemos ina interfaz.
```java
public interface Collaborator {
    void documentAdded(String title);
}
```

Para crear el doble en easymock se le puede pasar una clase o una interfaz

## Stub

```java
public class ClaseTest
{
	@Test
	public void testGenerarFactura() throws Exception
	{
		Cliente cli  = new Cliente(...);
		ClaseSut claseSut = new ClaseSut();

		ClaseDepEx stub = EasyMock.createNiceMock(ClaseDepEx.class); // 1) Stub
		EasyMock.expect(stub.metodo(anyObject()).andStubReturn(10);  // 2) Expectativas
		EasyMock.replay(stub);                                       // 3) Listo

		Factura expResult = new Factura(...);
		Factura result    = claseSut.sut(cli,stub);
		assertEquals(expResult, result);
	}
}
```

> * anyObject() → "cualquier objeto"
> * anyChar()   → "cualquier char"
> * anyFloat()  → "cualquier float"
> * anyInt()    → "cualquier int"
> * anyString() → "cualquier objeto String"


## Mock

```java
// 1. Creamos el doble (mock)
ClaseDoble mock1 = EasyMock.createMock(ClaseDoble.class); // Si sólo hay una única invocación del doble
ClaseDoble mock2 = EasyMock.createStrictMock(ClaseDoble.class);

// 2. Programamos las expectativas
mock1.met1(); //invocación de un método void sin parámetros
mock1.met2(8); //invocación de un método void con parámetros

//expectLastCall() hace referencia a la última invocación (en este caso al método met2(8))
expectLastCall().andThrow(new MyException());        // met2() devuelve una excepción
expect(mock1.met3("string")).andReturn(34);          // met3() devuelve un valor
expect(mock1.met4()).andThrow(new OtherException()); // met4() devuelve una excepción

replay(mock1); // 3. Indicar doble listo (cambiamos el doble a modo replay)
verify(mock1); // 4. Verificar que se realizan las invocaciones al mock desde SUT
```

### Ejemplo

```java
public class Currency
{
	private double amount;
	private String type;

	public Currency(double a, String t) {amount=a; t=type;}

	public Currency toEuros(ExchangeRate er) // SUT
	{
		if (type!="EUR")
		{
			double rate = er.getRate(type, "EUR"); // DEP EXT
        	return new Currency(amount*rate, "EUR");
		}
		else {return null;}
	}
}

pubic class CurrencyTest
{
	@Test
	public void testToEuros()
	{
		Currency testObject = new Currency(2.50, "USD");
		Currency expected   = new Currency(3.75, "EUR");
		
		ExchangeRate mock = EasyMock.createMock(ExchangeRate.class); // 1. Crear mock
		EasyMock.expect(mock.getRate("USD", "EUR")).andReturn(1.5);  // 2. Expectativa (que se llame 1 vez y devuelva 1.5)
		EasyMock.replay(mock);                                       // 3. Mock listo

		Currency actual = testObject.toEuros(mock); // Ejecutar sut
		assertEquals(expected, actual);             // Verificacion noraml
		EasyMock.verify(mock);                      // Verificación del mock
	}
}
```
