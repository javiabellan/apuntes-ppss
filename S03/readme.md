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

## POM

```xml
<project ...>

  <!----------------------- COORDENADAS ----------------------------->
  <groupId>    ppss         </groupId>
  <artifactId> proyecto     </artifactId>
  <version>    1.0-SNAPSHOT </version>
  <packaging> jar           </packaging> <!-- OPCIONAL: por defecto es jar -->

  <!----------------------- PROPIEDADES ----------------------------->
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

 <!----------------------- DEPENDENCIAS ----------------------------->
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-surefire-plugin</artifactId>
      <version>2.20</version>
      <configuration> <!-- ejecutar solo una categoría (grupo) de tests -->
        <groups>com.mycompany.SlowTests</groups>
      </configuration>
    </plugin>

    <plugin>
      <groupId>...</groupId>
      <artifactId>...<artifactId>
      <version>...</version>
      <execution>
        <phase>FaseC</phase>
        <goal>g2</goal>
      </execution>
    </plugin>
  </plugins>


</project>
```
`<scope>test</scope>` indica que la librería sólo se utiliza desde el código de test (`/src/test/java`)
