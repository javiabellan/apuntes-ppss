# Dependencias externas 2

En el SUT se puede:

| Stub                             | Mock                                     |
|----------------------------------|------------------------------------------|
| Controlar entradas indirectas    | Verificar salidas indirectas             |
| Verificación basada en el estado | Verificación basada en el comportamiento |

## EasyMock
Es un framework que nos permite "implementar" dobles de forma automática.
Todo va en la clase `Test`.

#### 1. Crear el stub
```java
// Parametro: una clase (o interfaz)
// Devuelve:  un objeto que será nuestro doble
stub = EasyMock.createNiceMock(Ejemplo.class);
```

#### 2. Programar expectativas
```java
EasyMock.expect().andStubReturn() // Cuando el método devuelva un valor
EasyMock.expect().andStubThrow()  // Cuando el método lance una excepción
EasyMock.asStub()                 // Cuando el método devuelva void
```

#### 3. Stub listo para ser llamado
```java
EasyMock.replay()
```

#### Ejemplo
```java
public class ClaseTest
{
	@Test
	public void testSut() throws Exception
	{
		ClaseSut claseSut = new ClaseSut();

		Stub stub = EasyMock.createNiceMock(Stub.class);            // 1) Crear
		EasyMock.expect(stub.metodo(anyObject()).andStubReturn(10); // 2) Expectativa
		EasyMock.replay(stub);                                      // 3) Listo

		ClaseReturn expResult = new ClaseReturn(...);
		ClaseReturn result    = claseSut.sut(stub);
		assertEquals(expResult, result);
	}
}
```
