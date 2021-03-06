# Pruebas de integración

Objetivo:
Encontrar defectos de la **interacción** entre las unidades.
* Se necesitan mínimo 2 uds para realizar pruebas de integración
  * 2 métodos
  * 1 método y la BBDD
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
pueden llamarse desde otras clases.
* **Paso de mensajes**: Solicitar información y devolver contenido. Ej: cliente-servidor.

### Consejos.
* Probar los extremos de los rangos de los params.
* Si se pasan punteros, probar punteros nulos.
* Probar que falle.
* Pruebas de estrés en paso por mensajes. Medir tiempos...
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

---

# DbUnit
Basado en JUnit. Controla la dep. ext. de la BBDD. Dependencias en el POM:

```xml
<dependencies>
    <!--Librería DbUnit-->
    <dependency>
        <groupId>org.dbunit</groupId>
        <artifactId>dbunit</artifactId>
        <version>2.5.4</version>
        <scope>test</scope>
    </dependency>

    <!--Librería para acceder a una BD MySql-->
    <dependency> 
        <groupId>mysql</groupId> 
        <artifactId>mysql-connector-java</artifactId> 
        <version>5.1.38</version>
	<scope>test</scope>
    </dependency>
</dependencies>
```

### Escenario típico

```
project
	src
		java
			ppss
				Customer.java
				CustomerFactory.java
	test
		java
			ppss
				CustomerTest.java
		resources
			sql
				createTableCustomer.sql
			customer_init.xml
			customer_expected.xml
```

#### Código fuente
La idea típica es hacer una clase java que simule una tabla (cada instancia una fila de la tabla).
Esta clase es simplemte para guardar los datos como atributos, así que solo necesita getters y setters.

Luego aparte, se hace una clase factoría para manejar la clase anterior con la BBDD.
Métodos tipicos de esta clase factoría son:
* `public Clase create(int id, String firstName, String lastName)`
* `public void insert(Clase clase) throws SQLException `
* `public void delete (Clase clase) throws SQLException`
* `public void update(Clase clase) throws SQLException`
* `public Clase retrieve(int id) throws SQLException`


#### Código de pruebas
1. Restaurar BBDD **antes de cada test** (no después).
2. Cargar datos de pruebas a la BBDD.
3. Usar DbUnit para aserciones.

`dataset-init.xml`: Fichero que se usa para cargar inicialmente un dataset vacío
```xml
<?xml version="1.0" encoding="UTF-8"?>
<dataset>

    <customer />

</dataset>
```

`dataset-expected.xml`: Fichero que se usa como dataset de resultado esperado
```xml
<?xml version="1.0" encoding="UTF-8"?>
<dataset>
	<!--TABLA CUSTOMER-->
	<customer id="1"
		firstname="John"
		street="1 Main Street" />
	
	<!--TABLA USER-->
	<user id="1"
		login="John"
		password="John" />
	<user id="2"
		login="Karl"
		password="Karl" />
</dataset>
```

```java
private Factoria factoria;
private IDatabaseTester databaseTester; // Instancia de IDatabaseTester para acceder a la BD
    
@Before
public void setUp() throws Exception
{
    // Fijamos los datos para acceder a la BD
    databaseTester = new JdbcDatabaseTester("driver", "url", "login", "pass");

    // Dataset inicial: con una tabla vacía por ejemploa
    DataFileLoader loader = new FlatXmlDataFileLoader();
    IDataSet dataSet = loader.load("/dataset-init.xml");	 

    databaseTester.setDataSet(dataSet);  // Determina los datos de prueba
    databaseTester.onSetup();            // Realiza una operación CLEAN_INSERT
    // CLEAN_INSERT = Borra los datos de las tablas del dataset, e inserta el dataSet en la BBDD
    
    factoria = Factoria.getInstance();
}

@Test
public void testInsert() throws Exception
{
    /////////////////////////////////////////////////////// RESULTADO REAL

    // Datos de entrada del caso de prueba
    Clase c = factoria.create(1,"Javi", "Abellan");
    c.setStreet("Calle falsa 123");
    c.setCity("Elda");
    
    factoria.insert(c); // Método a probar

    // Recuperamos la conexión con la BD
    IDatabaseConnection connection = databaseTester.getConnection();

    // Configuramos el tipo de la conexión como  mysql
    DatabaseConfig dbconfig = connection.getConfig();
    dbconfig.setProperty("http://www.dbunit.org/properties/datatypeFactory", new MySqlDataTypeFactory());

    // Recuperamos los datos de la BD (tablas)
    IDataSet databaseDataSet = connection.createDataSet();

    // Recuperamos una tabla
    ITable actualTable = databaseDataSet.getTable("customer");
    
    
    /////////////////////////////////////////////////////// RESULTADO ESPERADO
    
    // introducimos los valores esperados desde el fichero customer-expected.xml
    DataFileLoader loader = new FlatXmlDataFileLoader();
    IDataSet expectedDataSet = loader.load("/dataset-expected.xml");
    
    ITable expectedTable = expectedDataSet.getTable("customer");
    
    
    /////////////////////////////////////////////////////// COMPARAR
    
    Assertion.assertEquals(expectedTable, actualTable);
}
```



## Ciclo de vida (Maven)

| Fase                   | Goal por defecto        |
|------------------------|-------------------------|
| compile                | compiler:compile        |
| process-test-resources | resources:testResources |
| test-compile           | compiler:testCompile    |
| test                   | surefire:test           |
| package                | jar:jar                 |

Luego vienen las fases de integración, Maven contempla cuatro. **Estas fases no tienen goals por defecto**.

| Fase                  | Descripción                                                   | Goal a añadir en el POM   |
|-----------------------|---------------------------------------------------------------|---------------------------|
| pre-integration-test  | Iniciar algún servicio (BBDD, servidor web)                      | sql:execute               |
| integration-test      | Ejecuta los `@Test` de `IT*.java`, `*IT.java`, `*ITCase.java` | failsafe:integration-test |
| post-integration-test | Detener servicios (restaurar)                                 |                           |
| verify                | Comprobar que todo está listo                                 | failsafe:verify           |


Las goal se añaden en los plugins del POM:

```xml
<build>
    <plugins>

        <!--Plugin failsafe para ejecutar los tests de integración-->
        <plugin> 
            <groupId>org.apache.maven.plugins</groupId> 
            <artifactId>maven-failsafe-plugin</artifactId> 
            <version>2.20</version> 
            <executions> 
                <execution> 
                <goals> 
                    <goal>integration-test</goal> 
                    <goal>verify</goal> 
                </goals> 
                </execution> 
            </executions> 
        </plugin>

        <!--Plugin para ejecutar sentencias SQL-->
        <plugin> 
            <groupId>org.codehaus.mojo</groupId> 
            <artifactId>sql-maven-plugin</artifactId> 
            <version>1.5</version> 

            <!--Dependencia con el driver JDBC para acceder a una BBDD MySQL-->
            <dependencies>
                <dependency> 
                    <groupId>mysql</groupId> 
                    <artifactId>mysql-connector-java</artifactId> 
                    <version>5.1.38</version> 
                </dependency>
            </dependencies>

            <!--Configuración del driver JDBC-->
            <configuration> 
                <driver>com.mysql.jdbc.Driver</driver> 
                <url>jdbc:mysql://localhost:3306</url>
                <username>root</username> 
                <password>ppss</password> 
            </configuration> 

            <!--inicialización de la tabla customer, antes ejecutar los test de integración-->
            <executions> 
                <execution>  
                    <id>create-customer-table</id> 
                    <phase>pre-integration-test</phase> 
                    <goals> 
                        <goal>execute</goal>  
                    </goals>  
                    <configuration> 
                        <srcFiles>
                            <srcFile>src/test/resources/sql/create_table_customer.sql</srcFile> 
                        </srcFiles> 
                    </configuration> 
                </execution>
            </executions> 
        </plugin>
        
    </plugins>
</build>
```

# Ejercicios

#### Supongamos que tenemos que implementar tests para el método `void addAlumno(Alumnoalu)`, que añade la información del objeto Alumno en una base de datos. Contesta a lassiguientes preguntas, justificando claramente tus respuestas:

#### a) Justifica por qué podemos realizar pruebas de integración sobre dicho método.

Porque el método lo único que hace es introducir un alumno en la bbdd, con lo que integramos el método con la bbdd. Estamos integrando el método con la bbdd, con lo que ya son 2 unidades, una de nuestro código fuente y otra que es la bbdd.

---

#### b) Indica qué librerías, de las vistas en clase, necesitarías para implementar dichos tests deintegración y cómo configurarías el pom del proyecto con dichas librerías. Justificaclaramente para qué necesitas cada una de las librerías.

Driver mysql, driver junit, librería mysql, librería dbunit. SOLO LIBRERIAS, no plugins.

---

#### c) Muestra todo el código necesario para implementar un test de integración suponiendo queel objeto Alumno está formado por los atributos id (de tipo int), nombre, dirección, estos dos últimos de tipo String, y que la base de datos contiene la tabla alumno, con los camposalu_id, alu_nombre, alu_direccion. El método añade los datos del alumno en la citada tabla. Supón también que la tabla alumno inicialmente está vacía.

Todo el código necesario, los tests y el xml con el dataset inicial y el xml con el dataset final.
Puede preguntar dónde va cada fichero.
