# Pruebas de aceptación 2

Lo malo de **Katalon** es que no soporta:

* Bucles
* Condiciones
* Gestión de errores
* Tests en la costrucción de proyecto

## Selenium WebDriver

Por ello se puede usar **Selenium WebDriver** que usa **java** (entre otros)
para programar los tests de **pruebas emergentes funcionales** (basadas en escenarios normalmente)
sobre **aplicaciones web** en diferentes navegadores.

WebDriver usa un driver concreto según el navegador que estemos usando

```java
WebDriver driver = new FirefoxDriver();
driver.get("http://www.google.com");
```

Una página web está formada por elementos HTML, que se denominan **WebElements** en el contexto de WebDriver.

```java
WebElement searchBox = driver.findElement(By.name("q"));
```
> Formas de localizar:
> * `By.name("username")`
> * `By.id("loginForm")`
> * `By.className("required")`
> * `By.linkText("Register here")`
> * `By.partialLinkText("here")`
> * `By.xpath("//table//tr[@class='heading3']")`
> * `By.cssSelector("input[value='Business']")`

Una vez localizado, podemos realizar acciones.

```java
searchBox.clear();
searchBox.sendKeys("Packt Publishing");
searchBox.submit();
```

Otras acciones comunes a todos los WenElements son:
* Ratón
  * `click()` Click donde se encuentre
  * `click(WebElement onElement)` Click a elemento
  * `doubleClick()`
  * `contextClick()`
* Teclado
  * `sendKeys(CharSequence keysToSend)`
  * `keyDown(Keys theKey)` (para Shift, Ctrl, Alt)
  * `keyUp(Keys theKey)` (para Shift, Ctrl, Alt)
* `getAttribute()`
* `getLocation()`
* `getText()`
* `isDisplayed()`
* `isEnabled()`
* `isSelected()`

> #### Acciones compuestas
>
> ```java
> Actions actions = new Actions(driver);
> actions.keyDown(Keys.CONTROL)
> 	.click(element1)
> 	.click(element2)
> 	.click(element3)
> 	.keyUp(Keys.CONTROL)
> 	.build()    // Build the composite action.
> 	.perform(); // Perform the composite action.
> ```

### Esperar

Si no esperamos un ratico a que se carge la página, es probable que no se cargue lo que buscamos
(y se lanze un `NoSuchElementException`)

**Wait implicito**: Esperar un tiempo prestablecido.

```java
driver.manage().timeouts().implicitlyWait(10,TimeUnit.SECONDS);
```

**Wait explícito**, Objeto `WebDriverWait`, con `until()` espera hasta que algoen concreto ocurra o aparezca.

```java
WebDriverWait wait = new WebDriverWait(driver, 10);
wait.until(ExpectedConditions.titleContains("selenium")); // Option a) Wait until title is updated
wait.until(ExpectedConditions.alertIsPresent());          // Option b) Wait until alert appears
```

> RECUERDA: Los waits van en el src, no en los tests

### Ejemplo de un test

```java
@Test
public void signIn()
{
  WebDriver driver = new FirefoxDriver();    // 1) WebDriver
  driver.get("https://accounts.google.com"); // 2) Obtener pág

  Assert.assertEquals("Inicio de sesión", driver.getTitle()); // Un primer assert

  WebElement username = driver.findElement(By.id("Email"));  // WebElement login
  username.clear();
  username.sendKeys("TestSelenium"); //tecleamos el usuario

  WebElement password = driver.findElement(By.id("Passwd")); // WebElement pass
  password.clear();
  password.sendKeys("password123"); //tecleamos el password

  WebElement SignInButton = driver.findElement(By.id("signIn")); // WebElement button
  SignInButton.click();
  
  driver.manage().timeouts().implicitlyWait(5, TimeUnit.SECONDS); // Una pausilla
  
  Assert.assertEquals("Bienvenido", driver.getTitle()); // Otro assert

  driver.close();                                       // Cerrar el navegador
}
```


### POM

```xml
<dependencies>
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
  </dependency>
  <dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>           <!-- todos los nav -->
    <artifactId>selenium-firefox-driver</artifactId> <!-- solo firefox -->
    <version>3.11.0</version>
  </dependency>
</dependencies>
```

Podríamos usar categorías y poner los tests en `src/test/java`
pero en el laboratorio hemos visto que los ponemos en un **proyecto aparte**,
ya que el fuente es una pagina web.

### Page Object Pattern

Ya que nuestros tests son muy dependientes de la la interfaz de la página web,
y un pequeño puto cambio nos puede joder todos los tests.
Es una buena práctica usar el patrón de **Page Object**.

Básicamente consiste en crear una clase para cada página web, donde:
* Los atributos serán los elementos de la página web.
* Los métodos serán todos los servicios que nos proporciona la página.

##### Ejemplo

Page objects en `src/main/java`

```java
public class LoginPage
{
	WebDriver  driver;

	WebElement userID;
	WebElement password;
	WebElement login;
	WebElement loginTitle;

	public LoginPage(WebDriver driver)
	{
		this.driver = driver;
		this.driver.get("http://demo.guru99.com/V4");

		userID     = driver.findElement(By.name("uid"));
		password   = driver.findElement(By.name("password"));
		login      = driver.findElement(By.name("btnLogin"));
		loginTitle = driver.findElement(By.className("barone"));
	}
  
	public void login(String user, String pass)
	{
		userID.sendKeys(user);
		password.sendKeys(pass);
		login.click();
	}
  
	public String getLoginTitle()
	{
		return loginTitle.getText();
	}
}
```

Tests en `src/test/java`

```java
import org.junit.Test;
import org.junit.Assert;
import org.junit.Before;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.support.PageFactory;

public class TestLoginPage
{
	WebDriver driver;
	LoginPage poLogin;
	ManagerPage poManagerPage;
	
	@Before
	public void setup()
	{
		driver  = new FirefoxDriver();
		poLogin = new LoginPage(driver);
	}
	
	@Test
	public void test_Login_Correct()
	{
		String loginPageTitle = poLogin.getLoginTitle();
		Assert.assertTrue(loginPageTitle.toLowerCase().contains("guru99 bank"));
		
		poLogin.login("mngr34733", "AbEvydU");
		poManagerPage = new ManagerPage(driver);

		Assert.assertTrue(poManagerPage.getHomePageDashboardUserName()
				.toLowerCase().contains("manger id : mngr34733"));

		driver.close();
	}
}
```


### PageObject con PageFactory (la forma buena)

```java
public class LoginPage
{
	WebDriver driver;
	
	@FindBy(name="uid")         WebElement userID;
	@FindBy(name="password")    WebElement password;
	@FindBy(name="btnLogin")    WebElement login;
	@FindBy(className="barone") WebElement loginTitle;

	public LoginPage(WebDriver driver)
	{
		this.driver = driver;
		this.driver.get("http://demo.guru99.com/V4");
	}
	
	public void login(String user, String pass)
	{
		userID.sendKeys(user);
		password.sendKeys(pass);
		login.click();
	}
	
	public String getLoginTitle()
	{
		return loginTitle.getText();
	}
}
```

Test:

```java
import org.junit.Test;
import org.junit.Assert;
import org.junit.Before;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.support.PageFactory;

public class TestLoginPage
{
	WebDriver driver;
	LoginPage poLogin;
	ManagerPage poManagerPage;
	
	@Before
	public void setup()
	{
		driver = new FirefoxDriver();
		poLogin = PageFactory.initElements(driver, LoginPage.class);
	}
	
	@Test
	public void test_Login_Correct()
	{
		String loginPageTitle = poLogin.getLoginTitle();
		Assert.assertTrue(loginPageTitle.toLowerCase().contains("guru99 bank"));

		poLogin.login("mngr34733", "AbEvydU");
		poManagerPage = PageFactory.initElements(driver, ManagerPage.class);

		Assert.assertTrue(poManagerPage.getHomePageDashboardUserName()
			.toLowerCase().contains("manger id : mngr34733"));

		driver.close();
	}
}
```

# Ejercicios

#### a) Dados los tests implementados en prácticas con Katalon Recorder ¿Podríamos haber implementado esos mismos tests con Selenium WebDriver? Justifica claramente tu respuesta.

Sí, porque WebDriver añade mas lógica (bucles, condiciones) a la forma sequencial que tiene Katalon de hacer pruebas web.

---

#### b) ¿Podemos integrar la ejecución de los tests anteriores (que usan Selenium WebDriver) con la construcción del proyecto? Justifica claramente tu respuesta, indicando por qué no podemos hacerlo, si tu respuesta es negativa, y cómo podemos hacerlo si tu respuesta es afirmativa.

los tests de Selenium/katalon No se pueden integrar en el proyecto porque no es java.

Pero los tests de Selenium WebDriver si se podrían integrar pero en el laboratorio los hemos puesto en un proyecto aparte

---

#### c) Supón que accedemos a un sitio web (http://books_like.es/comics), que nos muestra la  sección de cómics, estamos interesados en leer el último número del comic de “Conan”. En esa página podemos leer cómics on-line, y además podemos ver el número de opiniones “me gusta” y “no me gusta” realizados de forma anónima por los usuarios que han leído el cómic. Cada vez que un usuario “pincha” sobre el icono “me gusta” o “no me gusta” (solamente puede activarse uno cada vez) se incrementa en uno el contador correspondiente..

##### Implementa con Webdriver (usando el patrón Page Object) un test en el que accedemos a http://books_like.es/comics, selecionamos el comic “Conan” y luego pulsamos sobre “Me gusta” (no es necesario que implementes los métodos de las page object, pero tienes que indicar para dichas clases, los atributos y las signaturas de los métodos que contienen).

![1](1.png)

```java
public class ComicsPage
{
	WebDriver driver;
	@FindBy(name="conan") WebElement conanBtn;

	public ComicsPage(WebDriver d)
	{
		driver = d;
		driver.get("http://books_like.es/comics");
	}

	public ComicPage clickComic(String nameComic)
	{
		...
	}
}
```

![2](2.png)

```java
public class ComicPage
{
	WebDriver driver;
	@FindBy(name="meGusta") WebElement meGustaBtn;
	@FindBy(name="noMeGusta") WebElement noMeGustaBtn;

	public ComicPage(WebDriver d)
	{
		driver = d;
	}

	public void clickMeGusta()
	{
		...
	}

	public void clickNoMeGusta()
	{
		...
	}
}
```

```java
public class TestComicsPage
{
	WebDriver driver;
	ComicsPage poComics; // principal page
	ComicPage poComic;   // comic page (Conan)

	@Before
	public void setup()
	{
		driver   = new FireFoxDriver();

		// Accedemos a http://books_like.es/comics
		poComics = PageFactory.initElements(driver, poComics.class);
	}

	@Test
	public void testProposed()
	{
		// Selecionamos el comic "Conan"
		poComic = poComics.clickComic("Conan");
		
		// Pulsamos sobre "Me gusta"
		poComic.clickMeGusta();
	}
}
```
