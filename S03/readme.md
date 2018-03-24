# Drivers


|                 | Producción       | Tests                      |
|-----------------|------------------|----------------------------|
| **Código**      | `src/main/java`  | `src/test/java`            |
| **Ejecutables** | `target/classes` | `target/test-classes`      |
| **Informes**    |                  | `/target/surefire-reports` |

## Fixtures

* `@Test`: Métodos de test, el orden en el que se ejecutan no está garantizado.
* `@Before`, `@After`: Se ejecutan al principio o al final de cada `@Test`.
* `@BeforeClass`, `@AfterClass`: Se ejecutan al principio o al final del conjunto de tests.

## POM

```xml
<project ...>
  
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  
</project>
```
`<scope>test</scope>` indica que la librería sólo se utiliza desde el código de test (`/src/test/java`)

## Fases

#### compile
* Se compilan los fuentes `src/main/java` en `target/classes`
* GOAL por defecto: `compiler:compile`


#### test-compile
* Se compilan los tests `src/test/java` en `target/test-classes`
* GOAL por defecto: `compiler:testCompile`


#### test
* Se ejecutan los tests unitarios (`target/test-classes`) y se generan artefactos en `/target/surefire-reports`
* GOAL por defecto: `surefire:test`

## Catergorías
Permiten hacer agrupaciones de tests

```java
public interface IntegrationTests{} // Definimos la categoría

@Category(IntegrationTests.class) // Anotamos  una clase
public class AccountIntegrationTest
{
	@Test
	public void thisC1() {...}
}

public class OtherIntegrationTest
{
	@Test
	@Category(IntegrationTests.class) // Anotamos un método
	public void thisC1() {...}
}
```
