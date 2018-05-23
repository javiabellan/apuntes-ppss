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

---

# DbUnit
Basado en JUnit. Controla la dep. ext. de la BBDD

```xml
<dependency>
    <groupId>org.dbunit</groupId>
    <artifactId>dbunit</artifactId>
    <version>2.5.4</version>
    <scope>test</scope>
</dependency>
<dependency> 
    <groupId>mysql</groupId> 
    <artifactId>mysql-connector-java</artifactId> 
    <version>5.1.38</version> 
</dependency>
```

### Escenario típico

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
1. BBDD limpia antes de cada test (no después)
2. Cargar datos a la BBDD
3. Usar DbUnit para aserciones

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
private IDatabaseTester databaseTester;
    
@Before
public void setUp() throws Exception
{
    // Permite conexion con la BD
    databaseTester = new JdbcDatabaseTester("driver", "url", "login", "pass");

    // Datos de prueba
    DataFileLoader loader = new FlatXmlDataFileLoader();
    IDataSet dataSet = loader.load("/dataset.xml");	 

    databaseTester.setDataSet(dataSet);  // Determina los datos de prueba
    databaseTester.onSetup();            // Realiza una operación CLEAN_INSERT (inserta en la BBDD el dataSet)
}

@Test
public void testInsert() throws Exception
{
    Clase c = factoria.create(1,"Javi", "Abellan");
    c.setStreet("Calle falsa 123");
    c.setCity("Elda");
            
    factoria.insert(c);

    // Recuperamos la conexión con la BD
    IDatabaseConnection connection = databaseTester.getConnection();

    // Configuramos el tipo de la conexión como  mysql
    DatabaseConfig dbconfig = connection.getConfig();
    dbconfig.setProperty("http://www.dbunit.org/properties/datatypeFactory", new MySqlDataTypeFactory());

    // Recuperamos los datos de la BD (tablas)
    IDataSet databaseDataSet = connection.createDataSet();

    // Recuperamos una tabla
    ITable actualTable = databaseDataSet.getTable("customer");    
}
```
