# Pruebas de aceptación 2

Lo malo de **Katalon** es que no soporta:

* Bucles
* Condiciones
* Gestión de errores
* Tests en la costrucción de proyecto

Por ello se puede usar **WebDriver** que usa **java** (entre otros)
para programar los tests de **pruebas emergentes funcionales**
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
Lacalizar:
* `By.name("username")`
* `By.id("loginForm")`
* `By.className("required")`
* `By.linkText("Register here")`
* `By.partialLinkText("here")`
* `By.xpath("//table//tr[@class='heading3']")`
* `By.cssSelector("input[value='Business']")`

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

### Ejemplo de un test

```java
@Test
public void signIn()
{
  WebDriver driver = new FirefoxDriver();
  driver.get("https://accounts.google.com");
  driver.manage().window().maximize();

  String expectedTitle = "Inicio de sesión - Cuentas de Google";
  String actualTitle   = driver.getTitle();
  Assert.assertEquals("Url incorrecta", expectedTitle, actualTitle);

  WebElement username = driver.findElement(By.id("Email"));
  username.clear();
  username.sendKeys("TestSelenium"); //tecleamos el usuario

  WebElement password = driver.findElement(By.id("Passwd"));
  password.clear();
  password.sendKeys("password123"); //tecleamos el password

  WebElement SignInButton = driver.findElement(By.id("signIn"));
  SignInButton.click();
  
  expectedTitle = "Inicio de sesión - Cuentas de Google";
  actualTitle   = driver.getTitle();
  Assert.assertEquals("Sign In Fallido", expectedTitle,actualTitle);

  driver.close(); //cerramos el navegador
}
```

### Esperar

Si no esperamos un ratico a que se carge la página, es probable que no se cargue lo que buscamos
(y se lanze un `NoSuchElementException`)

**Wait implicito**: Esperar siempre un tiempo prestablecido para todas las operaciones del driver.

```java
driver.manage().timeouts().implicitlyWait(10,TimeUnit.SECONDS);
```

También puedes usar un **wait explícito**,
en cuyo caso afectará al elemento al que asociemos el temporizador.
El `until` espera hasta que aparezca un WebElement concreto:

```java
WebDriverWait wait = new WebDriverWait(driver, 10);
wait.until(ExpectedConditions.titleContains("selenium")); // Option a) Wait until title is updated
wait.until(ExpectedConditions.alertIsPresent());          // Option b) Wait until alert appears
```

> RECUERDA: Los waits van en el src, no en los tests

### POM

```xml
<dependencies>
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
  
	public String getPageTitle()
	{
		return loginTitle.getText();
	}
}
```

Tests en `src/test/java`

```java
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
